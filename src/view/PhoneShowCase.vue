<template>
  <div class="container">
    <!-- 左侧导航 -->
    <div class="sidebar">
      <button
        v-for="(item, index) in items"
        :key="index"
        class="nav-btn"
        :class="{ active: currentIndex === index }"
        @click="playItem(index)"
      >
        <span v-if="currentIndex !== index">+</span>
        <span v-else style="font-size: 20px;">●</span>
        {{ item.title }}
      </button>
    </div>

    <!-- 右侧展示 -->
    <div class="content">
      <!-- iPhone 3D 模型 -->
      <IphoneModel 
        v-if="items[currentIndex].title === '机身'"
        class="iphone-model"
      />

      <!-- 视频 -->
      <video
        v-else-if="isVideo(items[currentIndex].src)"
        ref="videoPlayer"
        :key="items[currentIndex].src"
        :src="items[currentIndex].src"
        class="video-player"
        autoplay
        playsinline
        @ended="holdLastFrame"
      ></video>

      <!-- 图片 -->
      <img
        v-else
        :src="items[currentIndex].src"
        :alt="items[currentIndex].title"
        class="image-player"
      />
    </div>
  </div>
</template>

<script>
import IphoneModel from './IphoneModel.vue';

export default {
  name: "PhoneShowcase",
  components: {
    IphoneModel
  },
  data() {
    return {
      currentIndex: 0,
      items: [
        { title: "机身", src: "/assets/video1.mp4" },
        { title: "铝金属一体成型机身", src: "/assets/vedio/iphoneVedio008.mp4" },
        { title: "摄像头", src: "/assets/vedio/iphoneVedio004.mp4" },
        { title: "沉浸式 Pro 显示屏", src: "/assets/img/explan006.jpg" },
        { title: "相机控制", src: "/assets/vedio/iphoneVedio006.mp4" },
        { title: "操作按钮", src: "public/assets/vedio/iphoneVedio007.mp4" },
      ],
    };
  },
  methods: {
    playItem(index) {
      this.currentIndex = index;
      this.$nextTick(() => {
        const video = this.$refs.videoPlayer;
        if (video) {
          video.currentTime = 0;
          video.play();
        }
      });
    },
    holdLastFrame(event) {
      const video = event.target;
      video.pause();
      video.currentTime = video.duration; // 固定在最后一帧
    },
    isVideo(path) {
      return path.endsWith(".mp4") || path.endsWith(".webm");
    },
  },
};
</script>

<style scoped>

.container {
  display: flex;
  background: #000;
  border-radius: 20px;
  padding: 40px;
  max-width: 1200px;
  width: 90%;
  height: 80vh;
  margin: 0 auto;
  box-shadow: 0 0 30px rgba(0, 0, 0, 0.5);
  color: white;
  position: relative;
}

/* 左侧导航 */
.sidebar {
  display: flex;
  flex-direction: column;
  gap: 15px;
  flex: 1;
}

.nav-btn {
  background: #2a2a2a;
  border: none;
  border-radius: 20px;
  padding: 12px 20px;
  color: white;
  font-size: 16px;
  cursor: pointer;
  text-align: left;
  transition: all 0.3s;
  display: flex;
  align-items: center;
  gap: 10px;
}

.nav-btn:hover {
  background: #444;
}

.nav-btn.active {
  background: #e65c00;
  color: #fff;
}

/* 右侧展示 */
.content {
  flex: 3;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
}

.video-player{ width: 100%; 
    height: 100%; 
      margin-left: 40px;  /* 向右移动20px */
    object-fit: cover;/* 铺满裁切 */ border-radius: 10px; box-shadow: 0 8px 20px rgba(0, 0, 0, 0.5); }
.image-player {
  width: 100%;          /* 占满容器宽度 */
  height: 115%;         /* 占满容器高度 */
  object-fit: cover;    /* 铺满裁切，保证铺满效果 */
  border-radius: 10px;  /* 照片质感，带圆角 */
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.5); /* 阴影，让图片像照片 */
  background: #000;     /* 避免比例不足时出现空白 */
}

/* iPhone 3D 模型样式 */
.iphone-model {
  width: 100%;
  height: 100%;
  border-radius: 10px;
  overflow: hidden;
  top: 0%;              /* 垂直居中 */
  left: 3%;     
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.5);
}

.iphone-model #threepipe-canvas-container {
  width: 100% !important;
  height: 100% !important;
  border-radius: 10px;
  position: relative !important;
}

</style>