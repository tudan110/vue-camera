<template>
  <div id="app">
    <h1>摄像头截图示例</h1>
    <CameraCapture />
  </div>
</template>

<script setup>
import CameraCapture from './components/CameraCapture.vue'
</script>

<style>
#app {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  text-align: center;
}

h1 {
  margin-bottom: 30px;
  color: #2c3e50;
}
</style>