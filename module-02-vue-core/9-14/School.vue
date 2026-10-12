<script setup>
import { ref } from 'vue'

const activityList = ref([
  {
    title: '2026 春季校园歌手大赛',
    status: 'signing',
    cover: 'https://images.unsplash.com/photo-1511671782779-c97d3d27a1d4?w=600&auto=format&fit=crop',
    desc: '用歌声点亮春天，展现你的舞台魅力。'
  },
  {
    title: '校园篮球争霸赛',
    status: 'draft',
    cover: 'https://images.unsplash.com/photo-1546519638-68e109498ffc?w=600&auto=format&fit=crop',
    desc: '热血对决，逐梦球场，等你来挑战。'
  }
])

const currentIndex = ref(0)
const activity = ref(activityList.value[currentIndex.value])

const prev = () => {
  currentIndex.value =
    currentIndex.value === 0
      ? activityList.value.length - 1
      : currentIndex.value - 1

  activity.value = activityList.value[currentIndex.value]
}

const next = () => {
  currentIndex.value =
    currentIndex.value === activityList.value.length - 1
      ? 0
      : currentIndex.value + 1

  activity.value = activityList.value[currentIndex.value]
}
</script>

<template>
  <div class="page">
    <article class="activity-card">
      <div class="poster">
        <img :src="activity.cover" :alt="activity.title" />

        <span class="tag" :class="{ 'tag--signing': activity.status === 'signing' }">
          {{ activity.status === 'signing' ? '报名中' : '草稿' }}
        </span>
      </div>

      <h3>{{ activity.title }}</h3>
      <p class="desc">{{ activity.desc }}</p>

      <button class="offline-btn" :disabled="activity.status !== 'draft'">
        下架
      </button>

      <div class="btn-group">
        <button @click="prev">上一张</button>
        <button @click="next">下一张</button>
      </div>
    </article>
  </div>
</template>

<style>
.page {
  display: flex;
  justify-content: center;
  padding: 30px;
  background: #f5f7fb;
}

.activity-card {
  width: 360px;
  border-radius: 20px;
  overflow: hidden;
  background: #ffffff;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.15);
  transition: transform 0.3s ease;
}

.activity-card:hover {
  transform: translateY(-6px);
}

.poster {
  position: relative;
  width: 100%;
  height: 200px;
  overflow: hidden;
}

.poster img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.4s ease;
}

.activity-card:hover .poster img {
  transform: scale(1.08);
}

.tag {
  position: absolute;
  top: 12px;
  left: 12px;
  padding: 6px 14px;
  background: rgba(0, 0, 0, 0.5);
  color: #fff;
  border-radius: 20px;
  font-size: 13px;
}

.tag--signing {
  background: #ff4757;
}

h3 {
  margin: 16px 16px 6px;
  font-size: 20px;
  color: #222;
}

.desc {
  margin: 0 16px 14px;
  font-size: 14px;
  color: #666;
  line-height: 1.6;
}

.offline-btn {
  margin: 0 16px;
  padding: 8px 14px;
  border: none;
  border-radius: 8px;
  background: #ff6b6b;
  color: #fff;
  cursor: pointer;
}

.offline-btn:disabled {
  background: #c0c4cc;
  cursor: not-allowed;
}

.btn-group {
  margin: 16px;
  display: flex;
  gap: 12px;
}

button {
  flex: 1;
  padding: 10px;
  border: none;
  border-radius: 10px;
  background: linear-gradient(135deg, #667eea, #764ba2);
  color: #fff;
  font-size: 15px;
  cursor: pointer;
  transition: all 0.25s ease;
}

button:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 10px 20px rgba(102, 126, 234, 0.4);
}

button:disabled {
  background: #c0c4cc;
  cursor: not-allowed;
}
</style>
