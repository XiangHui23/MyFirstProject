<script setup>
import { ref } from 'vue'

const newNote = ref('')
const notes = ref([])

const addNote = () => {
  if (newNote.value.trim() !== '') {
    notes.value.push({ id: Date.now(), text: newNote.value })
    newNote.value = '' 
  }
}

const deleteNote = (id) => {
  notes.value = notes.value.filter(note => note.id !== id)
}
</script>

<template>
  <div style="max-width: 400px; margin: 50px auto; font-family: sans-serif;">
    <h1>我的便签本 📝</h1>
    
    <div style="display: flex; gap: 10px; margin-bottom: 20px;">
      <input 
        v-model="newNote" 
        @keyup.enter="addNote"
        placeholder="输入新便签..." 
        style="flex: 1; padding: 8px;"
      />
      <button @click="addNote" style="padding: 8px 16px; cursor: pointer;">添加</button>
    </div>

    <ul style="list-style: none; padding: 0;">
      <li v-for="note in notes" :key="note.id" style="display: flex; justify-content: space-between; padding: 10px; background: #f4f4f4; margin-bottom: 8px; border-radius: 4px;">
        {{ note.text }}
        <button @click="deleteNote(note.id)" style="color: white; background: red; border: none; padding: 4px 8px; cursor: pointer; border-radius: 4px;">删除</button>
      </li>
    </ul>
  </div>
</template>