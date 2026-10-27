<script setup>
import TaskItem from './TaskItem.vue';

const props = defineProps({
  tasks: {
    type: Array,
    default: () => []
  }
});

const emit = defineEmits(['toggle-task', 'delete-task']);

const toggleTask = (taskId) => {
  emit('toggle-task', taskId);
};

const deleteTask = (task) => {
  emit('delete-task', task);
};
</script>

<template>
  <div class="tasks">
    <div v-for="task in tasks" :key="task.id" class="task-item">
      <TaskItem :task="task" @toggle-task="toggleTask" @delete-task="deleteTask" />
    </div>
  </div>
</template>

<style scoped>
.tasks {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.task-item {
  background: #f9fafb;
  border: 1px solid #e5e7eb;
  border-radius: 12px;
  padding: 8px 12px;
}
</style>
