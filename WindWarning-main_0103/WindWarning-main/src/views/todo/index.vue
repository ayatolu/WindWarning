<script setup>
import { ref } from 'vue'
import '@fortawesome/fontawesome-free/css/all.min.css'
import todoList from '@/components/todo/todoList.vue'
import turbine from '@/components/todo/turbine.vue'

// 选择的起始和结束时间
const startDate = ref(null)
const endDate = ref(null)


const infoList = ref([
  {
    id: 1,
    name: '全部'
  },
  {
    id: 2,
    name: '预警一级'
  },
  {
    id: 3,
    name: '预警二级'
  },
  {
    id: 4,
    name: '通知'
  }
])

const infoId = ref(infoList.value[0].id)
</script>

<template>
  <div class="header">
    <div class="header-left">
      <span class="info"></span>
      <el-form-item label="信息类别" class="info-category">
        <el-select
          v-model="infoId"
          placeholder="全部"
          value-key="id"
          style="--el-input-text-color: white; width: 100px"
        >
          <el-option v-for="info in infoList" :key="info.id" :label="info.name" :value="info.id"></el-option>
        </el-select>
      </el-form-item>
    </div>
    <div class="header-right">
      <div class="time-filter">
        <el-button type="primary">全部</el-button>
        <el-button type="primary">最近七天</el-button>
        <div class="date-picker-container">
          <el-date-picker
            v-model="startDate"
            type="datetime"
            placeholder="开始时间"
            format="YYYY-MM-DD HH:mm:ss"
            value-format="YYYY-MM-DD HH:mm:ss"
          ></el-date-picker>
          <span class="date-separator">至</span>
          <el-date-picker
            v-model="endDate"
            type="datetime"
            placeholder="结束时间"
            format="YYYY-MM-DD HH:mm:ss"
            value-format="YYYY-MM-DD HH:mm:ss"
          ></el-date-picker>
        </div>
      </div>
    </div>
  </div>
  <div class="main">
    <div class="turbine">
      <turbine />
    </div>
    <div class="todoList">
      <todoList />
    </div>
  </div>
</template>

<style scoped>
.header {
  display: flex;
  align-items: center;
  padding: 10px;
  border: 1px solid #164b6d;
}

.header-left {
  display: flex;
  align-items: center;
  margin-right: 20px;
}

.header-left::after {
  content: '';
  height: 30px;
  width: 2px;
  background-color: #059af6;
  margin-left: 15px;
}
.info {
  width: 16px;
  height: 16px;
  background-color: #2098b2;
  color: white;
  border-radius: 3px;
  margin-right: 5px;
}
.info-category {
  display: flex;
  align-items: center;
  margin-bottom: 0;
}

.info-category .el-form-item__label {
  margin-right: 10px;
}

.header-right {
  flex-grow: 1;
  display: flex;
  justify-content: flex-start;
}

.time-filter {
  display: flex;
  align-items: center;
}

.date-picker-container {
  display: flex;
  align-items: center;
  margin-left: 10px;
}

.date-separator {
  margin: 0 10px;
}

.time-filter .el-button {
  margin-right: 10px;
}

.main {
  display: flex;
  margin-top: 10px;
  border: 1px solid #164b6d;
  height: 75vh;
  overflow: hidden;
}

.todoList {
  flex: 3;
  margin: 5px;
  border: 1px solid #164b6d;
}

.turbine {
  flex: 1;
  margin: 5px;
  border: 1px solid #164b6d;
  overflow: auto;
}
/* 表单 */
:deep(.el-form-item__label) {
  color: white !important;
}

:deep(.el-select__wrapper) {
  background-color: transparent !important;
  color: white;
}

:deep(.el-select-dropdown) {
  background-color: rgba(0, 0, 0, 0.5) !important;
  color: white;
}

:deep(.el-select-dropdown__item.hover),
:deep(.el-select-dropdown__item:hover) {
  background-color: rgba(255, 255, 255, 0.1) !important;
}

:deep(.el-input__wrapper) {
  background-color: transparent !important;
}

:deep(.el-input__inner) {
  background-color: transparent !important;
  color: white !important;
}

:deep(.el-input) {
  --el-input-bg-color: transparent !important;
  --el-input-text-color: white !important;
}
</style>
