<template>
  <div id="app">
    <Header @add-item="addItem"></Header>
    <main>
      <div v-show="ifTasksthere" class="filter">
        <div class="filter__item">All</div>
        <div class="filter__item">Active</div>
        <div class="filter__item">Completed</div>
      </div>
      <TodoList :todos="todos"></TodoList>
    </main>
  </div>
</template>

<script>
import eventBus from "./eventBus.js";
import Header from "./components/Header";
import TodoList from "./components/TodoList";
import shortid from "shortid";
export default {
  name: "app",
  components: {
    Header,
    TodoList
  },
  data() {
    return {
      todos: []
    };
  },
  methods: {
    addItem(text) {
      let todoObj = {
        text,
        completed: false,
        note: "",
        id: shortid.generate()
      };
      this.todos.unshift(todoObj);
    },
    updateTodos(todo) {
      this.todos = todo;
    }
  },
  computed: {
    ifTasksthere() {
      return this.todos.length > 0
    }
  },
  mounted() {
    eventBus.$on("checkbox-event", todoObj => {
      let newTodo = this.todos.map(todo => {
        if (todo.id === todoObj.id) {
          todo.completed = todoObj.value;
        }
        return todo;
      });
      this.updateTodos(newTodo);
    });

    eventBus.$on("delete-task", id => {
      let newTodo = this.todos.filter(todo => {
        if (todo.id === id) return false;
        return true;
      });
      this.updateTodos(newTodo);
    });

    eventBus.$on("edit-text", todoObj => {
      let newTodo = this.todos.map(todo => {
        if (todo.id === todoObj.id) {
          todo.text = todoObj.value;
        }
        return todo;
      });
      this.updateTodos(newTodo);
    });

    eventBus.$on("edit-note", todoObj => {
      let newTodo = this.todos.map(todo => {
        if (todo.id === todoObj.id) {
          todo.note = todoObj.value;
        }
        return todo;
      });
      this.updateTodos(newTodo);
    });
  }
};
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
html {
  font-size: 62.5%;
}

main {
  max-width: 960px;
  width: 100%;
  margin: 0 auto;
}

.filter {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 1.2rem;
}

.filter__item {
  font-size: 2.4rem;
  padding: 1rem;
  margin-right: 1rem;
  border: 2px solid teal;
  cursor: pointer;
  font-family: "Roboto", sans-serif;
  user-select: none;
  -webkit-touch-callout: none;
}
</style>
