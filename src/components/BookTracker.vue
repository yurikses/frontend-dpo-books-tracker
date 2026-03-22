<template>
    <div class="book-tracker">
        <h1 class="main-title">
            <Library :size="32" class="title-icon" />
            Моя библиотека
        </h1>

        <div class="stats">
            <p>
                <BookOpen :size="18" /> Всего книг:
                <strong>{{ totalBooks }}</strong>
            </p>
            <p>
                <CheckCircle :size="18" /> Прочитано:
                <strong>{{ readBooksCount }}</strong>
            </p>
        </div>

        <AddBookForm @add="handleAddBook" />

        <SearchPanel
            v-model:searchQuery="searchQuery"
            v-model:filterStatus="filterStatus"
        />

        <ul class="book-list">
            <BookItem
                v-for="book in filteredAndSearchedBooks"
                :key="book.id"
                :book="book"
                @delete="handleDeleteBook"
                @update:isRead="handleUpdateRead"
                @update:rating="handleUpdateRating"
            />

            <p v-if="filteredAndSearchedBooks.length === 0" class="empty-state">
                <Library :size="48" color="#ccc" />
                <br />
                Книги не найдены.
            </p>
        </ul>
    </div>
</template>

<script setup lang="ts">
import { ref, computed, watch, onMounted } from "vue";
import { Library, BookOpen, CheckCircle } from "lucide-vue-next";
import type { Book } from "../types";

import AddBookForm from "./AddBookForm.vue";
import SearchPanel from "./SearchPanel.vue";
import BookItem from "./BookItem.vue";

const books = ref<Book[]>([]);
const searchQuery = ref("");
const filterStatus = ref<"all" | "read" | "unread">("all");

onMounted(() => {
    const savedBooks = localStorage.getItem("my-library-books");
    if (savedBooks) books.value = JSON.parse(savedBooks);
});

watch(
    books,
    (newVal) => {
        localStorage.setItem("my-library-books", JSON.stringify(newVal));
    },
    { deep: true },
);

const totalBooks = computed(() => books.value.length);
const readBooksCount = computed(
    () => books.value.filter((b) => b.isRead).length,
);

const filteredAndSearchedBooks = computed(() => {
    let result = books.value;

    if (filterStatus.value === "read") result = result.filter((b) => b.isRead);
    else if (filterStatus.value === "unread")
        result = result.filter((b) => !b.isRead);

    if (searchQuery.value) {
        const query = searchQuery.value.toLowerCase();
        result = result.filter(
            (b) =>
                b.title.toLowerCase().includes(query) ||
                b.author.toLowerCase().includes(query),
        );
    }
    return result;
});

const handleAddBook = (bookData: {
    title: string;
    author: string;
    genre: string;
}) => {
    books.value.unshift({
        // unshift добавляет новую книгу в начало списка, а не в конец
        id: crypto.randomUUID(),
        ...bookData,
        isRead: false,
        rating: null,
    });
};

const handleDeleteBook = (id: string) => {
    books.value = books.value.filter((b) => b.id !== id);
};

const handleUpdateRead = (id: string, isRead: boolean) => {
    const book = books.value.find((b) => b.id === id);
    if (book) book.isRead = isRead;
};

const handleUpdateRating = (id: string, rating: number | null) => {
    const book = books.value.find((b) => b.id === id);
    if (book) book.rating = rating;
};
</script>

<style scoped>
.book-tracker {
    max-width: 600px;
    margin: 0 auto;
    font-family: sans-serif;
}
.main-title {
    display: flex;
    align-items: center;
    gap: 10px;
    color: #333;
}
.title-icon {
    color: #4caf50;
}

.stats {
    display: flex;
    gap: 20px;
    background: #f0f4f8;
    padding: 15px;
    border-radius: 8px;
    margin-bottom: 20px;
}
.stats p {
    display: flex;
    align-items: center;
    gap: 8px;
    margin: 0;
}

.book-list {
    list-style: none;
    padding: 0;
}
.empty-state {
    text-align: center;
    color: #888;
    padding: 40px 0;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 10px;
}
</style>
