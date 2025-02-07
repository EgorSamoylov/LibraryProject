<template>
  <div id="app">
    <h1>Библиотека</h1>
    <BookList/>
  </div>
</template>

<script>
import BookList from './components/BookList.vue';


export default {
  name: 'App',
  components: {
    BookList
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
