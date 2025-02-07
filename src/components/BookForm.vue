<template>
    <div>
        <h2>{{ isEditMode ? 'Редактировать Книгу' : 'Добавить Книгу' }}</h2>
        <!-- @submit.prevent="handleSubmit" - предотвращает обновления страницы -->
        <form @submit.prevent="handleSubmit">
            <div>
                <input placeholder="название книги"/>
                <input placeholder="Автор"/>
                <input placeholder="год издания"/>
            </div>
            <div>
                <button type="submit">{{ isEditMode ? 'Сохранить' : 'Добавить' }}</button>
                <button type="button">Отмена</button>
            </div>
        </form>
    </div>
</template>

<script>
export default {
    data() {
        return {
          book: { id: 1, title: "Book title 1", author: "Book author 1", publishedYear: 1997, borrowed: true }
        };
    },
    computed: {
        isEditMode() {
          return this.book.id !== undefined;
        }
    }
}
</script>

<style>

</style>