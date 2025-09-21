<script setup lang="ts">
import { ref } from "vue";

const title = ref("");
const emit = defineEmits<{
  (e: "add-todo", title: string): void;
}>();

const submit = () => {
  if (title.value.trim() !== "") {
    emit("add-todo", title.value);
    title.value = "";
  }
};
</script>

<template>
  <form @submit.prevent="submit">
    <input
      v-model="title"
      type="text"
      placeholder="เพิ่มงานใหม่..."
    />
    <button type="submit">เพิ่ม</button>
  </form>
</template>

<style scoped>
form {
  display: flex;
  gap: 0.5rem;
  margin-bottom: 1rem;
}
input {
  flex: 1;
  padding: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 4px;
}
button {
  padding: 0.5rem 1rem;
  background: #42b983;
  border: none;
  border-radius: 4px;
  color: white;
  cursor: pointer;
}
button:hover {
  background: #369f6f;
}
</style>
