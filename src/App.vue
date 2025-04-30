<!-- src/App.vue -->

<template>
    <div id="app">
      <h1 style="text-align: center; margin-top: 30px;">Slimefun 物品配置生成器</h1>
      <ItemYmlForm />
    </div>
  </template>
  
  <script setup>
  import ItemYmlForm from './components/ItemYmlForm.vue'
  </script>
  
  <style>
  body {
    font-family: Arial, sans-serif;
    background-color: #f8f9fa;
    margin: 0;
    padding: 0;
  }
  
  #app {
    max-width: 1200px;
    margin: auto;
    padding: 20px;
  }
  </style>