<template>
  <div class="container">
    <h1>Todo List</h1>
    <form @submit.prevent="addTodo">
      <input type="text" v-model="newTodo" placeholder="输入新任务..." class="input-box">
      <button type="submit" class="add-btn">添加</button>
    </form>

    <ul v-if="todos.length">
      <li v-for="todo in todos" :key="todo.id" :class="{ completed: todo.completed }">
        <span @click="toggleTodo(todo.id)">{{ todo.text }}</span>
        <button @click="removeTodo(todo.id)" class="del-btn">×</button>
      </li>
    </ul>
    <p v-else class="empty-tip">暂无任务，添加你的第一个任务吧！</p>
  </div>
</template>

<script setup>
import { ref, watchEffect } from 'vue'

// 待办事项数据结构
const todos = ref(JSON.parse(localStorage.getItem('todos')) || [])
const newTodo = ref('')

// 添加新任务
const addTodo = () => {
  if (newTodo.value.trim()) {
    todos.value.unshift({
      id: Date.now(),
      text: newTodo.value.trim(),
      completed: false
    })
    newTodo.value = ''
  }
}

// 切换完成状态
const toggleTodo = (id) => {
  const todo = todos.value.find(t => t.id === id)
  if (todo) todo.completed = !todo.completed
}

// 删除任务
const removeTodo = (id) => {
  todos.value = todos.value.filter(t => t.id !== id)
}

// 本地存储
watchEffect(() => {
  localStorage.setItem('todos', JSON.stringify(todos.value))
})
</script>

<style scoped>
.container {
  max-width: 600px;
  margin: 2rem auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}

h1 {
  color: #2c3e50;
  text-align: center;
}

.input-box {
  width: 70%;
  padding: 10px;
  font-size: 16px;
  border: 2px solid #ddd;
  border-radius: 4px;
  margin-right: 10px;
}

.add-btn {
  padding: 10px 20px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background 0.3s;
}

.add-btn:hover {
  background-color: #3aa876;
}

ul {
  list-style: none;
  padding: 0;
  margin-top: 20px;
}

li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px;
  margin: 8px 0;
  background: #f8f9fa;
  border-radius: 4px;
  transition: all 0.3s;
}

li:hover {
  transform: translateX(5px);
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.completed span {
  text-decoration: line-through;
  color: #999;
}

.del-btn {
  background: none;
  border: none;
  color: #ff4757;
  font-size: 1.2em;
  cursor: pointer;
  padding: 0 8px;
}

.del-btn:hover {
  color: #ff6b81;
}

.empty-tip {
  text-align: center;
  color: #666;
  margin-top: 2rem;
}
</style>