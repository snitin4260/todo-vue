<template>
  <header class="header">
    <h1 class="header__title">Todo</h1>
    <input
      class="todo-input"
      @keyup.enter="updateTodoItems"
      v-model="text"
      placeholder="What needs to be done?"
      autofocus
    >
  </header>
</template>

<script>
export default {
  name: "Header",
  data() {
    return {
      text: ""
    };
  },
  methods: {
    updateTodoItems() {
      if (this.text.trim() === "") return;
      this.$emit("add-item", this.text);
      this.text = "";
    }
  }
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css?family=Ubuntu|Roboto:300");
.todo-input {
  flex: 1 1 100%;
  max-width: 50rem;
  border: 0.4rem solid teal;
  padding: 1rem;
  font-size: 2.4rem;
  font-family: "Roboto", sans-serif;
  box-shadow: -1px -1px 13px -5px rgba(0, 0, 0, 0.64);
}

.header {
  display: flex;
  flex-wrap: wrap;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  padding: 1rem;
  margin-bottom: 1.5rem;
}

.header__title {
  font-family: "Ubuntu", sans-serif;
  font-size: 8rem;
  flex: 1 1 100%;
  text-align: center;
  color: teal;
  margin-bottom: 1.4rem;
}

</style>


