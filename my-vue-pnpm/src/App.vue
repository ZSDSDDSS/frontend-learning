<script setup>
import { ref, computed } from "vue";
import { ElMessage } from "element-plus";

const students = [
  {
    id: "20230101",
    name: "沈不渝",
    className: "软件 2301",
    direction: "前端开发",
    score: 92,
    status: "active",
    phone: "13845671101",
    comment: "优秀，代码风格整洁，动手能力强"
  },
  {
    id: "20230102",
    name: "方休",
    className: "软件 2301",
    direction: "后端开发",
    score: 85,
    status: "active",
    phone: "13845671102",
    comment: "优秀，逻辑思维突出，擅长接口设计"
  },
  {
    id: "20230103",
    name: "谢临渊",
    className: "软件 2302",
    direction: "前端开发",
    score: 78,
    status: "intern",
    phone: "13845671103",
    comment: "良好，基础扎实，实习表现稳定"
  },
  {
    id: "20230104",
    name: "陆知珩",
    className: "软件 2302",
    direction: "数据开发",
    score: 64,
    status: "leave",
    phone: "13845671104",
    comment: "及格，学习主动性不足，目前休学"
  },
  {
    id: "20230105",
    name: "温辞",
    className: "软件 2302",
    direction: "后端开发",
    score: 88,
    status: "active",
    phone: "13845671105",
    comment: "优秀，项目能力突出，乐于帮助同学"
  },
  {
    id: "20230106",
    name: "江叙白",
    className: "软件 2303",
    direction: "测试开发",
    score: 71,
    status: "intern",
    phone: "13845671106",
    comment: "良好，细心严谨，适合测试方向"
  },
  {
    id: "20230107",
    name: "裴砚",
    className: "软件 2303",
    direction: "前端开发",
    score: 95,
    status: "active",
    phone: "13845671107",
    comment: "优秀，学习能力极强，竞赛获奖"
  },
  {
    id: "20230108",
    name: "苏砚辞",
    className: "软件 2303",
    direction: "数据开发",
    score: 59,
    status: "leave",
    phone: "13845671108",
    comment: "不及格，课程多次挂科，办理休学"
  },
];

const viewMode = ref("table");

const STATUS_MAP = {
  active: { text: "在读", type: "success" },
  intern: { text: "实习中", type: "warning" },
  leave: { text: "休学", type: "info" },
};

function scoreColor(score) {
  if (score >= 85) return "#0f9d58";
  if (score >= 70) return "#e6a23c";
  return "#f56c6c";
}

function getLevel(score) {
  if (score >= 85) return "优秀";
  if (score >=70) return "良好";
  if (score >=60) return "及格";
  return "不及格";
}

function showDetail(student) {
  ElMessage.success(
    `${student.name}（${student.id}）：${student.direction}，成绩 ${student.score}，评价：${getLevel(student.score)}`
  );
}

const avgScore = computed(() => {
  const sum = students.reduce((total, item) => total + item.score, 0)
  return (sum / students.length).toFixed(1)
})
</script>

<template>
  <div class="page">
    <header class="page__head">
      <div>
        <h2 class="page__title">学生名单</h2>
        <p class="page__desc">
          共 {{ students.length }} 人 · 平均分：{{ avgScore }} · 切换视图用的是 v-if / v-else-if / v-else
        </p>
      </div>

      <el-radio-group v-model="viewMode">
        <el-radio-button value="table">表格</el-radio-button>
        <el-radio-button value="card">卡片</el-radio-button>
        <el-radio-button value="list">名单</el-radio-button>
      </el-radio-group>
    </header>

    <el-table v-if="viewMode === 'table'" :data="students" stripe border>
      <el-table-column prop="id" label="学号" width="120" />
      <el-table-column prop="name" label="姓名" width="110" />
      <el-table-column prop="className" label="班级" width="120" />
      <el-table-column prop="direction" label="方向" />
      <el-table-column prop="score" label="成绩" width="100" sortable>
        <template #default="scope">
          <span class="score" :style="{ color: scoreColor(scope.row.score) }">{{
            scope.row.score
          }}</span>
        </template>
      </el-table-column>
      <el-table-column label="等级" width="80">
        <template #default="scope">
          {{ getLevel(scope.row.score) }}
        </template>
      </el-table-column>
      <el-table-column prop="phone" label="联系电话" width="140" />
      <el-table-column label="状态" width="110">
        <template #default="scope">
          <el-tag :type="STATUS_MAP[scope.row.status].type" effect="light">
            {{ STATUS_MAP[scope.row.status].text }}
          </el-tag>
        </template>
      </el-table-column>
      <el-table-column label="评语" min-width="200">
        <template #default="scope">
          {{ scope.row.comment }}
        </template>
      </el-table-column>
      <el-table-column label="操作" width="90">
        <template #default="scope">
          <el-button link type="primary" @click="showDetail(scope.row)"
            >查看</el-button
          >
        </template>
      </el-table-column>
    </el-table>

    <el-row v-else-if="viewMode === 'card'" :gutter="16">
      <el-col
        v-for="item in students"
        :key="item.id"
        :xs="24"
        :sm="12"
        :md="6"
        :lg="4"
        class="card-col"
      >
        <div class="stu-card">
          <div class="card-layer card-top">
            <el-avatar :size="48" class="avatar">{{
              item.name.charAt(0)
            }}</el-avatar>
            <div>
              <p class="stu-card__name">{{ item.name }}</p>
              <p class="stu-card__id">{{ item.id }} · {{ item.className }}</p>
            </div>
          </div>

          <div class="card-layer card-middle">
            <div class="stu-card__tags">
              <el-tag size="small" effect="plain">{{ item.direction }}</el-tag>
              <el-tag size="small" :type="STATUS_MAP[item.status].type" effect="light">
                {{ STATUS_MAP[item.status].text }}
              </el-tag>
              <el-tag size="small">{{ getLevel(item.score) }}</el-tag>
            </div>
            <p class="stu-card__score">成绩 {{ item.score }}</p>
            <el-progress
              :percentage="item.score"
              :color="scoreColor(item.score)"
              :stroke-width="8"
              :show-text="false"
            />
            <p class="stu-card-phone">电话：{{ item.phone }}</p>
            <p class="stu-card-comment">评语：{{ item.comment }}</p>
          </div>

          <div class="card-layer card-bottom">
            <el-button link type="primary" @click="showDetail(item)"
              >查看详情</el-button
            >
          </div>
        </div>
      </el-col>
    </el-row>

    <ul v-else class="name-list">
      <li v-for="item in students" :key="item.id" class="name-list__item">
        <el-avatar :size="32" class="avatar">{{ item.name.charAt(0) }}</el-avatar>
        <span class="name-list__name">{{ item.name }}</span>
        <span class="name-list__meta"
          >{{ item.id }} · {{ item.className }} · {{ item.direction }}</span
        >
        <span class="score" :style="{ color: scoreColor(item.score) }">{{
          item.score
        }}</span>
        <span>{{ getLevel(item.score) }}</span>
        <el-tag size="small" :type="STATUS_MAP[item.status].type" effect="light">
          {{ STATUS_MAP[item.status].text }}
        </el-tag>
        <span class="phone-text">{{ item.phone }}</span>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.page {
  width: 100%;
  min-height: 100vh;
  margin: 0;
  padding: 40px 24px;
  box-sizing: border-box;
  background: linear-gradient(135deg, #fff7e6, #ffe4c4, #ffd29a);
}

.page__head {
  display: flex;
  flex-wrap: wrap;
  gap: 16px;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 16px;
}

.page__title {
  margin: 0;
  font-size: 22px;
  color: #5d4037;
}

.page__desc {
  margin: 6px 0 0;
  font-size: 13px;
  color: #8d6e63;
}

.score {
  font-weight: 600;
}

.avatar {
  background: #4285f4;
  color: #fff;
}

.card-col {
  margin-bottom: 16px;
}

.stu-card {
  background: #ffffff;
  border-radius: 16px;
  box-shadow: 0 4px 16px rgba(255, 170, 80, 0.25);
  overflow: hidden;
}

.card-layer {
  padding: 20px 24px;
}

.card-top {
  display: flex;
  gap: 12px;
  align-items: center;
  border-bottom: 1px dashed #ffe0b2;
}

.stu-card__name {
  margin: 0;
  font-size: 16px;
  font-weight: 600;
  color: #1f2329;
}

.stu-card__id {
  margin: 4px 0 0;
  font-size: 12px;
  color: #8a919f;
}

.card-middle {
  border-bottom: 1px dashed #ffe0b2;
}

.stu-card__tags {
  display: flex;
  gap: 8px;
  margin: 14px 0;
}

.stu-card__score {
  margin: 0 0 6px;
  font-size: 13px;
  color: #5c6470;
}

.stu-card-phone {
  font-size:13px;
  color:#666;
  margin:8px 0 4px;
}
.stu-card-comment {
  font-size:12px;
  color:#777;
  margin:0;
}

.card-bottom {
  padding: 16px 24px;
}

.name-list {
  margin: 0;
  padding: 0;
  list-style: none;
  border: 1px solid #ebeef5;
  border-radius: 8px;
  overflow: hidden;
  background: #fff;
}

.name-list__item {
  display: flex;
  gap: 12px;
  align-items: center;
  padding: 10px 16px;
  background: #fff;
  border-bottom: 1px solid #f2f3f5;
}

.name-list__item:last-child {
  border-bottom: none;
}

.name-list__item:hover {
  background: #fff3e0;
}

.name-list__name {
  width: 80px;
  font-weight: 600;
  color: #1f2329;
}

.name-list__meta {
  flex: 1;
  font-size: 13px;
  color: #8a919f;
}
.phone-text{
  font-size:13px;
  color:#666;
}
</style>
