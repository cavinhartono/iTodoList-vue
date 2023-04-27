<template>
  <div class="container p-6">
    <section class="greeting">
      <h2 class="font-bold">
        Good night,
        <input
          class="outline-none border-none"
          type="text"
          placeholder="Your Name"
          v-model="name"
        />
      </h2>
    </section>
    <section class="w-full">
      <h3>Create a todo</h3>
      <form @submit.prevent="addTodo">
        <h4>What's on your todolist</h4>
        <input type="text" placeholder="File like video or picture" v-model="content" />
        {{ content }}
        <h4>Category</h4>
        <div class="options">
          <label>
            <input type="radio" name="category" v-model="category" value="activity" />
            <span class="bubble"></span>
            <h5>Activity</h5>
          </label>
          <label>
            <input type="radio" name="category" v-model="category" value="personal" />
            <span class="bubble"></span>
            <h5>Personal</h5>
          </label>
        </div>
        <button class="btn primary">Add</button>
      </form>
    </section>

    <section class="todo-list">
      <ul class="todo">
        <li
          class="list"
          v-for="todo in todosOrderByAscending"
          :key="todo.createdAt"
          :class="`todo-list ${todo.done && 'done'}`"
        >
          <label>
            <input type="checkbox" v-model="todo.done" />
            <span :class="`bubble ${todo.category}`"></span>
          </label>
          <div class="content">
            <h2>{{ todo.content }}</h2>
            <button class="btn danger" @click="removeTodo">Delete</button>
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
    return b.createdAt - a.createdAt
  })
)

const addTodo = () => {
  if (content.value.trim() === '' || content.value === null) return

  todos.value.push(
    {
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
  todos.value = todos.value.filter((list) => list !== todo)
}

watch(name, (newVal) => localStorage.setItem('name', newVal))
watch(todos, (newVal) => localStorage.setItem('todos', JSON.stringify(newVal)), { deep: true })
onMounted(() => {
  name.value = localStorage.getItem('name') || ''
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
})
</script>
