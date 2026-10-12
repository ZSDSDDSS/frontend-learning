<script setup>
import { ref } from 'vue'
const isSignedUp = ref(false)
const count = ref(0)
const handleClick = () => {
  if(count.value >= 10){
    // 达到10次，重置全部状态回到初始
    count.value = 0
    isSignedUp.value = false
    return
  }
  isSignedUp.value = !isSignedUp.value
  count.value ++;
}
</script>

<template>
  <button @click="handleClick" class="btn">
    {{ isSignedUp ? '退订' : '报名' }}
  </button>

  <p :class="isSignedUp ? 'status--done' : 'status--idle'" class="show">
    {{ isSignedUp ? '已报名' : '未报名' }}
  </p>

  <p class="show">
    {{ count >= 10 ? '次数超过' : `你已经点击了 ${count} 次` }}
  </p>

  <p class="show">hello world</p>
</template>

<style>
button{
  width: 80px;
  height: 40px;
  background-color: #5CA67B;
  color: #FFFFFF;
  border: none;
  border-radius: 8px;
}

.show {
  width: 40%;
  font-size: 24pt;
  text-align: center;
  background-color: antiquewhite;
  padding-top: 10px;
  padding-bottom: 10px;
}

/* 只有status--done才改背景和文字，未报名保持show原本样式不变 */
.status--done {
  background-color: #5CA67B !important;
  color: white;
}
</style>
