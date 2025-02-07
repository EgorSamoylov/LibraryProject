<template>
  <div id="app">
    <h1>Библиотека</h1>
    <BookList/>
    <BookForm
      :initialBook="editingBook"
      @book-submitted="bookSubmitted"
      @cancel="cancelEdit"/>
  </div>
</template>

<script>
import BookForm from './components/BookForm.vue';
import BookList from './components/BookList.vue';


export default {
  name: 'App',
  components: {
    BookList,
    BookForm
  },
  data() {
    return {
      editingBook: {} // пустая книга для создания или изменения через форму
    };
  },
  methods: {
    editBook(book) {
      this.editingBook = { ...book }; //копируем book
    },
    bookSubmitted() {
      this.editingBook = {};
      this.$children[0].fetchBooks(); // обновляем список книг
    },
    cancelEdit() {
      this.editingBook = {}; // очищаем editingBook
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: flex;
  justify-content: center;
  flex-direction: column;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
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
