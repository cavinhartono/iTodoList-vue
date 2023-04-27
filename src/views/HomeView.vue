<style>
body {
  background: #f8f8f8;
  display: grid;
  place-content: center;
}

.done {
  text-decoration: line-through;
}
</style>

<template>
  <div class="container p-6">
    <section class="greeting">
      <h2 class="font-bold">
        Good night,
        <input
          class="outline-none border-none bg-transparent"
          type="text"
          placeholder="Your Name"
          v-model="name"
        />
      </h2>
    </section>
    <section class="w-full bg-white rounded-lg">
      <form class="my-4 p-4" @submit.prevent="addTodo">
        <h4>What's on your todolist</h4>
        <input
          type="text"
          class="border border-gray-300 rounded p-3 my-2 w-full"
          placeholder="File like video or picture"
          v-model="content"
        />
        <h4>Category</h4>
        <div class="flex justify-between items-center gap-4 my-2">
          <label class="p-12 w-full rounded border border-gray-300">
            <input type="radio" name="category" v-model="category" value="activity" />
            <span class="bubble"></span>
            <h5>Activity</h5>
          </label>
          <label class="p-12 w-full rounded border border-gray-300">
            <input type="radio" name="category" v-model="category" value="personal" />
            <span class="bubble"></span>
            <h5>Personal</h5>
          </label>
        </div>
        <button class="bg-blue-500 text-white rounded px-6 py-2 w-full">Add</button>
      </form>
    </section>

    <section class="w-full">
      <ul class="flex flex-col gap-4">
        <li
          class="w-full px-6 py-2 flex justify-center items-center bg-white rounded-lg"
          v-for="todo in todosOrderByAscending"
          :key="todo.createdAt"
          :class="`todo-list ${todo.done && 'done'}`"
        >
          <label>
            <input type="checkbox" v-model="todo.done" />
            <span :class="`bubble ${todo.category}`"></span>
          </label>
          <div class="w-full ml-6 flex">
            <h2 :class="`w-full flex items-center ${todo.done ?? 'done'}`">{{ todo.content }}</h2>
            <button
              class="bg-red-600 text-white rounded px-6 py-2"
              @click="removeTodo(todo.createdAt)"
            >
              Delete
            </button>
          </div>
        </li>
      </ul>
    </section>
  </div>
</template>

<script setup>
import { ref, onMounted, computed, watch } from 'vue'

const todos = ref([]),
  name = ref(''),
  content = ref(),
  category = ref(null)

const todosOrderByAscending = computed(() =>
  [...todos.value].sort((a, b) => {
    return a.createdAt - b.createdAt
  })
)

const addTodo = () => {
  let i = 0
  if (content.value.trim() === '' || content.value === null) return

  todos.value.push(
    {
      id: i++,
      content: content.value,
      category: category.value,
      createdAt: new Date().getTime(),
      done: false
    },
    { deep: true }
  )

  content.value = ''
  category.value = null
}

const removeTodo = (todo) => {
  this.todos.value = this.todos.filter((list) => list !== todo)
}

watch(name, (newVal) => localStorage.setItem('name', newVal))
watch(todos, (newVal) => localStorage.setItem('todos', JSON.stringify(newVal)), { deep: true })
onMounted(() => {
  name.value = localStorage.getItem('name') || ''
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
})
</script>
