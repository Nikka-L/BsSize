<template>
  <div class="phone-frame">
    <!-- 手机外框 -->
    <img src="/assets/img/iphone.png" alt="iPhone Frame" class="phone-image" />

    <!-- 屏幕区域 -->
    <div class="phone-screen">
      <!-- 当前照片 -->
      <img :src="currentPhoto" alt="Screen Photo" class="screen-photo" />

      <!-- 参数按钮 -->
      <div class="params">
        <button
          v-for="(item, index) in photos"
          :key="index"
          :class="{ active: currentIndex === index }"
          @click="switchPhoto(index)"
        >
          {{ item.label }}
        </button>
      </div>
    </div>

    <!-- 灵动岛+摄像头覆盖层 (永远在顶层) -->
    <img src="/assets/img/iphoneSXT.png" alt="Dynamic Island" class="dynamic-island" />
  </div>
</template>

<script>
export default {
  name: "IphoneFrameHorizontal",
  data() {
    return {
      currentIndex: 0,
      photos: [
        { label: "8x", src: "/assets/SJJH/SJJH001.jpg" },
        { label: "4x", src: "/assets/SJJH/SJJH002.jpg" },
        { label: "2x", src: "/assets/SJJH/SJJH003.jpg" },
        { label: "0.5x", src: "/assets/SJJH/SJJH007.jpg" },
      ],
    };
  },
  computed: {
    currentPhoto() {
      return this.photos[this.currentIndex].src;
    },
  },
  methods: {
    switchPhoto(index) {
      this.currentIndex = index;
    },
  },
};
</script>

<style scoped>
.phone-frame {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100vh;
  overflow: hidden;
}

/* 外框 */
.phone-image {
  height: 120%;
  transform: rotate(270deg);
  object-fit: contain;
  position: relative;  /* ✅ 加这个 */
  z-index: 1;          /* ✅ 外框层级较低 */
}

/* 屏幕区域 */
.phone-screen {
  position: absolute;
  width: 70.3%;
  height: 55.6%;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
  border-radius: 71px;
  background: black;
  z-index: 2; /* 确保屏幕在手机外框之上 */
}

/* 照片内容 */
.screen-photo {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

/* 参数按钮 */
.params {
  position: absolute;
  bottom: 20px;
  display: flex;
  gap: 14px;
  z-index: 50; /* 按钮在屏幕里，且低于灵动岛 */
}

.params button {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background: rgba(0, 0, 0, 0.6);
  border: 2px solid white;
  color: white;
  font-size: 14px;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.25s ease;
  transform: scale(1);
}

.params button:active {
  transform: scale(0.75); /* 点击时缩小 */
}

.params button.active {
  background: white;
  color: black;
  transform: scale(1.25); /* 选中时轻微放大 */
}

/* 灵动岛 PNG 覆盖层 (顶层) */
.dynamic-island {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-290%, -50%)  rotate(270deg);
  width: 150px;
  height: auto;
  z-index: 99; /* 永远在顶层 */
  pointer-events: none;
}
</style>
