<script setup>
const props = defineProps({
  task: {
    type: Object,
    required: true
  }
});

const emit = defineEmits(['toggle-task', 'delete-task']);

const toggleTask = () => {
  emit('toggle-task', props.task.id);
};

const deleteTask = () => {
  emit('delete-task', props.task);
};
</script>

<template>
  <article class="view-tasks">
    <p :class="{ completed: task.estat === 'completat' }">{{ task.nom }}</p>
    <div class="task-actions">
      <button v-if="task.estat === 'pendent'" @click="toggleTask">Completar</button>
      <button v-else @click="toggleTask">Desmarcar</button>
      <button class="delete-btn" @click="deleteTask">🗑️</button>
    </div>
  </article>
</template>

<style scoped>
.view-tasks {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 10px;
}

.view-tasks p {
  margin: 0;
  flex: 1;
  color: #111827;
}

.task-actions {
  display: flex;
  align-items: center;
  gap: 8px;
}

.view-tasks button {
  background: #e0f2fe;
  color: #0f172a;
  border: none;
  border-radius: 10px;
  padding: 10px 12px;
  cursor: pointer;
}

.delete-btn {
  background: #fee2e2;
}

.completed {
  text-decoration: line-through;
  opacity: 0.6;
}
</style>
