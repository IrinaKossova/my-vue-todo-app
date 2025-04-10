<template>
    <div class="filter-buttons">
        <button @click="setFilter(all)" :class="{ active: filter === all }">
            {{ all }}
        </button>
        <button @click="setFilter(completed)" :class="{ active: filter === completed }">
            {{ completed }}
        </button>
        <button @click="setFilter(uncompleted)" :class="{ active: filter === uncompleted }">
            {{ uncompleted }}
        </button>
    </div>
</template>

<style scoped>
.filter-buttons {
    display: flex;
    gap: 10px;
    margin-bottom: 15px;
    flex-wrap: wrap;
}

button {
    background-color: #fff;
    border: 2px solid #e53935;
    padding: 8px 16px;
    border-radius: 4px;
    cursor: pointer;
    font-size: 14px;
    font-weight: bold;
    color: black;
    transition: background-color 0.3s ease, border-color 0.3s ease;
}

button:hover {
    background-color: #E3F2FD;
}

button.active {
    background-color: #1976D2;
    color: white;
}
</style>

<script setup>
import { defineEmits, defineExpose, defineProps } from 'vue';

const all = "Все";
const completed = "Выполненные";
const uncompleted = "Невыполненные";

defineProps({
    filter: String,
    tasks: Array
});

const emit = defineEmits(['filter-changed']);

function setFilter(filterName) {
    emit('filter-changed', filterName);
}

defineExpose({
    all,
    completed,
    uncompleted
});
</script>