<template>
    <div class="book-form">
        <h2>{{ isEditMode ? 'Редактировать Книгу' : 'Добавить Книгу' }}</h2>
        <!-- @submit.prevent="handleSubmit" - предотвращает обновления страницы -->
        <form @submit.prevent="handleSubmit">
            <div class="inputs">
                <input class="input" placeholder="название книги" id="title" v-model="book.title" required/>
                <input class="input" placeholder="Автор" id="author" v-model="book.author" required/>
                <input class="input" placeholder="год издания" id="publishedYear" v-model.number="book.publishedYear" required/>
            </div>
            <div>
                <button class="btn" type="submit">{{ isEditMode ? 'Сохранить' : 'Добавить' }}</button>
                <button class="btn" type="button" @click="cancel">Отмена</button>
            </div>
        </form>
    </div>
</template>

<script>
import axios from '@/plugins/axios';

export default {
    props: {
        initialBook: {
            type: Object,
            default: () => ({
                title: '',
                author: '',
                publishedYear: null,
                isBorrowed: false
            })
        }
    },
    data() {
        return {
          book: { ...this.initialBook }
        };
    },
    computed: {
        isEditMode() {
          return this.book.id !== undefined;
        }
    },
    watch: {
        initialBook: {
          handler(newVal) {
            this.book = { ...newVal };
          },
          deep: true
        }
    },
    methods: {
        async handleSubmit() {
            try {
                if (this.isEditMode) {
                  await axios.put(`/books/${this.book.id}`, this.book);
                } else {
                  await axios.post('/books', this.book);
                }
                this.$emit('book-submitted');
            } catch (error) {
                console.error('Ошибка при сохранении книги:', error);
            }
        },
        cancel() {
            this.$emit('cancel');
        }
    }
}
</script>

<style>
.book-form {
    display: flex;
    padding: 15px;
    border: 0.8px solid rgb(220, 227, 235);
    border-radius: 24px;
    margin-top: 15px;
    font-family: Arial, Helvetica, sans-serif;
    align-items: center;
    flex-direction: column;
    justify-content: center;
    width: 50%;
}

.inputs {
    width: 100%;
}
</style>