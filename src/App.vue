<script setup lang="ts">
import { onUnmounted, ref } from 'vue';
import TodoList from './components/TodoList.vue';
import TodoForm from './components/TodoForm.vue';

interface Todo {
  id: number;
  title: string;
  is_done: boolean;
}

const todos = ref<Todo[]>([]);

const fetchTodos = async () => {
  const res = await fetch('http://localhost:3000/todos');
  todos.value = await res.json();
};

const addTodo = async (title: string) => {
  const res = await fetch("http://localhost:3000/todos", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ title })
  });
  const newTodo = await res.json();
  todos.value.push(newTodo);
};

const toggleTodo = async (id: number, is_done: boolean) => {
  await fetch(`http://localhost:3000/todos/${id}`, {
    method: "PUT",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ is_done })
  });
  const todo = todos.value.find(t => t.id === id);
  if (todo) todo.is_done = is_done;
};

const deleteTodo = async (id: number) => {
  await fetch(`http://localhost:3000/todos/${id}`, { method: "DELETE" });
  todos.value = todos.value.filter(t => t.id !== id);
};

onUnmounted(fetchTodos);
</script>

<template>
  <main>
    <h1>My Todo App</h1>
    <TodoForm @add-todo="addTodo" />
    <TodoList 
      :todos="todos" 
      @toggle-todo="toggleTodo" 
      @delete-todo="deleteTodo" 
      />
  </main>
</template>

<style scoped>
main {
  max-width: 600px;
  margin: 2rem auto;
  padding: 1rem;
  font-family: Arial, sans-serif;
  border-radius: 8px;
}
h1{
  text-align: center;
}
</style>
