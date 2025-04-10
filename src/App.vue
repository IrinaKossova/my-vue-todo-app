<template>
    <TaskFilter :filter="filter" :tasks="tasks" @filter-changed="setFilter" />
    <ul>
        <li v-for="task in filteredTasks" :key="task.id">
            <TaskItem :task="task" @task-changed="updateTask" />
        </li>
    </ul>
    <p v-bind:class="{ active: error }" v-if="error">{{ error }}</p>
</template>

<style>
body {
    font-family: sans-serif;
    background-color: #f0f0f0;
    color: black;
    font-weight: bold;
}

ul {
    list-style: none;
    padding: 0;
}

li {
    margin-bottom: 8px;
    padding: 10px;
    background-color: #fff;
    border-radius: 4px;
    box-shadow: 0 2px 4px #e53935;
}

.active {
    color: #e53935;
    font-weight: bold;
}
</style>

<script>
import { ref, onMounted, computed, watch } from 'vue';
import TaskItem from './components/TaskItem.vue';
import TaskFilter from './components/TaskFilter.vue';

export default {
    components: {
        TaskItem,
        TaskFilter
    },
    setup() {
        const tasks = ref([]);
        const error = ref('');
        const filter = ref('Все');
        const isLoadAttempted = ref(false); // Флаг, чтобы избежать повторной загрузки задач

        const filteredTasks = computed(() => {
            return filter.value === 'Выполненные'
                ? tasks.value.filter((task) => task.completed)
                : filter.value === 'Невыполненные'
                    ? tasks.value.filter((task) => !task.completed)
                    : tasks.value;
        });

        async function fetchTasksFromJson() {
            error.value = '';
            let response = await fetch('/tasks.json');
            return await response.json();
        }

        function saveTasksToLocalStorage() {
            localStorage.setItem('tasks', JSON.stringify(tasks.value));
        }

        async function loadTasks() {
            try {
                const data = await fetchTasksFromJson();
                tasks.value = data;
                isLoadAttempted.value = true;
            } catch (e) {
                error.value += `Ошибка при загрузке задач, ${e.message} \n`;
            }
        }

        function updateTask(id) {
            tasks.value = tasks.value.map(task => {
                if (task.id === id) {
                    return { ...task, completed: !task.completed };
                }
                return task;
            });
        }

        function setFilter(filterName) {
            filter.value = filterName;
        }

        onMounted(() => {
            if (!isLoadAttempted.value) {
                const storedTasks = localStorage.getItem('tasks');

                if (storedTasks) {
                    try {
                        tasks.value = JSON.parse(storedTasks);
                    } catch (parseError) {
                        error.value = 'Ошибка при загрузке данных\n';
                        console.error("Ошибка парсинга localStorage:", parseError);
                    }
                } else {
                    loadTasks();
                    error.value = 'Ошибка при загрузке данных\n';
                }

                isLoadAttempted.value = true;
            }
        });

        watch(() => tasks.value, saveTasksToLocalStorage);

        return {
            tasks,
            error,
            filter,
            filteredTasks,
            updateTask,
            setFilter,
        };
    },
};
</script>