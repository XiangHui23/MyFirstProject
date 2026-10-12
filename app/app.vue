<script setup>
import { ref, computed } from 'vue'

const searchQuery = ref('')
const newName = ref('') // 修复：必须声明绑定的变量
const newRole = ref('')

const employees = ref([
  { id: 1, name: 'Alex', role: 'Frontend Engineer', email: 'alex@company.com' },
  { id: 2, name: 'Bella', role: 'Project Manager', email: 'bella@company.com' },
  { id: 3, name: 'Charlie', role: 'UI Designer', email: 'charlie@company.com' },
  { id: 4, name: 'David', role: 'Backend Engineer', email: 'david@company.com' }
])

// 进阶：使用 computed 动态计算过滤后的列表，不破坏原数组
const filteredEmployees = computed(() => {
  if (searchQuery.value === '') {
    return employees.value
  }
  return employees.value.filter(emp => 
    // 修复：JS 中查字符串包含用 .includes() 而不是 in
    emp.name.toLowerCase().includes(searchQuery.value.toLowerCase())
  )
})

const addEmp = () => {
  // 修复：引号拼写错误
  if (newName.value.trim() !== '' && newRole.value.trim() !== '') {
    employees.value.push({
      id: employees.value.length + 1, // 修复：加上 .value
      name: newName.value,
      role: newRole.value,
      email: '-'
    })
    newName.value = ''
    newRole.value = ''
  }
}
</script>

<template>
  <div style="max-width: 400px; margin: 50px auto; font-family: sans-serif;">
    <h1>Employee Directory 📝</h1>
    
    <h2>搜索</h2>
    <div style="display: flex; gap: 10px; margin-bottom: 20px;">
      <!-- 注意这里移除了 click 按钮，因为有了 computed，只要 searchQuery 变化就会自动实时过滤 -->
      <input 
        v-model="searchQuery" 
        placeholder="输入姓名实时搜索..." 
        style="flex: 1; padding: 8px;"
      />
    </div>

    <!-- 修复：循环 computed 算出来的 filteredEmployees，且 :key 绑定到单体 emp.id -->
    <ul style="list-style: none; padding: 0;">
      <li v-for="emp in filteredEmployees" :key="emp.id" style="display: flex; justify-content: space-between; padding: 10px; background: #f4f4f4; margin-bottom: 8px; border-radius: 4px;">
        <!-- 修复：Vue 使用双花括号展示变量 -->
        {{ emp.name }} - {{ emp.role }} ({{ emp.email }})
      </li>
    </ul>

    <h2>添加员工</h2>
    <div style="display: flex; gap: 10px; margin-bottom: 20px;">
      <input v-model="newName" placeholder="Name" style="flex: 1; padding: 8px;"/>
      <input v-model="newRole" placeholder="Role" style="flex: 1; padding: 8px;"/>
      <button @click="addEmp" style="padding: 8px 16px; cursor: pointer;">录入</button>
    </div>
  </div>
</template>