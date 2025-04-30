<template>
  <div id="app">
    <!-- 页面标题 -->
    <h1 style="text-align: center; margin-top: 30px;">RykenSlimeCustomizer 配置文件生成器</h1>

    <!-- Tabs 栏 -->
    <div class="tabs">
      <button
        v-for="tab in tabs"
        :key="tab.id"
        class="tab-button"
        :class="{ 'active': currentTab === tab.component }"
        @click="currentTab = tab.component"
      >
        {{ tab.label }}
      </button>
    </div>

    <!-- 动态组件区域 -->
    <div class="tab-content">
      <component :is="currentTab" />
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import ItemYmlForm from './components/ItemYmlForm.vue'
import ComingSoon from './components/ComingSoon.vue'

// Tabs 定义，关联 label、id、component
const tabs = [
  { id: 'itemYml', label: 'item.yml', component: ItemYmlForm },
  { id: 'comingSoon', label: '小彩蛋 ...', component: ComingSoon }
]

// 当前激活的 Tab，默认是第一个
const currentTab = ref(tabs[0].component)
</script>

<style scoped>
.tabs {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin-bottom: 20px;
}

.tab-button {
  padding: 10px 20px;
  border: none;
  background-color: #e0e0e0;
  color: #333;
  font-size: 16px;
  cursor: pointer;
  border-radius: 5px;
  transition: all 0.3s ease;
}

/* 当鼠标悬停时微调背景色 */
.tab-button:hover {
  background-color: #bef5ff;
}

</style>