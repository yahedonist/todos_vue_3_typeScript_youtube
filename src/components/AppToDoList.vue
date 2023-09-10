<script lang="ts">
import {defineComponent, PropType} from "vue";
import AppToDoItem from "@/components/AppToDoItem.vue";
import {Todo} from "@/types/todo";


export default defineComponent({
  components: {
    AppToDoItem
  },
  props: {
    todos: {
      type: Array as PropType<Todo[]>
    }
  },
  methods: {
    toggleTodo(id: number){
      this.$emit('toggleTodo', id)
    },
    removeTodo(id: number){
      this.$emit('removeTodo', id)
    }
  },
  emits: {
    toggleTodo: (id: number) => Number.isInteger(id),
    removeTodo: (id: number) => Number.isInteger(id),
  }
})
</script>

<template>
  <ul class="todo-list">
    <AppToDoItem v-for="todo in todos" :key="todo.id" :todo="todo" @toggle-todo="toggleTodo" @remove-todo="removeTodo"/>
  </ul>
</template>

<style scoped>

</style>