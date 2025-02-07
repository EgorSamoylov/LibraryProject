<template>
  <div>
    <h2>Список книг</h2>
    <div>
      <input class="input" type="text" v-model="searchTitle" placeholder="Поиск по названию">
      <input class="input" type="text" v-model="searchAuthor" placeholder="Поиск по автору">
    </div>
    <div v-if="loading"></div>
    <div v-else>
      <div class="book-List" v-for="book in filteredBooks" :key="book.id">
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
            <button class="btn" @click="deleteBook(book.id)">Удалить</button>
            <button class="btn" @click="toggleBorrowStatus(book)">{{ book.borrowed ? 'Вернуть' : 'Взять' }}</button>
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

.book-List {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  width: 100%;

}

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
}

.book-p {
  display: flex;
  align-items: center;
  justify-content: center;
}

.book-buttons {
  display: flex;
  flex-direction: column;
}

/* Стили на UI компонентов */
.btn {
    padding: 5px 15px;
    color: white;
    background-color: teal;
    border: solid 2px teal;
    text-decoration-color:teal;
    transition-timing-function: cubic-bezier(0.25, 0.1, 0.25, 1);
    font-family: Arial, Helvetica, sans-serif;
    border-radius: 8px;
    margin: 5px 10px;
    height: 30px;
    cursor: pointer;
}

.btn:hover {
    background-color: rgb(0, 155, 155);
    border: solid 2px gb(0, 155, 155);
    color: white;
}

.input {
    width: 50%;
    padding: 5px 15px;
    margin: 5px 0px;
    border: 1px solid rgb(42, 49, 55);
    border-radius: 8px;
    box-shadow: rgb(220, 227, 235) 0px 0px 0px 1px;
    font-family: Arial, Helvetica, sans-serif;
    height: 20px;
}
</style>