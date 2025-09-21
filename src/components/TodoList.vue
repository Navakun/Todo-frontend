<script setup lang="ts">
interface Todo {
  id: number;
  title: string;
  is_done: boolean;
}

const props = defineProps<{
  todos: Todo[];
}>();

const emit = defineEmits<{
  (e: "toggle-todo", id: number, is_done: boolean): void;
  (e: "delete-todo", id: number): void;
}>();

</script>

<template>
  <ul>
    <li v-for="todo in todos" :key="todo.id">
      <label>
        <input
          type="checkbox"
          :checked="todo.is_done"
          @change="emit('toggle-todo', todo.id, !todo.is_done)"
        />
        <span :class="{ done: todo.is_done }">{{ todo.title }}</span>
      </label>
      <button @click="emit('delete-todo', todo.id)">ลบ</button>
    </li>
  </ul>
</template>

<style scoped>
ul {
  list-style: none;
  padding: 0;
}
li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.5rem;
  border-bottom: 1px solid #ddd;
}
span.done {
  text-decoration: line-through;
  color: gray;
}
button {
  background: #e74c3c;
  border: none;
  border-radius: 4px;
  color: white;
  padding: 0.3rem 0.7rem;
  cursor: pointer;
}
button:hover {
  background: #c0392b;
}
</style>
