<template>
  <div class="max-w-[600px] my-8 mx-auto px-4">
    <TodoForm @addTodo="addNewTodo" />
    <!-- @addTodo it's our own creation -->
    <ul class="bg-white/80 backdrop-blur-md rounded-2xl p-6 shadow-lg">
      <!-- <TaskItem :isChecked="false"> True Komponen TaskItem </TaskItem>
      <TaskItem :isChecked="false"> False Komponen Task </TaskItem> -->
      <li v-for="(item, key) in sortedList" :key="key">
        <TaskItem :isChecked="item.checked" @change="updateItem(item)">{{
          item.title
        }}</TaskItem>
      </li>
    </ul>
  </div>
</template>
<script setup lang="ts">
import { computed, onMounted, ref, type Ref } from 'vue'
import TaskItem from './TaskItem.vue'
import TodoForm from './TodoForm.vue'

const storageItems:Ref<TodoItem[]> = ref([])

const getFromStorage = (): TodoItem[] => {
  const storedData = localStorage.getItem('todo-list')
  if (storedData) {
    return JSON.parse(storedData)
  }
  return []
}

const setToStorage  = (items: TodoItem[]): void => {
  localStorage.setItem('todo-list', JSON.stringify(items))
}

interface TodoItem {
  title: string
  checked: boolean
}

const initListItems = (): void => {
  if (storageItems.value?.length === 0) {
    const listItems: TodoItem[]= [
    { title: 'Belajar Vue.js', checked: false },
    { title: 'Membuat Aplikasi Todo', checked: true },
    { title: 'Asawawu', checked: false },
    { title: 'Deploy ke githubpages/netlify', checked: false },
  ]
    setToStorage(listItems)
    storageItems.value = listItems
  }
}


const updateItem = (item:TodoItem) : void => {
  const updatedItem = findItemInList(item)
  if (updatedItem) {
    toggleItemChecked(updatedItem)
    setToStorage(storageItems.value)
  }
}

const findItemInList = (item:TodoItem) : TodoItem | undefined => {
  return storageItems.value.find((itemInList:TodoItem) => itemInList.title === item.title)
}

const toggleItemChecked = (item:TodoItem) : void => {
     item.checked = !item.checked
}

const sortedList = computed(() => {
  return [...storageItems.value].sort((a, b) => Number(a.checked ? 1 : 0) - (b.checked ? 1 : 0))
})

const addNewTodo = (title:string) : void => {
  const newTodo:TodoItem = {
    title,
    checked: false,
  }
  storageItems.value.unshift(newTodo)
  setToStorage(storageItems.value)
}

onMounted(()=>{
  storageItems.value = getFromStorage()
  initListItems()
})
</script>
