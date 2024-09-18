<script setup>
import { defineProps, defineEmits, ref } from 'vue';

const props = defineProps({
  task: {
    type: Object,
    required: true
  }
});

const emit = defineEmits(['toggle-complete', 'delete-task', 'edit-task']);

const isEditing = ref(false);
const editedText = ref(props.task.text);

const resetText = () => {
  editedText.value = props.task.text;
};

const toggleComplete = () => {
  emit('toggle-complete', props.task.id);
};

const deleteTask = () => {
  emit('delete-task', props.task.id);
};

const startEditing = () => {
  isEditing.value = true;
  resetText();
};

const saveEdit = () => {
  if (editedText.value.trim()) {
    emit('edit-task', props.task.id, editedText.value.trim());
    isEditing.value = false;
  }
};

const cancelEdit = () => {
  isEditing.value = false;
  resetText();
};
</script>

<template>
  <li :class="{ completed: task.status === 'completed' }">
    <template v-if="!isEditing">
      <span @click="toggleComplete">{{ task.text }}</span>
      <div>
        <button @click="startEditing">編集</button>
        <button @click="deleteTask">削除</button>
      </div>
    </template>
    <template v-else>
      <input v-model="editedText" @keyup.enter="saveEdit" @keyup.esc="cancelEdit" />
      <div>
        <button @click="saveEdit">保存</button>
        <button @click="cancelEdit">キャンセル</button>
      </div>
    </template>
  </li>
</template>

<style scoped>
input {
  flex-grow: 1;
  padding: 0.25rem;
  margin-right: 0.5rem;
}

li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.5rem;
  margin-bottom: 0.5rem;
  background-color: #f8f8f8;
  border-radius: 4px;
}

.completed span {
  text-decoration: line-through;
  color: #888;
}

button {
  background-color: #ff4136;
  color: white;
  border: none;
  padding: 0.25rem 0.5rem;
  border-radius: 4px;
  cursor: pointer;
}
</style>
