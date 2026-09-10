<script setup lang="ts">
import { onMounted, ref, watch } from 'vue'

type Task = {
  id: number
  text: string
  completed: boolean
}

const STORAGE_KEY = 'todo-tasks'

const newTaskText = ref('')
const tasks = ref<Task[]>([])

function isTask(value: unknown): value is Task {
  if (typeof value !== 'object' || value === null) return false

  const task = value as Record<string, unknown>

  return typeof task.id === 'number' && typeof task.text === 'string' && typeof task.completed === 'boolean'
}

function loadTasks() {
  try {
    const savedTasks = localStorage.getItem(STORAGE_KEY)

    if (!savedTasks) return

    const parsedTasks: unknown = JSON.parse(savedTasks)

    if (Array.isArray(parsedTasks) && parsedTasks.every(isTask)) {
      tasks.value = parsedTasks
    }
  } catch {
    tasks.value = []
  }
}

onMounted(loadTasks)

watch(
  tasks,
  (updatedTasks) => {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(updatedTasks))
  },
  { deep: true },
)

function addTask() {
  const text = newTaskText.value.trim()

  if (!text) return

  tasks.value.push({
    id: Date.now(),
    text,
    completed: false,
  })
  newTaskText.value = ''
}

function removeTask(id: number) {
  tasks.value = tasks.value.filter((task) => task.id !== id)
}
</script>

<template>
  <main class="task-app">
    <section class="task-card" aria-labelledby="app-title">
      <h1 id="app-title">Список задач</h1>

      <form class="task-form" @submit.prevent="addTask">
        <label class="visually-hidden" for="new-task">Новая задача</label>
        <input
          id="new-task"
          v-model="newTaskText"
          type="text"
          placeholder="Например, изучить Vue"
          autocomplete="off"
        />
        <button type="submit">Добавить</button>
      </form>

      <p v-if="tasks.length === 0" class="empty-message">Задач пока нет. Добавьте первую!</p>

      <ul v-else class="task-list">
        <li v-for="task in tasks" :key="task.id" class="task-item">
          <label class="task-label">
            <input v-model="task.completed" type="checkbox" />
            <span :class="{ completed: task.completed }">{{ task.text }}</span>
          </label>
          <button class="delete-button" type="button" @click="removeTask(task.id)">Удалить</button>
        </li>
      </ul>
    </section>
  </main>
</template>

<style scoped>
:global(*) {
  box-sizing: border-box;
}

:global(body) {
  margin: 0;
  min-width: 320px;
  background: #f4f7fb;
  color: #1f2937;
  font-family: Arial, sans-serif;
}

.task-app {
  display: grid;
  min-height: 100vh;
  padding: 24px;
  place-items: center;
}

.task-card {
  width: min(100%, 560px);
  padding: 32px;
  border-radius: 16px;
  background: #fff;
  box-shadow: 0 12px 30px rgb(31 41 55 / 12%);
}

h1 {
  margin: 0 0 24px;
  font-size: 28px;
}

.task-form {
  display: flex;
  gap: 12px;
}

.task-form input {
  min-width: 0;
  flex: 1;
  padding: 12px;
  border: 1px solid #cbd5e1;
  border-radius: 8px;
  font: inherit;
}

button {
  padding: 12px 16px;
  border: 0;
  border-radius: 8px;
  background: #2563eb;
  color: #fff;
  cursor: pointer;
  font: inherit;
}

button:hover {
  background: #1d4ed8;
}

.task-list {
  display: grid;
  gap: 12px;
  margin: 24px 0 0;
  padding: 0;
  list-style: none;
}

.task-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 16px;
  padding: 12px;
  border-radius: 8px;
  background: #f8fafc;
}

.task-label {
  display: flex;
  align-items: center;
  gap: 10px;
  min-width: 0;
}

.task-label span {
  overflow-wrap: anywhere;
}

.completed {
  color: #64748b;
  text-decoration: line-through;
}

.delete-button {
  flex-shrink: 0;
  background: #dc2626;
}

.delete-button:hover {
  background: #b91c1c;
}

.empty-message {
  margin: 24px 0 0;
  color: #64748b;
}

.visually-hidden {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}

@media (max-width: 480px) {
  .task-card {
    padding: 24px;
  }

  .task-form {
    flex-direction: column;
  }
}
</style>
