<template>
  <div id="app">
    <h1>Библиотека</h1>
    <BookList/>
    <BookForm/>
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
</style>
