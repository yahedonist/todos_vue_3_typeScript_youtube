<template>
  <AppHeader/>
  <AppFilter :active-filter="activeFilter" @set-filter="setFilter"/>

  <main class="app-main">
    <AppToDoList :todos="filteredTodos" @toggle-todo="toggleTodo" @remove-todo="removeTodo"/>
    <AppAddToDo @add-todo="addTodo"/>
  </main>
  <AppFooter :stats="getStats"/>
</template>
<script lang="ts">
import {defineComponent} from 'vue'
import AppHeader from "@/components/AppHeader.vue";
import AppFilter from "@/components/AppFilter.vue";
import AppToDoList from "@/components/AppToDoList.vue";
import AppAddToDo from "@/components/AppAddToDo.vue";
import AppFooter from "@/components/AppFooter.vue";
import {Todo} from "@/types/todo";
import {Filter} from "@/types/filter";
import Stats from "@/types/stats";

interface State {
  todos: Todo[],
  activeFilter: Filter
}

export default defineComponent({
  components: {
    AppFooter,
    AppAddToDo,
    AppToDoList,
    AppFilter,
    AppHeader
  },
  data(): State {
    return {
      todos: [
        {id: 0, text: 'Learn the basics of Typescript', completed: true},
        {id: 1, text: 'Learn the basics of JavaScript', completed: true},
        {id: 2, text: 'Create todo app', completed: false},
        {id: 3, text: 'Test task', completed: false},
      ],
      activeFilter: 'All'
    }
  },
  computed: {
    filteredTodos(): Todo[] {
      switch (this.activeFilter) {
        case "Active":
          return this.activeTodos
        case "Done":
          return this.doneTodos
        case "All":
        default:
          return this.todos
      }
    },
    getStats(): Stats {
      return {
        active: this.activeTodos.length,
        done: this.doneTodos.length
      }
    },
    activeTodos(): Todo[] {
      return this.todos.filter(todo => !todo.completed)
    },
    doneTodos(): Todo[] {
      return this.todos.filter(todo => todo.completed)
    }
  },
  methods: {
    addTodo(todo: Todo) {
      this.todos.push(todo)
    },
    toggleTodo(id: number) {
      const targetTodo = this.todos.find((todo: Todo) => todo.id === id)

      if (targetTodo) {
        targetTodo.completed = !targetTodo.completed
      }
    },
    removeTodo(id: number) {
      this.todos = this.todos.filter((todo: Todo) => todo.id !== id)
    },
    setFilter(filter: Filter) {
      this.activeFilter = filter
    }
  }
})
</script>