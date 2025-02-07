<template>
    <div>
        <h2>{{ isEditMode ? 'Редактировать Книгу' : 'Добавить Книгу' }}</h2>
        <!-- @submit.prevent="handleSubmit" - предотвращает обновления страницы -->
        <form @submit.prevent="handleSubmit">
            <div>
                <input placeholder="название книги" id="title" v-model="book.title" required/>
                <input placeholder="Автор" id="author" v-model="book.author" required/>
                <input placeholder="год издания" id="publishedYear" v-model.number="book.publishedYear" required/>
            </div>
            <div>
                <button type="submit">{{ isEditMode ? 'Сохранить' : 'Добавить' }}</button>
                <button type="button" @click="cancel">Отмена</button>
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

</style>