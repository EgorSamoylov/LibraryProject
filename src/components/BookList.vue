<template>
  <div>
    <h2>Список книг</h2>
    <div>
      <input type="text" v-model="searchTitle" placeholder="Поиск по названию">
      <input type="text" v-model="searchAuthor" placeholder="Поиск по автору">
    </div>
    <div class="book-List">
      <div v-for="book in filteredBooks" :key="book.id">
        <div>
          <div class="book-content">
            <h3>{{ book.id }}. {{ book.title }}</h3>
            <p>
              Автор: {{ book.author }}. <br/>
              Год: {{ book.publishedYear }} <br/>
              Статус: {{ book.isBorrowed }}
            </p>
          </div>
          <div class="book-buttons">
            Здесь будут кнопки
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        books: [
          {id: 1, title: "Book title 1", author: "Book author 1", publishedYear: 1997, isBorrowed: true},
          {id: 2, title: "Book title 2", author: "Book author 2", publishedYear: 1999, isBorrowed: false},
          {id: 3, title: "Book title 3", author: "Book author 3", publishedYear: 1990, isBorrowed: true},
        ],
        searchTitle: '',
        searchAuthor: ''
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
  }
</script>

<style>

</style>