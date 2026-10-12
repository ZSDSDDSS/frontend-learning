<script setup>
import { ref } from "vue";

// 朋友圈模拟数据，包含1/2/3/4/9张图片场景
const posts = ref([
  {
    id: 1,
    name: "陈砚舟",
    avatar: "https://picsum.photos/id/1005/100/100",
    text: "周末爬山九连拍，山顶的风真的很舒服。",
    time: "10 分钟前",
    likes: 26,
    comments: 8,
    images: [
      "https://picsum.photos/seed/m11/400/400",
      "https://picsum.photos/seed/m12/400/400",
      "https://picsum.photos/seed/m13/400/400",
      "https://picsum.photos/seed/m14/400/400",
      "https://picsum.photos/seed/m15/400/400",
      "https://picsum.photos/seed/m16/400/400",
      "https://picsum.photos/seed/m17/400/400",
      "https://picsum.photos/seed/m18/400/400",
      "https://picsum.photos/seed/m19/400/400",
    ],
  },
  {
    id: 2,
    name: "苏晚晴",
    avatar: "https://picsum.photos/id/1027/100/100",
    text: "实训室今天的下午茶，四个人分刚好。",
    time: "1 小时前",
    likes: 41,
    comments: 5,
    images: [
      "https://picsum.photos/seed/m21/400/400",
      "https://picsum.photos/seed/m22/400/400",
      "https://picsum.photos/seed/m23/400/400",
      "https://picsum.photos/seed/m24/400/400",
    ],
  },
  {
    id: 3,
    name: "周予安",
    avatar: "https://picsum.photos/id/1012/100/100",
    text: "只发一张，晚霞。这张图是横着的，看看单图会怎么显示。",
    time: "3 小时前",
    likes: 63,
    comments: 12,
    images: ["https://picsum.photos/seed/m31/720/480"],
  },
  {
    id: 4,
    name: "林一鸣",
    avatar: "https://picsum.photos/id/1025/100/100",
    text: "两图对比：改造前 / 改造后。",
    time: "昨天 21:40",
    likes: 18,
    comments: 3,
    images: [
      "https://picsum.photos/seed/m41/400/400",
      "https://picsum.photos/seed/m42/400/400",
    ],
  },
  {
    id: 5,
    name: "许知微",
    avatar: "https://picsum.photos/id/1014/100/100",
    text: "三张图走完一整天，早中晚各一张。",
    time: "昨天 18:05",
    likes: 30,
    comments: 6,
    images: [
      "https://picsum.photos/seed/m51/400/400",
      "https://picsum.photos/seed/m52/400/400",
      "https://picsum.photos/seed/m53/400/400",
    ],
  },
]);

// 根据图片数量返回动态类名
function gridClass(count) {
  return "pic-grid pic-grid--" + count;
}

// 点赞功能
function like(index) {
  posts.value[index].likes += 1;
}
</script>

<template>
  <div class="page">
    <header class="page__head">
      <h2 class="page__title">朋友圈动态</h2>
      <p class="page__desc">
        图片数量决定布局：1张原图；2、4张两列；3、5‑9张三列九宫格
      </p>
    </header>

    <ul class="feed">
      <li v-for="(post, index) in posts" :key="post.id" class="feed__item">
        <img class="feed__avatar" :src="post.avatar" :alt="post.name" />
        <div class="feed__body">
          <p class="feed__name">{{ post.name }}</p>
          <p class="feed__text">{{ post.text }}</p>

          <!-- 图片容器：一套模板，动态class切换布局，没有v-if -->
          <div :class="gridClass(post.images.length)">
            <img
              v-for="(img, i) in post.images"
              :key="i"
              class="pic"
              :src="img"
              :alt="post.name + ' 的图片 ' + (i + 1)"
            />
          </div>

          <div class="feed__foot">
            <span class="feed__time">{{ post.time }}</span>
            <button class="feed__action" @click="like(index)">
              赞 {{ post.likes }}
            </button>
            <span class="feed__action">评论 {{ post.comments }}</span>
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.page {
  max-width: 620px;
  margin: 40px auto;
  padding: 0 16px;
}
.page__head {
  margin-bottom: 8px;
}
.page__title {
  margin: 0;
  font-size: 22px;
  color: #1f2329;
}
.page__desc {
  margin: 6px 0 0;
  font-size: 13px;
  color: #8a919f;
}

/* 动态列表 */
.feed {
  margin: 16px 0 0;
  padding: 0;
  list-style: none;
}
.feed__item {
  display: flex;
  gap: 12px;
  padding: 18px 0;
  border-bottom: 1px solid #f1f3f6;
}
.feed__item:last-child {
  border-bottom: none;
}
.feed__avatar {
  flex: none;
  width: 42px;
  height: 42px;
  border-radius: 6px;
  object-fit: cover;
  background: #f1f3f6;
}
.feed__body {
  flex: 1;
  min-width: 0;
}
.feed__name {
  margin: 0;
  font-size: 15px;
  font-weight: 600;
  color: #5b6b9c;
}
.feed__text {
  margin: 6px 0 10px;
  font-size: 15px;
  line-height: 1.6;
  color: #1f2329;
  word-break: break-word;
}
.feed__foot {
  display: flex;
  gap: 16px;
  align-items: center;
  margin-top: 10px;
  font-size: 13px;
  color: #8a919f;
}
.feed__action {
  padding: 0;
  border: none;
  background: none;
  font-size: 13px;
  color: #5b6b9c;
  cursor: pointer;
}

/* ========== 核心图片Grid布局 ========== */
/* 默认基础：3列，5‑9张图直接复用这套，不用写额外css */
.pic-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 4px;
  width: 258px;
}

/* 单张图片：关闭网格，按原图比例 */
.pic-grid--1 {
  display: block;
  width: auto;
}

/* 2张、4张图片 → 改为2列布局 */
.pic-grid--2,
.pic-grid--4 {
  grid-template-columns: repeat(2, 1fr);
}

/* 网格内图片：强制正方形，cover裁剪不变形 */
.pic {
  display: block;
  width: 100%;
  height: 100%;
  aspect-ratio: 1 / 1;
  object-fit: cover;
  border-radius: 4px;
  background: #f1f3f6;
  cursor: pointer;
}

/* 单图特殊覆盖：取消正方形约束，限制最大240px */
.pic-grid--1 .pic {
  width: auto;
  height: auto;
  aspect-ratio: auto;
  max-width: 240px;
  max-height: 240px;
  border-radius: 6px;
}

.pic:hover {
  opacity: 0.92;
}
</style>
