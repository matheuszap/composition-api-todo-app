<template>
  <h1>Task App</h1>

  <input  v-model="newTodo" name="newTodo" autocomplete="off" placeholder="New Task" v-on:keyup.enter="addTodo"/>

  <h2>Task List</h2>
  <ul>
    <li v-for="(todo, index) in todos" :key="index">
      <span :class="{ done: todo.done }" @click="doneTodo(todo)">
        {{ todo.content }} 
      </span>
      <button @click="removeTodo(index)">Remove</button>
    </li>
  </ul>

  <h4 v-if="todos.length === 0">Empty List</h4>


</template>

<script>

import { ref } from 'vue';
export default {
  name: 'App',
  setup() {
    const newTodo = ref('');
    const defaultData = [{
      done: false,
      content: ''
    }]
    const todosData = JSON.parse(localStorage.getItem('todos')) || defaultData;
    const todos = ref(todosData);

    function addTodo() {
      if(newTodo.value) {
        todos.value.push({
          done: false,
          content: newTodo.value
        });
        newTodo.value = '';
      }
      saveData();
    }

    function doneTodo(todo) {
      todo.done = !todo.done;
      saveData();
    }

    function removeTodo(index) {
      todos.value.splice(index, 1);
      saveData();
    }

    function saveData() {
      const storageData = JSON.stringify(todos.value);
      localStorage.setItem('todos', storageData);
    }

    return {
      todos,
      newTodo,
      addTodo,
      doneTodo,
      removeTodo,
      saveData
    }
  }

}
</script>

<style>


body {
  margin: 0;
  padding: 0;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  background-color: #27292d;
  color: white;

}

#app {
    max-width: 600px;
    margin-left: auto;
    margin-right: auto;
    padding: 20px; 
  }

h1 {
  font-weight: bold;
  font-size: 28px;
  text-align: center;
}

input {
  display: block;
  width: 100%;
  padding: 15px;

  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

input:focus {
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 16px 0px 16px 0px;
}

ul {
	padding: 10px;
}


li {
	display: flex;
	justify-content: space-between;
  align-items: center;
	border: 2px solid;
	padding: 12px 24px;
	border-radius: 6px;
	margin-bottom: 12px;
}

span {
  cursor: pointer;
}

.done {
  text-decoration: line-through;
}

button {
  font-size: 12px;
	padding: 6px;
}

</style>
