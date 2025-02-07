<template>
  <div>
    <h2>Список книг</h2>
    <div>
      <input type="text" v-model="searchTitle" placeholder="Поиск по названию">
      <input type="text" v-model="searchAuthor" placeholder="Поиск по автору">
    </div>
    <div v-if="loading"></div>
    <div v-else class="book-List">
      <div v-for="book in filteredBooks" :key="book.id">
        <div>
          <div class="book-content">
            <h3>{{ book.id }}. {{ book.title }}</h3>
            <p>
              Автор: {{ book.author }}. <br/>
              Год: {{ book.publishedYear }} <br/>
              Статус: {{ book.borrowed }}
            </p>
          </div>
          <div class="book-buttons">
            <button @click="deleteBook(book.id)">Удалить</button>
            <button @click="toggleBorrowStatus(book)">{{ book.borrowed ? 'Вернуть' : 'Взять' }}</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from '@/plugins/axios';

  export default {
    data() {
      return {
        books: [
          {id: 1, title: "Book title 1", author: "Book author 1", publishedYear: 1997, borrowed: true},
          {id: 2, title: "Book title 2", author: "Book author 2", publishedYear: 1999, borrowed: false},
          {id: 3, title: "Book title 3", author: "Book author 3", publishedYear: 1990, borrowed: true},
        ],
        searchTitle: '',
        searchAuthor: '',
        loading: false
      };
    },
    computed: {
      filteredBooks() {
        return this.books.filter(book => {
          const titleMatch = book.title.toLowerCase().includes(this.searchTitle.toLowerCase());
          const authorMatch = book.author.toLowerCase().includes(this.searchAuthor.toLowerCase());
          return titleMatch && authorMatch;
        });
      }
    },
    methods: {
      async fetchBooks() {
        this.loading = true;
        try {
          const response = await axios.get('/books');
          this.books = response.data;
        } catch (error) {
          console.error('Ошибка при загрузке книг:', error);
        } finally {
          this.loading = false;
        }
      },
      async deleteBook(id) {
        try {
          await axios.delete(`/books/${id}`);
          this.fetchBooks(); // обновление списка после удаления
        } catch (error) {
          console.error('Ошибка при удалении книги:', error);
        }
      },
      async toggleBorrowStatus(book) {
        try {
          const endpoint = book.borrowed ? 'return' : 'borrow';
          await axios.put(`/books/${book.id}/${endpoint}`);
          this.fetchBooks(); // обновление списка после изменения статуса книги
        } catch (error) {
          console.error('Ошибка при изменении статуса книги:', error);
        }
      }
    },
    mounted() {
      this.fetchBooks();
    },
  }
</script>

<style>

</style>