
<script setup lang="ts">
import { Search } from "lucide-vue-next";

const searchQuery = defineModel<string>("searchQuery", { default: "" });
const filterStatus = defineModel<"all" | "read" | "unread">("filterStatus", {
    default: "all",
});
</script>


<template>
    <div class="controls">
        <div class="search-wrapper">
            <Search :size="18" class="search-icon" />
            <input
                v-model="searchQuery"
                placeholder="Поиск по названию или автору..."
                class="search-input"
            />
        </div>

        <div class="filters">
            <label
                ><input type="radio" v-model="filterStatus" value="all" />
                Все</label
            >
            <label
                ><input type="radio" v-model="filterStatus" value="unread" />
                Непрочитанные</label
            >
            <label
                ><input type="radio" v-model="filterStatus" value="read" />
                Прочитанные</label
            >
        </div>
    </div>
</template>

<style scoped>
.controls {
    display: flex;
    flex-direction: column;
    gap: 15px;
    padding: 15px;
    border: 1px solid #ddd;
    border-radius: 8px;
    margin-bottom: 20px;
}

.search-wrapper {
    position: relative;
    display: flex;
    align-items: center;
}

.search-icon {
    position: absolute;
    left: 10px;
    color: #888;
}

.search-input {
    width: 100%;
    padding: 8px 8px 8px 35px; /* Отступ слева для иконки */
    border: 1px solid #ccc;
    border-radius: 4px;
    box-sizing: border-box;
}

.filters {
    display: flex;
    gap: 15px;
}
</style>
