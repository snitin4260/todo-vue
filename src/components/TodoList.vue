<template>
  <ul class="list-container">
    <TodoItem v-for="todo in todos" :key="todo.id" :todo="todo"></TodoItem>
  </ul>
</template>

<script>
import TodoItem from "./TodoItem";
export default {
  name: "TodoList",
  components: {
    TodoItem
  },
  props: {
    todos: {
      type: Array,
      required: true
    }
  },
  data() {
    return {};
  }

};
</script>

<style scoped>
.list-container {
  padding: 0 0.3rem;
}

</style>