<script setup>
import { ref, computed } from 'vue';
import TaskList from './components/TaskList.vue';
import TaskForm from './components/TaskForm.vue';

class Task {
  id;
  nom;
  estat;

  constructor(id, nom, estat) {
    this.id = id;
    this.nom = nom;
    this.estat = estat;
  }
}

const tasks = ref([]);
const taskName = ref('');
const viewCompleted = ref(false);

const createTask = (nom = taskName.value) => {
  const name = String(nom || '').trim();
  if (!name) return;

  const newTask = new Task(tasks.value.length + 1, name, 'pendent');
  tasks.value.push(newTask);
  taskName.value = '';
};

const deleteTask = (taskToDelete) => {
  const taskId = taskToDelete?.id ?? taskToDelete;
  tasks.value = tasks.value.filter(task => task.id !== taskId);
};

const totalTasks = computed(() => tasks.value.length);
const pendentTasks = computed(() => tasks.value.filter(task => task.estat === 'pendent').length);
const completedTaksOnly = computed(() => tasks.value.filter(task => task.estat === 'pendent'));

const toggleEstat = (taskId) => {
  const task = tasks.value.find(item => item.id === taskId);
  if (!task) return;

  task.estat = task.estat === 'pendent' ? 'completat' : 'pendent';
};

const visibleTasks = computed(() => {
  return viewCompleted.value ? completedTaksOnly.value : tasks.value;
});
</script>

<template>
  <main class="app">
    <section class="card">
      <h1>Gestor de Tasques</h1>

      <label class="label">Posa la tasca</label>
      <TaskForm v-model:task-name="taskName" @add-task="createTask" />

      <label class="check-row">
        <input v-model="viewCompleted" type="checkbox" />
        <span>Mostrar només pendents</span>
      </label>

      <TaskList :tasks="visibleTasks" @toggle-task="toggleEstat" @delete-task="deleteTask" />
    </section>

    <section class="account">
      <p>Total: {{ totalTasks }} | Pendets: {{ pendentTasks }}</p>
    </section>
  </main>
</template>

<style scoped>
:global(body) {
  margin: 0;
  font-family: Arial, sans-serif;
  background: #f3f4f6;
}

* {
  box-sizing: border-box;
}

.app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  padding: 24px;
}

.card {
  width: min(100%, 500px);
  background: #ffffff;
  border-radius: 16px;
  box-shadow: 0 8px 22px rgba(0, 0, 0, 0.08);
  padding: 24px;
}

h1 {
  margin: 0 0 20px;
  text-align: center;
  color: #1f2937;
}

.label {
  display: block;
  margin-bottom: 8px;
  color: #374151;
  font-weight: bold;
}

.check-row {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 18px;
  color: #4b5563;
}

.account {
  margin-top: 16px;
  text-align: center;
  color: #374151;
}

.account p {
  margin: 0;
}
</style>