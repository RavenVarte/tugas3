<template>
  <div class="background">
  <img src="../src/assets/wp.jpg" />
  </div>
  <div class="todo-app">
  <form @submit.prevent="addTodo" class="todo-from">
    <input v-model="newTodo" required placeholder="Masukan TODO" class="todo-input">
    <button type="submit" class="todo-button">Add Todo</button>
  </form>

  <ul class="todo-list">
    <li v-for="todo in filteredTodos" :key="todo.id" class="todo-item">
      <input type="checkbox"  v-model="todo.done">
      <template v-if="todo !== editingTodo">
        <span :class="{done: todo.done}">{{ todo.text }}</span>
        <button @click="removeTodo(todo)" class="delete-button">X</button>
        <button @click="editTodo(todo)" class="edit-button">Edit</button>
      </template>
      <!-- Tambahkan input untuk pengeditan -->
      <template v-else>
        <input v-model="editedTodoText" @keyup.enter="saveEdit(todo)" @keyup.esc="cancelEdit" class="todo-input">
        <button @click="saveEdit(todo)" class="todo-button">Save</button>
        <button @click="cancelEdit" class="delete-button">Cancel</button>
      </template>
  </li>
  </ul>

  <button @click="hideCompleted = !hideCompleted" class="toggle-completed">
  {{ hideCompleted ? 'show all' : 'Hide Completed'}}</button> 
  <p>{{ pesan }}</p>
  </div>
</template>

<script>
let id = 0

export default {
  data() {
    return {
      newTodo: '',
      hideCompleted: false,
      todos: [
        { id: id++, text: 'Learn HTML', done: true },
        { id: id++, text: 'Learn JavaScript', done: true },
        { id: id++, text: 'Learn Vue', done: false }
      ],
      editingTodo: null,
      editedTodoText: ''
    }
  },
  computed: {
    filteredTodos() {
      return this.hideCompleted
        ? this.todos.filter((t) => !t.done)
        : this.todos
    }
  },
  methods: {
    addTodo() {
      this.todos.push({ id: id++, text: this.newTodo, done: false })
      this.newTodo = ''
    },
    removeTodo(todo) {
      this.todos = this.todos.filter((t) => t !== todo)
    },
    editTodo(todo) {
      this.editingTodo = todo
      this.editedTodoText = todo.text
    },
    cancelEdit() {
      this.editingTodo = null
      this.editedTodoText = ''
    },
    saveEdit(todo) {
      todo.text = this.editedTodoText
      this.cancelEdit()
    }
  }
}
</script>

<style>
.todo-app{
  max-width: 100%;
  max-height: 100%;
}
.todo-input{
  margin:  0 20px 5px 1px;
  padding: 5px;
}

.todo-button{
  color: white;
  background-color: green;
  padding: 4px;
  border-radius: 4px;
  cursor: pointer;
}

.delete-button{
  margin-left: 13px;
  color: white;
  background-color: red;
  cursor: pointer;
}

.done{
  text-decoration: line-through;
}

.background img{
  position: fixed;
  top: 0;
  left: 0;
  min-height: 90%;
  min-width: 500px;
  width: 100%;
  height: 100%;
  overflow: hidden;
  z-index: -2;
  object-fit: cover;
  -webkit-background-size:cover; -moz-background-size:cover; -o-background-size:cover; background-size: cover;
  filter: brightness(0.8);
}
</style>