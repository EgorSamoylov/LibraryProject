<template>
        <div class="book-item">
          <div class="book-content">
            <strong>{{ book.id }}. {{ book.title }}</strong>
            <div class="book-p">
              Автор: {{ book.author }}. <br/>
              Год: {{ book.publishedYear }} <br/>
              Статус: {{ book.borrowed }}
            </div>
          </div>
          <div class="book-buttons">
            <button class="btn" @click="editBook(book)">Редактировать</button>
            <button class="btn" @click="deleteBook(book.id)">Удалить</button>
            <button class="btn" @click="toggleBorrowStatus(book)">{{ book.borrowed ? 'Вернуть' : 'Взять' }}</button>
          </div>
        </div>
</template>

<script>
    export default {
        props: {
            book: {
                type: Object,
                default: () => ({
                    title: '',
                    author: '',
                    publishedYear: null,
                    borrowed: false
                })
            }
        },
        methods: {
            editBook() {
                this.$emit('editBook', this.book);
            },
            deleteBook() {
                this.$emit('deleteBook', this.book.id);
            },
            toggleBorrowStatus() {
                this.$emit('toggleBorrowStatus', this.book);
            }
        }        
    }
</script>

<style>
.book-item {
    display: flex;
    padding: 15px;
    border: 0.8px solid rgb(220, 227, 235);
    border-radius: 24px;
    margin-top: 15px;
    justify-content: space-between;
    font-family: Arial, Helvetica, sans-serif;
    width: 50%;
}

.book-item:hover {
  border: 0.8px solid rgb(42, 49, 55);
}

.book-content {
  display: flex;
  flex-direction: column;
  text-align: left;
  align-items: flex-start;
}

.book-p {
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>