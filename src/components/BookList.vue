<template>
  <div>
    <h2>Список книг</h2>
    <div>
      <input class="input" type="text" v-model="searchTitle" placeholder="Поиск по названию">
      <input class="input" type="text" v-model="searchAuthor" placeholder="Поиск по автору">
    </div>
    <div v-if="loading">
      <strong>Загрузка....</strong>
    </div>
    <div v-else>
      <div v-if="filteredBooks.length == 0">Ничего не найдено</div>
      <div class="book-List" v-for="book in filteredBooks" :key="book.id">
        <BookItem 
          :book="book"
          @editBook="editBook"
          @deleteBook="deleteBook"
          @toggleBorrowStatus="toggleBorrowStatus"/>
      </div>
    </div>
  </div>
</template>

<script>
import axios from '@/plugins/axios';
import BookItem from './BookItem.vue';

  export default {
    components: {
      BookItem
    },
    data() {
      return {
        books: [],
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
      },
      editBook(book) {
        this.$emit('edit-book', book)
      }
    },
    mounted() {
      this.fetchBooks();
    },
  }
</script>

<style>

.book-List {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  width: 100%;

}

.book-buttons {
  display: flex;
  flex-direction: column;
}
</style>