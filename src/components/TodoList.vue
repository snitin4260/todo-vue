<template>
  <ul class="list-container">
    <TodoItem v-for="todo in filteredItems" :key="todo.id" :todo="todo"></TodoItem>
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
    },
    filter: {
      type: String,
      required: true
    }
  },

  computed: {
    filteredItems() {
      if (this.filter === "All") return this.todos;
      if (this.filter === "Completed") {
        const completedTodos = this.todos.filter(todo => {
          if (todo.completed === true) return true;
          return false;
        });

        return completedTodos;
      }
      if (this.filter === "Active") {
        const activeTodos = this.todos.filter(todo => {
          if (todo.completed === false) return true;
          return false;
        });

        return activeTodos;
      }
    }
  }
};
</script>

<style scoped>
.list-container {
  padding: 0 0.3rem;
}
</style>