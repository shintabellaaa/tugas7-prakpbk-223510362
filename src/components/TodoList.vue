<template>
    <div class="todo-list">
      <h1>Todo List</h1>
      <input v-model="newTask" @keyup.enter="addNewTask" placeholder="Add a new task" />
      <ul>
        <li v-for="(task, index) in tasks" :key="index">
          <span :class="{ completed: task.completed }" @click="toggleTaskCompletion(index)">
            {{ task.text }}
          </span>
          <button @click="removeTask(index)">Delete</button>
        </li>
      </ul>
      <p>{{ unfinishedTasks }} tasks left</p>
    </div>
  </template>
  
  <script>
  import { useTodoStore } from '../stores/todo';
  import { ref } from 'vue';
  
  export default {
    setup() {
      const todoStore = useTodoStore();
      const newTask = ref('');
  
      const addNewTask = () => {
        if (newTask.value.trim()) {
          todoStore.addTask(newTask.value);
          newTask.value = '';
        }
      };
  
      return {
        newTask,
        tasks: todoStore.tasks,
        addNewTask,
        removeTask: todoStore.removeTask,
        toggleTaskCompletion: todoStore.toggleTaskCompletion,
        unfinishedTasks: todoStore.unfinishedTasks,
      };
    }
  };
  </script>
  
  <style scoped>
  .todo-list {
    max-width: 500px;
    margin: 0 auto;
    padding: 20px;
    background-color: #f9f9f9;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0,0,0,0.1);
  }
  
  input {
    width: 100%;
    padding: 10px;
    margin-bottom: 20px;
    border: 1px solid #ddd;
    border-radius: 4px;
  }
  
  ul {
    list-style: none;
    padding: 0;
  }
  
  li {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px 0;
    border-bottom: 1px solid #eee;
  }
  
  .completed {
    text-decoration: line-through;
    color: #bbb;
  }
  
  button {
    background: none;
    border: none;
    color: #ff6b6b;
    cursor: pointer;
  }
  </style>
  