<template>
  <ToDoList :todos="todos" @add:todo="addTodo" @delete:todo="deleteTodo" @update:todo="updateTodo"></ToDoList>
</template>

<script>
import ToDoList from "./components/ToDoList";

export default {
  components: { ToDoList },
  data() {
    return {
      todos: [
        { id: 1, content: "Buy steak", done: false },
        { id: 2, content: "Cook dinner", done: false },
        { id: 3, content: "Play some ultimate frisbee", done: false }
      ]
    };
  },
  methods: {
    addTodo(todo) {
      const id = Math.round(Math.random() * 100 + 1);

      const newTodo = { ...todo, id };
      this.todos = [...this.todos, newTodo];
    },
    deleteTodo(id) {
      this.todos = this.todos.filter(todo => todo.id !== id);
    },
    updateTodo(updatedTodo) {
      this.todos = this.todos.map(todo =>
        todo.id === updatedTodo.id ? updatedTodo : todo
      );
    }
  }
};
</script>

<style></style>
