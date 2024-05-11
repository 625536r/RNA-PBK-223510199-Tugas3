<template>
  <div class="container">
    <h2>To-Do List</h2>
    <form @submit.prevent="addTodo" class="form-group">
      <input v-model="newTodo" required placeholder="New todo" class="todo-input">
      <button class="todo-btn">Add Todo</button>
    </form>
    <ul class="todo-list">
      <li v-for="todo in filteredTodos" :key="todo.id" class="todo-item">
        <input type="checkbox" v-model="todo.done" class="todo-checkbox">
        <span :class="{ done: todo.done }" class="todo-text">{{ todo.text }}</span>
        <button @click="editTodo(todo)" class="btn edit-btn">Edit</button>
        <button @click="removeTodo(todo)" class="btn delete-btn">X</button>
      </li>
    </ul>
    <button @click="hideCompleted = !hideCompleted" class="toggle-btn">
      {{ hideCompleted ? 'Show all' : 'Hide completed' }}
    </button>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

let id = 0

const newTodo = ref('')
const hideCompleted = ref(false)
const todos = ref([
  { id: id++, text: 'Belajar HTML', done: false },
  { id: id++, text: 'Belajar JavaScript', done: false },
  { id: id++, text: 'Belajar Vue', done: false }
])

const filteredTodos = computed(() => {
  return hideCompleted.value
    ? todos.value.filter((t) => !t.done)
    : todos.value
})

function addTodo() {
  todos.value.push({ id: id++, text: newTodo.value, done: false })
  newTodo.value = ''
}

function removeTodo(todo) {
  todos.value = todos.value.filter((t) => t !== todo)
}

function editTodo(todo) {
  const newText = prompt('Enter new text', todo.text)
  if (newText !== null) {
    todo.text = newText
  }
}
</script>


