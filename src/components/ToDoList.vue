<template>
  <div class="small-container">
    <h2>To Do List</h2>
    <p v-if="todos.length == 0">All done! No Todos</p>
    <ol>
      <li
        v-for="todo in todos"
        :key="todo.id"
        :style="{fontSize:25 + 'px', marginBottom: 10 + 'px'}"
        :class="{done:todo.done}"
      >
        <template v-if="editing !== todo.id">{{todo.content}}</template>
        <template v-else>
          <input type="text" v-model="todo.content" />
        </template>

        <button v-if="editing === todo.id" class="editBtn" @click="updateTodo(todo)">Save</button>
        <button v-else class="editBtn" @click="editTodos(todo)">Edit</button>
        <button v-if="editing === todo.id" class="doneBtn" @click="cancelEdit(todo);">Cancel</button>
        <button v-else class="doneBtn" @click="$emit('delete:todo',todo.id)">Delete</button>
      </li>
    </ol>
    <form @submit.prevent="addItem">
      <input type="text" v-model="todo.content" />
      <button>add</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      todo: {
        content: "",
        done: false
      },
      editing: null
    };
  },
  props: {
    todos: Array
  },
  methods: {
    addItem() {
      this.todo.content === ""
        ? alert("PUT SOMETHING DAMNIT")
        : this.$emit("add:todo", this.todo);
      this.todo.content = "";
    },
    updateTodo(todo) {
      if (todo.content !== "") {
        this.$emit("update:todo", todo);
        this.editing = null;
      } else {
        alert("PUT SOMETHING DAMNIT");
      }
    },
    cancelEdit(todo) {
      Object.assign(todo, this.cachedTodos);
      this.editing = null;
    },
    editTodos(todo) {
      if (this.editing !== null) {
        alert("Please edit 1 todo at a time");
      } else {
        this.cachedTodos = Object.assign({}, todo);
        this.editing = todo.id;
      }
    }
  }
};
</script>

<style scoped>
button {
  background: lightgreen;
  border: 1px solid lightgreen;
}

.doneBtn {
  background: lightblue;
  border: 1px solid lightblue;
  position: absolute;
  right: 200px;
}

.editBtn {
  background: lightpink;
  border: 1px solid lightpink;
  position: absolute;
  right: 300px;
}

h2 {
  text-align: center;
  border-bottom: 2px solid gray;
}
.small-container {
  max-width: 900px;
}

input {
  width: 300px;
  display: inline-block;
  margin-right: 10px;
}

.done {
  text-decoration: line-through;
}
</style>