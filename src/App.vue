<script setup>
import { ref, computed } from 'vue'

let id = 0

const newTodo = ref('')
const hideCompleted = ref(false)
const todos = ref([
  { id: id++, text: 'test ', done: true, editing: false },
  { id: id++, text: 'learn vue ', done: true, editing: false },
  { id: id++, text: 'Nobar Real Madrid VS Dormunt ', done: false, editing: false }
])


function hapus(todo) {
  todos.value = todos.value.filter((t) => t !== todo)
}

function edit(todo) {
  todo.editing = true
  const input = document.querySelector('.editInput')
  input.focus()
}

function saveEdit(todo) {
  todo.editing = false
}

function tambah() {
  if (newTodo.value.trim() === '') {
    alert('Masukkan teks untuk menambahkan todo baru!')
    return
  }
  todos.value.push({ id: id++, text: newTodo.value, done: false, editing: false })
  newTodo.value = ''
}


const filteredTodos = computed(() => {
  return hideCompleted.value
    ? todos.value.filter((t) => !t.done)
    : todos.value
})
</script>

<template>
  <div>
    <h2 style="margin-bottom: 1rem;">To do list</h2>
    <form>
      <input v-model="newTodo" placeholder="Input here">
      <button @click.prevent="tambah">Tambah</button>
    </form>

    <ol>
      <li v-for="todo in filteredTodos" :key="todo.id">
        <input type="checkbox" v-model="todo.done">
        <span v-if="!todo.editing" :class="{done: todo.done}">{{ todo.text }}</span>
        <input v-else type="text" class="editInput" v-model="todo.text" @keyup.enter="saveEdit(todo)" @blur="saveEdit(todo)">
        <button @click="hapus(todo)">Hapus</button>
        <button @click="edit(todo)">Edit</button>
      </li>
    </ol>

    <button @click="hideCompleted = !hideCompleted">
      {{ hideCompleted ? 'Show all' : 'Hide completed' }}
    </button>
  </div>
</template>


