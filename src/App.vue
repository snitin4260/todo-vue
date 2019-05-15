<template>
  <div id="app">
  <TodoInput @add-item="addItem"></TodoInput>
  <main>
  <TodoList :todos="todos"></TodoList>
  </main>
    
  </div>
</template>

<script>
import eventBus from "./eventBus.js"
import TodoInput from"./components/Input"
import TodoList from "./components/TodoList"
import shortid from 'shortid'
export default {
  name: "app",
  components: {
    TodoInput,
    TodoList
  },
  data() {
    return {
      todos:[]
    }
  },
  methods: {
    addItem(text) {
      let todoObj = {
        text,
        completed:false,
        note:'',
        id: shortid.generate()

      }
      this.todos.unshift(todoObj)
    },
    updateTodos(todo) {
      this.todos=todo
    }
  },
  mounted() {
    eventBus.$on('checkbox-event',todoObj=> {
      let newTodo=this.todos.map((todo)=> {
        if(todo.id===todoObj.id) {
          todo.completed= todoObj.value
        }
        return todo
        
      })
      this.updateTodos(newTodo)
    })

    eventBus.$on('delete-task',id=> {
      let newTodo=this.todos.filter((todo)=> {
        if(todo.id === id) return false;
        return true
      })
      this.updateTodos(newTodo)
    })
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

</style>
