<template>
    <div class="camera-container">
      <el-button type="primary" @click="openCamera" :disabled="isCameraOpen">
        打开摄像头
      </el-button>
      <el-button type="success" @click="takeSnapshot" :disabled="!isCameraOpen">
        截图
      </el-button>
      
      <div class="video-wrapper">
        <video ref="videoRef" autoplay></video>
        <canvas ref="canvasRef" style="display: none;"></canvas>
      </div>
  
      <el-image v-if="snapshotUrl" :src="snapshotUrl" fit="contain" />
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue'
  
  const videoRef = ref(null)
  const canvasRef = ref(null)
  const isCameraOpen = ref(false)
  const snapshotUrl = ref('')
  
  const openCamera = async () => {
    try {
      const stream = await navigator.mediaDevices.getUserMedia({ video: true })
      videoRef.value.srcObject = stream
      isCameraOpen.value = true
    } catch (error) {
      console.error('无法访问摄像头:', error)
      ElMessage.error('无法访问摄像头，请检查权限')
    }
  }
  
  const takeSnapshot = () => {
    const video = videoRef.value
    const canvas = canvasRef.value
    
    // 设置canvas尺寸与视频一致
    canvas.width = video.videoWidth
    canvas.height = video.videoHeight
    
    // 绘制截图
    const ctx = canvas.getContext('2d')
    ctx.drawImage(video, 0, 0, canvas.width, canvas.height)
    
    // 转换为base64
    snapshotUrl.value = canvas.toDataURL('image/png')
    
    // 如果需要Blob格式
    canvas.toBlob(blob => {
      console.log('Blob:', blob)
      // 可以在这里处理Blob对象，比如上传到服务器
    }, 'image/png')
  }
  </script>
  
  <style scoped>
  .camera-container {
    max-width: 800px;
    margin: 20px auto;
    text-align: center;
  }
  
  .video-wrapper {
    margin: 20px 0;
  }
  
  video {
    max-width: 100%;
    height: auto;
    border: 1px solid #ddd;
    border-radius: 4px;
  }
  </style>