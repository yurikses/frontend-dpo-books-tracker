<template>
    <form @submit.prevent="submitForm" class="add-form">
        <h3>Добавить новую книгу</h3>
        <input
            v-model.trim="form.title"
            placeholder="Название книги"
            required
        />
        <input v-model.trim="form.author" placeholder="Автор" required />
        <input v-model.trim="form.genre" placeholder="Жанр" required />
        <button type="submit">
            <PlusCircle :size="18" />
            Добавить
        </button>
    </form>
</template>

<script setup lang="ts">
import { ref } from "vue";
import { PlusCircle } from "lucide-vue-next";

const emit = defineEmits<{
    (
        e: "add",
        bookData: { title: string; author: string; genre: string },
    ): void;
}>();

const form = ref({ title: "", author: "", genre: "" });

const submitForm = () => {
    if (!form.value.title || !form.value.author || !form.value.genre) return;
    emit("add", { ...form.value });
    form.value = { title: "", author: "", genre: "" };
};
</script>

<style scoped>
.add-form {
    display: flex;
    flex-direction: column;
    gap: 10px;
    padding: 15px;
    border: 1px solid #ddd;
    border-radius: 8px;
    margin-bottom: 20px;
}
input {
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 4px;
}
button {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    padding: 8px;
    background: #4caf50;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}
button:hover {
    background: #45a049;
}
</style>
