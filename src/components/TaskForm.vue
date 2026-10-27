<script setup>
import { computed } from 'vue';

const props = defineProps({
  taskName: {
    type: String,
    default: ''
  }
});

const emit = defineEmits(['add-task', 'update:taskName']);

const inputValue = computed({
  get: () => props.taskName,
  set: (value) => emit('update:taskName', value)
});

const submitTask = () => {
  const name = String(inputValue.value || '').trim();
  if (!name) return;

  emit('add-task', name);
  emit('update:taskName', '');
};
</script>

<template>
  <div class="input-row">
    <input v-model="inputValue" type="text" placeholder="Posa el nom de la tasca" @keyup.enter="submitTask" />
    <button class="add-btn" @click="submitTask">Afegir</button>
  </div>
</template>

<style scoped>
.input-row {
  display: flex;
  gap: 10px;
  margin-bottom: 16px;
}

input[type='text'] {
  flex: 1;
  border: 1px solid #d1d5db;
  border-radius: 10px;
  padding: 10px 12px;
}

.add-btn {
  border: none;
  border-radius: 10px;
  padding: 10px 12px;
  cursor: pointer;
  background: #4f46e5;
  color: white;
  font-weight: bold;
}
</style>
