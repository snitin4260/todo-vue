<template>
<li class="list" :id="todo.id" >
  <div @click="toggleCheckBox" class="list__checkbox">
    <div v-show="todo.completed" class="list__tick">&#10003;</div>
  </div>
  <p class="list__task" :class="{ taskDone: todo.completed}">{{todo.text}}</p>
  <div @click="deleteTask" class="list__deleteButton">&#9747;</div>
</li>

</template>

<script>
import Vue from'vue'
import eventBus from "../eventBus.js"

export default {
  name: 'TodoItem',
  props: {
    todo: {
      type:Object,
      required:true
    }
  },
  methods:{
    toggleCheckBox() {
      let newCheckBoxValue;
      if(this.todo.completed) newCheckBoxValue = false
      else {newCheckBoxValue = true}
      eventBus.$emit('checkbox-event',{value:newCheckBoxValue,id:this.todo.id})

    },
    deleteTask() {
      eventBus.$emit('delete-task', this.todo.id)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import url('https://fonts.googleapis.com/css?family=Roboto:300');
.list {
  list-style-type: none;
  display: flex;
  font-size: 2.4rem;
  justify-content: space-between;
  align-items: center;
  border: 0.4rem solid teal;
  padding: 1rem;
  margin-bottom: 1rem;
}

.list__tick {
  position: absolute;
  top: 14%;
  left: 18%;
  width: 0.5rem;
  height: 0.5rem;
  color: green;
  user-select: none;
  -webkit-touch-callout: none;
}


.list__task {
  font-family: "Roboto", sans-serif;
  flex: 1 1 auto;
  word-break: break-all;
  transition: opacity 0.3s ease-out;
}

.list__checkbox {
  width: 4rem;
  height: 4rem;
  border-radius: 100%;
  border: 0.2rem solid black;
  position: relative;
  margin-right: 1.5rem;
  flex-shrink: 0;
  cursor: pointer;
}

.list__deleteButton {
  font-size: 3.8rem;
  color: red;
  opacity: 0.6;
  flex-shrink: 0;
  cursor: pointer;
  user-select: none;
  -webkit-touch-callout: none;
}

.list__edit {
  flex: 1 1 auto;
  border: 0.2rem solid black;
  font-family: "Roboto", sans-serif;
  font-size: 2.4rem;
  padding: 1rem;
}
@media screen and (max-width: 400px) {
  .list__edit {
    flex: 0 1 60%;
    max-width: 330px;
  }
}

.taskDone {
  opacity: 0.3;
  text-decoration: line-through;
}


</style>
