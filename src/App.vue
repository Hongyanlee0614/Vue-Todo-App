<template>
  <h1>Todo App</h1>
  <form @submit.prevent="addNewTodo">
    <label>New Todo</label>
    <input v-model="newTodo" name="newTodo">
    <button>Add New Todo</button>
  </form>
  <button @click="removeAllTodos">Remove All</button>
  <button @click="markAllDone">Mark All Done</button>
  <div v-if="todos.length">
    <div v-for="(todo, index) in todos" :key="todo.id" class="todo">
      <p :class="{done: todo.done}" @click="toggleDone(todo)">{{todo.content}}</p>
      <button @click="removeTodo(index)">Remove Todo</button>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'

export default {
  setup(){
    const newTodo = ref('')
    const todos = ref([])
    function addNewTodo(){
      if (newTodo.value !== '' && !todos.value.some(todo=>todo.content===newTodo.value) )
      todos.value.push({
        id: Date.now(),
        done: false,
        content: newTodo.value
      })
      newTodo.value=''
    }
    function toggleDone(todo){
      todo.done = !todo.done
    }
    function removeTodo(index){
      todos.value.splice(index,1)
    }
    function markAllDone(){
      todos.value.forEach(todo=>todo.done=true)
    }
    function removeAllTodos(){
      todos.value=[]
    }
    return {todos, newTodo, addNewTodo, toggleDone, removeTodo, markAllDone, removeAllTodos}
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
body {
  font-family: sans-serif;
  padding-top: 1em;
  padding-bottom: 1em;
  font-size: 2em;
  width: 80%;
  margin: 0 auto;
}
input, textarea, button, p, div, section, article, select {
  display: 'block';
  width: 100%;
  font-family: sans-serif;
  font-size: 1em;
  margin: 0.5em;
}
.todo {
  cursor: pointer;
}
.done {
  text-decoration: line-through;
}
</style>
