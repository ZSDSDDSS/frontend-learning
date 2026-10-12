<script setup>
import { ref } from 'vue'
const todos = ref([
  { id: 1, text: '整理活动报名名单', done: false },
  { id: 2, text: '联系场地负责人', done: false },
  { id: 3, text: '准备审核说明材料', done: true }
])
const newText = ref('')

const doneCount = () => todos.value.filter(item => item.done).length

// 添加待办
const addTodo = () => {
  if(newText.value.trim()){
    todos.value.push({
      id: Date.now(),
      text: newText.value,
      done: false
    })
    newText.value = ''
  }
}
// 删除待办
const delTodo = (id) => {
  todos.value = todos.value.filter(item => item.id !== id)
}
</script>

<template>
  <input v-model="newText" placeholder="输入待办" />
  <button @click="addTodo">添加</button>
  <ul>
    <li v-for="item in todos" :key="item.id">
      <input type="checkbox" v-model="item.done"/>
      <span :style="{textDecoration: item.done ? 'line-through':'none'}">
        {{ item.text }}
      </span>
      <button @click="delTodo(item.id)">删除</button>
    </li>
  </ul>
  <p>已完成 {{ doneCount() }} / {{ todos.length }}</p>
</template>

<style>
ul{list-style:none;padding:0;}
li{margin:8px 0;}
</style>
