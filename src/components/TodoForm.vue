<template>
  <form @submit="handleSubmit">
    <input
      v-model="newTodo"
      type="text"
      placeholder="Add new Activity!"
      maxlength="100"
    />
    <button type="submit">
      <span class="text-xl font-bold">+</span>
      Add Todo
    </button>
  </form>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const newTodo = ref('')
const emit = defineEmits<{
  (e: 'addTodo', title: string): void
}>()

const handleSubmit = () => {
  emit('addTodo', newTodo.value)
}
</script>
