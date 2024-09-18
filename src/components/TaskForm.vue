<script setup>
import { ref } from 'vue';

const newTask = ref('');
const emit = defineEmits(['add-task']);
console.log('newTask', newTask);

const handleSubmit = () => {
  // ここのtrimがよくわからない
  if (newTask.value.trim()) {
    console.log('newTask.value', newTask.value);
    // 親で定義されたadd-taskイベントを発火
    emit('add-task', newTask.value.trim());
    newTask.value = '';
  }
};
</script>

<template>
  <form @submit.prevent="handleSubmit">
    <input v-model="newTask" type="text" placeholder="新しいタスクを入力" />
    <button type="submit">追加</button>
  </form>
</template>

<style scoped>
form {
  display: flex;
  margin-bottom: 1rem;
}

input {
  flex-grow: 1;
  padding: 0.5rem;
  font-size: 1rem;
}

button {
  padding: 0.5rem 1rem;
  font-size: 1rem;
  background-color: #4caf50;
  color: white;
  border: none;
  cursor: pointer;
}
</style>
