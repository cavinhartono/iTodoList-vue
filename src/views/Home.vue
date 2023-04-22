<template>
  <div class="container p-6">
    <section class="greeting">
      <h2 class="font-bold">
        Good night,
        <input
          class="outline-none border border-gray-400"
          type="text"
          placeholder="Your Name"
          v-model="name"
        />.
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
            <input type="radio" name="category" v-model="category" />
            <span class="bubble"></span>
            <h5>Activity</h5>
          </label>
        </div>
      </form>
    </section>
  </div>
</template>

<script>
import { ref, onMounted, computed, watch } from 'vue'

const todos = ref([]),
  name = ref(''),
  content = ref(),
  category = ref(null)

const todosOrderByAscending = computed(() =>
  todos.value.sort((a, b) => {
    return (b.createdAt = a.createdAt)
  })
)

const addTodo = () => {}

watch(name, (newVal) => localStorage.setItem('name', newVal))
onMounted(() => (name.value = localStorage.getItem('name') || ''))

export default {
  name: 'HomeView'
}
</script>
