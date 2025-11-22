<script setup>
import { ref, computed } from 'vue';

// --- 1. 假数据 (Mock Data) ---
const mockDatabase = [
  "百度地图", "百度网盘", "百度翻译", "百度贴吧", "百度文库",
  "前端开发教程", "Vite 构建工具", "Vue3 入门", "React 和 Vue 哪个好",
  "今天天气怎么样", "如何制作红烧肉", "Baidu Search Demo",
  "ChatGPT vs DeepSeek", "JavaScript 高级程序设计"
];

// --- 2. 响应式状态 ---
const keyword = ref(''); // 输入框的内容
const isFocused = ref(false); // 输入框是否聚焦

// --- 3. 核心逻辑：联想搜索 ---
const suggestions = computed(() => {
  if (!keyword.value.trim()) return []; 
  return mockDatabase.filter(item => 
    item.toLowerCase().includes(keyword.value.toLowerCase())
  );
});

// 是否显示下拉框
const showDropdown = computed(() => {
  return isFocused.value && suggestions.value.length > 0;
});

// --- 4. 事件处理 ---
const selectItem = (item) => {
  keyword.value = item;
  isFocused.value = false; 
  console.log(`正在搜索: ${item}`);
};

const handleBlur = () => {
  setTimeout(() => {
    isFocused.value = false;
  }, 200);
};

const doSearch = () => {
  if(keyword.value) {
    alert(`模拟搜索跳转：${keyword.value}`);
  }
};
</script>

<template>
  <div class="container">
    <div class="logo-wrapper">
      <img 
        src="https://www.baidu.com/img/PCtm_d9c8750bed0b3c7d089fa7d55720d6cf.png" 
        alt="Baidu Logo" 
        class="logo"
      />
    </div>

    <div class="search-wrapper">
      <span class="input-container" :class="{ 'active': isFocused }">
        <input 
          type="text" 
          v-model="keyword"
          @focus="isFocused = true"
          @blur="handleBlur"
          @keydown.enter="doSearch"
          class="search-input"
        />
        <span class="camera-icon">📷</span>
      </span>
      <button class="search-btn" @click="doSearch">百度一下</button>

      <ul class="dropdown" v-if="showDropdown">
        <li v-for="(item, index) in suggestions" :key="index" @click="selectItem(item)">
          {{ item }}
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
* { box-sizing: border-box; }
/* 修改 .container 为全屏绝对居中 */
.container {
  display: flex;
  flex-direction: column;
  align-items: center;     /* 水平居中 */
  justify-content: center; /* 垂直居中 */
  height: 100vh;           /* 关键修改：强制占满屏幕高度 */
  width: 100%;             /* 占满宽度 */
  padding-bottom: 100px;   /* 小技巧：视觉修正，稍微往上提一点点，不然视觉重心会觉得掉下去了 */
}
.logo-wrapper { margin-bottom: 20px; }
.logo { height: 129px; width: 270px; }
.search-wrapper { position: relative; display: flex; width: 650px; height: 44px; }
.input-container {
  flex: 1; display: flex; align-items: center; border: 2px solid #c4c7ce;
  border-right: none; border-radius: 10px 0 0 10px; padding: 0 10px;
  transition: border-color 0.2s; background: #fff;
}
.input-container.active { border-color: #4e6ef2; }
.search-input { flex: 1; border: none; outline: none; height: 100%; font-size: 16px; color: #333; }
.camera-icon { cursor: pointer; font-size: 20px; color: #999; padding-right: 5px; }
.search-btn {
  width: 108px;
  height: 100%;
  background-color: #4e6ef2;
  color: white;
  font-size: 17px;
  font-weight: 500;
  border: none;
  border-radius: 0 10px 10px 0; /* 只有右边有圆角 */
  cursor: pointer;
  transition: background-color 0.2s;
  
  padding: 0;              /* 1. 清除浏览器默认的按钮内边距 */
  white-space: nowrap;     /* 2. 强制文字不换行 */
  display: flex;           /* 3. 使用 Flex 布局 */
  align-items: center;     /* 4. 垂直居中 */
  justify-content: center; /* 5. 水平居中 */
  line-height: normal;     /* 6. 重置行高，防止文字偏下 */
}
.search-btn:hover { 
  background-color: #4662d9;
}
.dropdown {
  position: absolute; top: 44px; left: 0; width: calc(100% - 108px);
  background: #fff; border: 1px solid #ccc; border-top: none;
  border-radius: 0 0 10px 10px; list-style: none; padding: 0; margin: 0;
  box-shadow: 0 4px 10px rgba(0,0,0,0.1); z-index: 100; overflow: hidden;
}
.dropdown li { padding: 10px 14px; cursor: pointer; font-size: 15px; color: #333; text-align: left; }
.dropdown li:hover { background-color: #f5f5f6; color: #4e6ef2; }
</style>