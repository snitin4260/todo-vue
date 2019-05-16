<template>
  <div id="app">
    <Header @add-item="addItem"></Header>
    <main>
      <div v-show="ifTasksthere" class="filter">
        <FilterTab
          @change-tab-and-filter="changeTabAndFilter"
          v-for="(tab,index) in filters"
          :tab="tab"
          :key="index"
          :id="index"
          :selectedTab="selectedTab"
        ></FilterTab>
      </div>
      <TodoList :filter="filters[selectedTab]" :todos="todos"></TodoList>
    </main>
  </div>
</template>

<script>
import eventBus from "./eventBus.js";
import Header from "./components/Header";
import TodoList from "./components/TodoList";
import FilterTab from "./components/FilterTab";
import shortid from "shortid";
export default {
  name: "app",
  components: {
    Header,
    TodoList,
    FilterTab
  },
  data() {
    return {
      todos: [],
      filters: ["All", "Active", "Completed"],
      selectedTab: 0
    };
  },
  methods: {
    addItem(text) {
      let that = this;
      let todoObj = {
        text,
        completed: false,
        note: "",
        id: shortid.generate(),
        time: that.currentTime()
      };
      this.todos.unshift(todoObj);
    },
    updateTodos(todo) {
      this.todos = todo;
    },
    changeTabAndFilter(id) {
      this.selectedTab = id;
    },
    currentTime() {
      let time = Date().toLocaleString();
      let firstWhitespaceIndex = time.indexOf(" ");
      let gmtIndex = time.indexOf("GMT");
      return time.slice(firstWhitespaceIndex + 1, gmtIndex - 1);
    }
  },
  computed: {
    ifTasksthere() {
      return this.todos.length > 0;
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
</style>
