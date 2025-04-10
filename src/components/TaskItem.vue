<template>
  <li :class="{ completed: task.completed }">
    <label class="checkbox-container">
      <input type="checkbox" :checked="task.completed" @change="updateTask(task.id)">
      <span class="checkmark"></span>
    </label>
    {{ task.title }}
  </li>
</template>

<style scoped>
li {
  display: flex;
  align-items: center;
  padding: 10px;
  background-color: #fff;
  border-radius: 4px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  margin-bottom: 8px;
}

.checkbox-container {
  display: inline-block;
  position: relative;
  padding-left: 35px;
  margin-right: 10px;
  margin-bottom: 12px;
  cursor: pointer;
  font-size: 22px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

.checkbox-container input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
  height: 0;
  width: 0;
}

.checkmark {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  left: 0;
  height: 25px;
  width: 25px;
  background-color: #eee;
  border-radius: 4px;
  border: 2px solid #e53935;
}

.checkbox-container:hover input~.checkmark {
  background-color: #ccc;
}

.checkbox-container input:checked~.checkmark {
  background-color: #e53935;
}

.checkmark:after {
  content: "";
  position: absolute;
  display: none;
}

.checkbox-container input:checked~.checkmark:after {
  display: block;
}

.checkbox-container .checkmark:after {
  left: 9px;
  top: 5px;
  width: 5px;
  height: 10px;
  border: solid white;
  border-width: 0 3px 3px 0;
  -webkit-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  transform: rotate(45deg);
}

.completed {
  color: #888;
  text-decoration: line-through;
}
</style>

<script setup>
import { defineProps, defineEmits } from 'vue';

defineProps({
  task: {
    type: Object,
    required: true
  }
});

const emit = defineEmits(['task-changed']);

function updateTask(taskId) {
  emit('task-changed', taskId);
}
</script>