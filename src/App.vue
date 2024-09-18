<script setup>
import { ref, computed } from 'vue';
import TaskForm from './components/TaskForm.vue';
import TaskItem from './components/TaskItem.vue';

const tasks = ref([]);
const filter = ref('all');

const editTask = (taskId, newText) => {
  const task = tasks.value.find((t) => t.id === taskId);
  if (task) {
    task.text = newText;
  }
};

const filteredTasks = computed(() => {
  switch (filter.value) {
    case 'todo':
      return tasks.value.filter((task) => task.status === 'todo');
    case 'completed':
      return tasks.value.filter((task) => task.status === 'completed');
    default:
      return tasks.value;
  }
});

const addTask = (newTask) => {
  tasks.value.push({
    id: Date.now(),
    text: newTask,
    status: 'todo'
  });
};

// なんでこれでtasks Stateの方も変えれるんだ？
const toggleComplete = (taskId) => {
  const task = tasks.value.find((t) => t.id === taskId);
  if (task) {
    if (task.status === 'todo') {
      task.status = 'completed';
    } else if (task.status === 'completed') {
      task.status = 'todo';
    }
  }
};

const deleteTask = (taskId) => {
  tasks.value = tasks.value.filter((t) => t.id !== taskId);
};

const changeFilter = (newFilter) => {
  filter.value = newFilter;
};
</script>

<template>
  <div class="app">
    <h1>タスク管理アプリ</h1>
    <TaskForm @add-task="addTask" />
    <div class="filters">
      <button @click="changeFilter('all')" :class="{ active: filter === 'all' }">all</button>
      <button @click="changeFilter('todo')" :class="{ active: filter === 'todo' }">未完了</button>
      <button @click="changeFilter('completed')" :class="{ active: filter === 'completed' }">
        完了済み
      </button>
    </div>
    <ul>
      <TaskItem
        v-for="task in filteredTasks"
        v-bind:key="task.id"
        v-bind:task="task"
        v-on:toggle-complete="toggleComplete"
        v-on:delete-task="deleteTask"
        v-on:edit-task="editTask"
      ></TaskItem>
    </ul>
  </div>
</template>

<style scoped>
.app {
  max-width: 500px;
  margin: 0 auto;
  padding: 1rem;
}

ul {
  list-style-type: none;
  padding: 0;
}

.filters {
  margin-bottom: 1rem;
}

.filters button {
  margin-right: 0.5rem;
  padding: 0.25rem 0.5rem;
  border: 1px solid #ccc;
  background-color: white;
  cursor: pointer;
}

.filters button.active {
  background-color: #007bff;
  color: white;
}
</style>
