<template>
  <div>
    <AddTodo v-on:add-todo="addTodo" />
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" />
  </div>
</template>

<script>
import Todos from "../components/Todos";
import AddTodo from "../components/AddTodo";

export default {
  name: "Home",
  components: {
    Todos,
    AddTodo
  },
  data() {
    return {
      todos: []
    };
  },
  methods: {
    deleteTodo(id) {
      fetch(`https://jsonplaceholder.typicode.com/todos/${id}`, {
        method: "DELETE"
      })
        .then(() => {
          this.todos = this.todos.filter(todo => todo.id !== id);
        })
        .catch(error => console.log(error));
    },

    addTodo(newTodo) {
      const { title, completed } = newTodo;
      const headers = {
        "Content-Type": "application/json"
      };
      const payload = {
        title,
        completed
      };
      const url = "https://jsonplaceholder.typicode.com/todos";
      fetch(url, {
        method: "POST",
        headers,
        body: JSON.stringify(payload)
      })
        .then(response => response.json())
        .then(data => {
          this.todos = [...this.todos, data];
        })
        .catch(error => console.log(error));
    }
  },
  created() {
    fetch("https://jsonplaceholder.typicode.com/todos?_limit=5")
      .then(response => response.json())
      .then(data => (this.todos = data))
      .catch(error => console.log(error));
  }
};
</script>