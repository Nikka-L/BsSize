<template>
  <div
    class="compare-container"
    @mousemove="onDrag"
    @mouseup="stopDrag"
    @mouseleave="stopDrag"
    @touchmove="onDragTouch"
    @touchend="stopDrag"
  >
    <!-- 底层照片 -->
    <img :src="image" alt="Photo" class="image" />

    <!-- 白雾层 -->
    <div class="fog-overlay" :style="{ left: position + '%' }"></div>

    <!-- 分界线 -->
    <div
      class="divider"
      :style="{ left: position + '%' }"
      @mousedown="startDrag"
      @touchstart="startDrag"
    >
      <div class="handle">
        <span class="arrow left">‹</span>
        <span class="arrow right">›</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ImageFogCompare",
  data() {
    return {
      position: 50, // 分界线位置（百分比）
      dragging: false,
      image: "/assets/img/explan001.jpg", // 照片
    };
  },
  methods: {
    startDrag() {
      this.dragging = true;
    },
    stopDrag() {
      this.dragging = false;
    },
    onDrag(e) {
      if (!this.dragging) return;
      const rect = e.currentTarget.getBoundingClientRect();
      const newPos = ((e.clientX - rect.left) / rect.width) * 100;
      this.position = Math.max(0, Math.min(100, newPos));
    },
    onDragTouch(e) {
      if (!this.dragging) return;
      const rect = e.currentTarget.getBoundingClientRect();
      const touch = e.touches[0];
      const newPos = ((touch.clientX - rect.left) / rect.width) * 100;
      this.position = Math.max(0, Math.min(100, newPos));
    },
  },
};
</script>

<style scoped>
.compare-container {
  position: relative;
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  overflow: hidden;
  user-select: none;
}

.image {
  width: 100%;
  height: auto;
  display: block;
}

/* 白雾层 */
.fog-overlay {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  background: rgba(212, 210, 210, 0.082);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  transition: left 0.1s ease-out; /* 让滑动更丝滑 */
}

/* 分界线 */
.divider {
  position: absolute;
  top: 0;
  bottom: 0;
  width: 2px;
  background: #fff;
  cursor: ew-resize;
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: left 0.1s ease-out; /* 丝滑过渡 */
}

/* 拖动手柄 */
.handle {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 30px;
  height: 30px;
  background: rgba(0, 0, 0, 0.6);
  border: 3px solid #ffffff;
  border-radius: 50%;
}

/* 箭头 */
.arrow {
  color: #fff;
  font-size: 20px;
  font-weight: bold;
  margin: 0 4px;
  user-select: none;
}

.arrow.left {
  transform: translateX(-2px);
}

.arrow.right {
  transform: translateX(2px);
}
</style>
