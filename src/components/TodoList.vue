<template>
  <div class="todo-container">
    <h1 class="todo-title">Todo List</h1>
    <div class="input-container">
      <input v-model="newTodoText" @keyup.enter="addTodo" placeholder="Add a new task" class="todo-input" />
      <button @click="addTodo" class="todo-button">Add</button>
    </div>
    <ul class="todo-list">
      <li v-for="(todo, index) in todos" :key="index" class="todo-item">
        <input type="checkbox" :checked="todo.completed" @change="toggleTodoCompletion(index)" class="todo-checkbox" />
        <span :class="{ completed: todo.completed }" class="todo-text">{{ todo.text }}</span>
        <button @click="removeTodo(index)" class="todo-delete">Delete</button>
      </li>
    </ul>
    <p class="todo-count">Incomplete tasks: {{ incompleteCount }}</p>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import { useTodoStore } from '../stores/todo';

const todoStore = useTodoStore();

const newTodoText = ref('');

const addTodo = () => {
  if (newTodoText.value.trim()) {
    todoStore.addTodo(newTodoText.value);
    newTodoText.value = '';
  }
};

const removeTodo = (index) => {
  todoStore.removeTodo(index);
};

const toggleTodoCompletion = (index) => {
  todoStore.toggleTodoCompletion(index);
};

const todos = computed(() => todoStore.todos);
const incompleteCount = computed(() => todoStore.incompleteCount);
</script>

<style scoped>
.todo-container {
  max-width: 600px;
  margin: 50px auto;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  background-color: #aedcff;
  font-family: Arial, sans-serif;
}

.todo-title {
  font-size: 2em;
  margin-bottom: 20px;
  text-align: center;
  color: #004c99;
}

.input-container {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.todo-input {
  flex: 1;
  padding: 10px;
  font-size: 1em;
  border: 1px solid #004c99;
  border-radius: 5px 0 0 5px;
  outline: none;
}

.todo-button {
  padding: 10px 20px;
  font-size: 1em;
  border: none;
  background-color: #004c99;
  color: #fff;
  border-radius: 0 5px 5px 0;
  cursor: pointer;
}

.todo-button:hover {
  background-color: #003366;
}

.todo-list {
  list-style: none;
  padding: 0;
}

.todo-item {
  display: flex;
  align-items: center;
  padding: 10px;
  border-bottom: 1px solid #4da6ff;
}

.todo-item:last-child {
  border-bottom: none;
}

.todo-checkbox {
  margin-right: 10px;
}

.todo-text {
  color: #004c99;
}

.todo-text.completed {
  text-decoration: line-through;
  color: #888;
}

.todo-delete {
  margin-left: auto;
  background-color: #cc0000;
  color: #fff;
  border: none;
  padding: 5px 10px;
  cursor: pointer;
}

.todo-delete:hover {
  background-color: #990000;
}

.todo-count {
  text-align: center;
  color: #004c99;
}
</style>
