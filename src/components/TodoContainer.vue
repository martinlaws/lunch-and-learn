<template>
  <div>
    <div class="todos-container">
      <div
        class="todo-card"
        v-for="todo in todos"
        :key="todo.id"
        :class="{ complete: todo.complete }"
      >
        <h2 @click="completeTodo(todo)">{{ todo.title }}</h2>
        <input type="text" v-model="todo.title" @blur="save(todos)" />
      </div>
    </div>
    <form class="edit-form" @submit.prevent="save(todos)">
      <button @click="resetTodos">Reset Todos</button>
    </form>
  </div>
</template>

<script>
const STORAGE_KEY = 'todo-list'
const baseTodos = [
  { id: 0, title: 'Big Todo!', complete: false },
  { id: 1, title: 'Little Todo #1', complete: false },
  { id: 2, title: 'Little Todo #2', complete: false },
  { id: 3, title: 'Little Todo #3', complete: false }
]

export default {
  data: function() {
    return {
      todos: this.fetchTodos()
    }
  },
  methods: {
    fetchTodos() {
      const todos = JSON.parse(localStorage.getItem(STORAGE_KEY)) || baseTodos
      return todos
    },
    completeTodo(todo) {
      todo.complete = !todo.complete

      this.save(this.todos)
    },

    save(todos) {
      this.todos = todos
      localStorage.setItem(STORAGE_KEY, JSON.stringify(todos))
    },
    resetTodos() {
      this.todos = baseTodos
      this.save(baseTodos)
    }
  }
}
</script>

<style scoped>
.todos-container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: 1fr 1fr;
  gap: 2rem;
}

.todos-container .todo-card:first-of-type {
  grid-column: span 3;
  font-size: 3rem;
}

.todo-card {
  border-radius: 0.5rem;
  height: 15rem;
  background-color: lightgreen;
  padding: 1rem;
  display: flex;
  flex-flow: column nowrap;
  align-items: center;
  justify-content: center;
}

.todo-card h2 {
  cursor: pointer;
  user-select: none;
}

.complete {
  text-decoration: line-through;
  background-color: lightcoral;
}
</style>
