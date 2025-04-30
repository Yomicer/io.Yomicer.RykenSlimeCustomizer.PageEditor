<template>
  <div class="coming-soon-container">
    <h2>敬请期待...</h2>
    <p>功能正在开发中，很快就会和大家见面！</p>

    <!-- 你提供的 HTML 内容 -->
    <div id="Content2" class="content">
      <label for="dropFromInput" class="wide-label">看到这么好的网站该怎么做呢？</label>
      <select id="dropFromInput">
        <option value="">三连</option>
        <option value="">点赞</option>
        <option value="true">收藏</option>
        <option value="false">转发</option>
      </select>
      <br />
      <a href="https://space.bilibili.com/272421943" target="_blank" rel="noopener noreferrer"
         class="bilibili-link">
        点击跳转到我的B站主页
      </a>
    </div>
  </div>
</template>

<script setup>
// 可以在这里添加逻辑，当前页面为空时可不写内容
</script>

<style scoped>
.coming-soon-container {
  text-align: center;
  padding: 40px 20px;
}

.content {
  margin-top: 30px;
}

.wide-label {
  display: block;
  margin: 10px 0 5px;
  font-size: 16px;
  font-weight: bold;
}

select {
  padding: 8px 12px;
  width: 200px;
  font-size: 14px;
  border-radius: 4px;
  border: 1px solid #ccc;
  margin-bottom: 15px;
}

.bilibili-link {
  display: inline-block;
  padding: 8px 16px;
  background-color: #007bff;
  color: white;
  text-decoration: none;
  border-radius: 4px;
  text-align: center;
  margin-top: 10px;
  transition: background-color 0.3s ease;
}

.bilibili-link:hover {
  background-color: #0056b3;
}
</style>