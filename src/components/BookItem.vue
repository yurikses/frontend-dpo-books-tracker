<template>
    <li class="book-item" :class="{ read: book.isRead }">
        <div class="book-info">
            <h4>{{ book.title }}</h4>
            <p>{{ book.author }} ({{ book.genre }})</p>
        </div>

        <div class="book-actions">
            <label class="checkbox-label">
                <input type="checkbox" v-model="isReadLocal" /> Прочитано
            </label>

            <!-- Интерактивный рейтинг звездочками -->
            <div v-if="book.isRead" class="rating">
                <div class="stars">
                    <Star
                        v-for="n in 5"
                        :key="n"
                        :size="18"
                        :fill="
                            ratingLocal && ratingLocal >= n ? '#f59e0b' : 'none'
                        "
                        :color="
                            ratingLocal && ratingLocal >= n ? '#f59e0b' : '#ccc'
                        "
                        class="star-icon"
                        @click="ratingLocal = n"
                    />
                </div>
            </div>

            <button @click="$emit('delete', book.id)" class="delete-btn">
                <Trash2 :size="16" />
                Удалить
            </button>
        </div>
    </li>
</template>

<script setup lang="ts">
import { computed } from "vue";
import { Trash2, Star } from "lucide-vue-next";
import type { Book } from "../types";

const props = defineProps<{ book: Book }>();

const emit = defineEmits<{
    (e: "delete", id: string): void;
    (e: "update:isRead", id: string, value: boolean): void;
    (e: "update:rating", id: string, value: number | null): void;
}>();

const isReadLocal = computed({
    get: () => props.book.isRead,
    set: (val) => emit("update:isRead", props.book.id, val),
});

const ratingLocal = computed({
    get: () => props.book.rating,
    set: (val) => emit("update:rating", props.book.id, val),
});
</script>

<style scoped>
.book-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 15px;
    border: 1px solid #eee;
    border-radius: 8px;
    margin-bottom: 10px;
}
.book-item.read {
    background-color: #f9fff9;
    border-color: #c3e6c3;
}
.book-info h4 {
    margin: 0 0 5px 0;
}
.book-info p {
    margin: 0;
    color: #666;
    font-size: 0.9em;
}
.book-actions {
    display: flex;
    flex-direction: column;
    align-items: flex-end;
    gap: 10px;
}

.checkbox-label {
    cursor: pointer;
    display: flex;
    align-items: center;
    gap: 5px;
}

.rating {
    display: flex;
    align-items: center;
    gap: 8px;
    font-size: 0.9em;
    color: #555;
}
.stars {
    display: flex;
    gap: 2px;
}
.star-icon {
    cursor: pointer;
    transition: transform 0.1s;
}
.star-icon:hover {
    transform: scale(1.1);
}

.delete-btn {
    display: flex;
    align-items: center;
    gap: 5px;
    background: #ff4d4f;
    color: white;
    border: none;
    padding: 5px 10px;
    border-radius: 4px;
    cursor: pointer;
}
.delete-btn:hover {
    background: #d9363e;
}
</style>
