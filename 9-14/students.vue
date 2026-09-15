<template>
  <div class="profile-card" :class="{ onDuty: isOnDuty, leave: !isOnDuty }">
    <!-- 头像 -->
    <img class="avatar" :src="userInfo.avatar" alt="用户头像" />

    <!-- 姓名 -->
    <h2 class="username">{{ userInfo.name }}</h2>

    <!-- 角色标签：对象形式类名绑定 -->
    <span class="role-tag" :class="{
      organizer: userInfo.role === 'organizer',
      auditor: userInfo.role === 'auditor',
      student: userInfo.role === 'student'
    }">
      {{ roleText }}
    </span>

    <!-- 在岗 / 请假状态 -->
    <div class="status-block">
      <span class="status-dot" :class="{ onDuty: isOnDuty, leave: !isOnDuty }"></span>
      <span class="status-text" :class="{ onDuty: isOnDuty, leave: !isOnDuty }">
        {{ onDutyText }}
      </span>
      <button class="status-btn" @click="toggleOnDuty">切换在岗/请假</button>
    </div>

    <!-- 技能标签列表 -->
    <div class="skill-block">
      <p>技能标签</p>
      <div class="skill-group">
        <span class="skill-tag" v-for="skill in skillTags" :key="skill">
          {{ skill }}
        </span>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, reactive } from 'vue'

// 用户信息
const userInfo = reactive({
  name: '张瑞',
  avatar: 'https://picsum.photos/id/1012/160/160',
  role: 'organizer'
})

// 在岗状态：true 在岗，false 请假
const isOnDuty = ref(true)

// 技能标签数组
const skillTags = ref(['活动策划', '现场统筹', '文案编辑', '海报设计', '沟通协调'])

// 角色中文显示
const roleText = computed(() => {
  const map = {
    organizer: '活动组织者',
    auditor: '审核员',
    student: '学生'
  }
  return map[userInfo.role]
})

// 根据状态显示文字
const onDutyText = computed(() => {
  return isOnDuty.value ? '在岗' : '请假'
})

// 切换状态
const toggleOnDuty = () => {
  isOnDuty.value = !isOnDuty.value
}
</script>

<style scoped>
.profile-card {
  width: 420px;
  padding: 32px;
  border-radius: 20px;
  background: #ffffff;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.08);
  text-align: center;
  transition: all 0.3s ease;
}

/* 在岗：绿色主题 */
.profile-card.onDuty {
  border-top: 5px solid #22c55e;
  box-shadow: 0 8px 24px rgba(34, 197, 94, 0.25);
}

/* 请假：红色主题 */
.profile-card.leave {
  border-top: 5px solid #ef4444;
  box-shadow: 0 8px 24px rgba(239, 68, 68, 0.25);
}

.avatar {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  object-fit: cover;
  border: 3px solid #e5e7eb;
}

.profile-card.onDuty .avatar {
  border-color: #22c55e;
}

.profile-card.leave .avatar {
  border-color: #ef4444;
}

.username {
  margin: 16px 0 8px;
  font-size: 22px;
  color: #111827;
}

/* 角色标签样式 */
.role-tag {
  display: inline-block;
  padding: 4px 12px;
  border-radius: 999px;
  color: #fff;
  font-size: 14px;
  margin-bottom: 24px;
}

.organizer {
  background-color: #3b82f6;
}

.auditor {
  background-color: #8b5cf6;
}

.student {
  background-color: #f59e0b;
}

/* 状态区域 */
.status-block {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  margin-bottom: 24px;
}

.status-dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: #9ca3af;
}

.status-dot.onDuty {
  background: #22c55e;
  box-shadow: 0 0 0 4px rgba(34, 197, 94, 0.2);
}

.status-dot.leave {
  background: #ef4444;
  box-shadow: 0 0 0 4px rgba(239, 68, 68, 0.2);
}

.status-text {
  font-size: 16px;
  font-weight: 600;
}

.status-text.onDuty {
  color: #22c55e;
}

.status-text.leave {
  color: #ef4444;
}

.status-btn {
  padding: 6px 14px;
  border-radius: 8px;
  border: 1px solid #d1d5db;
  background: #fff;
  color: #374151;
  cursor: pointer;
  transition: 0.2s;
}

.profile-card.onDuty .status-btn:hover {
  background: #22c55e;
  color: #fff;
  border-color: #22c55e;
}

.profile-card.leave .status-btn:hover {
  background: #ef4444;
  color: #fff;
  border-color: #ef4444;
}

/* 技能标签 */
.skill-block p {
  margin-bottom: 10px;
  color: #374151;
}

.skill-group {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 8px;
}

.skill-tag {
  padding: 4px 10px;
  background: #f3f4f6;
  border-radius: 6px;
  font-size: 14px;
  color: #374151;
}

.profile-card.onDuty .skill-tag {
  border-left: 3px solid #22c55e;
}

.profile-card.leave .skill-tag {
  border-left: 3px solid #ef4444;
}
</style>
