<template>
  <div class="book-page-section">
    <div class="carousel-container">
      <div ref="carousel" class="carousel-wrapper">

<div 
  v-for="(item, index) in carouselItems" 
  :key="index"
  :class="getItemClass(index)"
  class="carousel-item"
  :data-index="index"
>
  <img 
    v-if="item.type === 'image'"
    :src="item.src" 
    :alt="item.alt" 
    class="carousel-image"
  >
  <video
    v-else-if="item.type === 'video'"
    ref="videoRefs"
    :data-index="index"
    :src="item.src"
    class="carousel-image"
    muted
    playsinline
    loop
  ></video>
</div>


      </div>
      
      <div 
        class="carousel-nav carousel-nav-left" 
        @click="prevItem"
        aria-label="上一项"
      ></div>
      <div 
        class="carousel-nav carousel-nav-right" 
        @click="nextItem"
        aria-label="下一项"
      ></div>
    </div>
    
    <div class="carousel-controls">
      <div class="carousel-dots">
        <div 
          v-for="(item, index) in carouselItems"
          :key="`dot-${index}`"
          :class="['nav-dot', { active: index === currentIndex }]"
          :aria-label="`切换到第${index + 1}项`"
          :aria-current="index === currentIndex ? 'true' : 'false'"
          @click="goToIndex(index)"
        ></div>
      </div>
      
    </div>
  </div>
</template>

<script>
export default {
  name: 'BookPage',
  data() {
    return {
      currentIndex: 0,
      isPlaying: true,
      autoplayInterval: null,
      idleTimer: null,
      touchStartX: 0,
      currentX: 0,
      isDragging: false,
      touchStartTime: 0,
      AUTO_PLAY_DELAY: 5000,
      IDLE_DELAY: 5000,
      carouselItems: [
        {
           type: 'video',
          src: '/assets/vedio/iphoneVedio001.mp4',
          alt: 'iOS 26 功能展示1'
        },
        {
           type: 'video',
          src: '/assets/vedio/iphoneVedio002.mp4',
          alt: 'iOS 26 功能展示2'
        },
       {
           type: 'video',
          src: '/assets/vedio/iphoneVedio003.mp4',
          alt: 'iOS 26 功能展示3'
        },
        {
           type: 'video',
          src: '/assets/vedio/iphoneVedio004.mp4',
          alt: 'iOS 26 功能展示4'
        }
      ],
      videoRefs: []
    }
  },
  computed: {
    totalItems() {
      return this.carouselItems.length;
    }
  },
  mounted() {
    this.updateCarousel();
    this.startAutoplay();
    this.setupKeyboardListeners();
    this.setupTouchListeners();
    this.setupMouseListeners();
  },
  beforeUnmount() {
    this.stopAutoplay();
    if (this.idleTimer) {
      clearTimeout(this.idleTimer);
    }
    this.removeEventListeners();
  },
  methods: {
    getItemClass(index) {
      const diff = (index - this.currentIndex + this.totalItems) % this.totalItems;
      
      if (diff === 0) {
        return 'carousel-item-center';
      } else if (diff === 1) {
        return 'carousel-item-right';
      } else if (diff === this.totalItems - 1) {
        return 'carousel-item-left';
      } else {
        return 'carousel-item-hidden';
      }
    },
    
    updateCarousel() {
  this.$nextTick(() => {
    const videos = this.$refs.videoRefs || [];
    videos.forEach((video) => {
      const index = parseInt(video.dataset.index);
      if (index === this.currentIndex) {
        video.play().catch(() => {}); // 防止自动播放报错
      } else {
        video.pause();
        video.currentTime = 0; // 回到开头
      }
    });
  });
},

    
    goToIndex(index) {
      this.currentIndex = (index + this.totalItems) % this.totalItems;
      this.updateCarousel();
      this.resetIdleTimer();
    },
    
    nextItem() {
      this.goToIndex(this.currentIndex + 1);
    },
    
    prevItem() {
      this.goToIndex(this.currentIndex - 1);
    },
    
    startAutoplay() {
      if (this.autoplayInterval) clearInterval(this.autoplayInterval);
      this.autoplayInterval = setInterval(this.nextItem, this.AUTO_PLAY_DELAY);
      this.isPlaying = true;
    },
    
    stopAutoplay() {
      clearInterval(this.autoplayInterval);
      this.autoplayInterval = null;
      this.isPlaying = false;
    },
    
    toggleAutoplay() {
      if (this.isPlaying) {
        this.stopAutoplay();
      } else {
        this.startAutoplay();
      }
    },
    
    resetIdleTimer() {
      if (this.idleTimer) clearTimeout(this.idleTimer);
      this.stopAutoplay();
      
      this.idleTimer = setTimeout(() => {
        if (!this.isPlaying) this.startAutoplay();
      }, this.IDLE_DELAY);
    },
    
    setupKeyboardListeners() {
      this.keydownHandler = (e) => {
        if (e.key === 'ArrowRight') {
          this.nextItem();
          this.resetIdleTimer();
        }
        if (e.key === 'ArrowLeft') {
          this.prevItem();
          this.resetIdleTimer();
        }
      };
      document.addEventListener('keydown', this.keydownHandler);
    },
    
    setupTouchListeners() {
      this.touchStartHandler = (e) => {
        this.touchStartX = e.touches[0].clientX;
        this.currentX = this.touchStartX;
        this.isDragging = true;
        this.touchStartTime = Date.now();
        this.resetIdleTimer();
      };
      
      this.touchMoveHandler = (e) => {
        if (!this.isDragging) return;
        this.currentX = e.touches[0].clientX;
        const deltaX = this.currentX - this.touchStartX;

        const prevIndex = (this.currentIndex - 1 + this.totalItems) % this.totalItems;
        const nextIndex = (this.currentIndex + 1) % this.totalItems;

        // 直接操作DOM来处理触摸时的实时变换
        const carousel = this.$refs.carousel;
        const items = carousel.querySelectorAll('.carousel-item');

        items[this.currentIndex].style.transition = "none";
        items[prevIndex].style.transition = "none";
        items[nextIndex].style.transition = "none";

        items[this.currentIndex].style.transform = `translateX(${deltaX * 0.5}px) scale(1)`;
        items[prevIndex].style.transform = `scale(0.7) translateX(${deltaX * 0.2 - 80}%)`;
        items[nextIndex].style.transform = `scale(0.7) translateX(${deltaX * 0.2 + 80}%)`;
      };
      
      this.touchEndHandler = (e) => {
        if (!this.isDragging) return;
        
        const deltaX = e.changedTouches[0].clientX - this.touchStartX;
        const touchDuration = Date.now() - this.touchStartTime;
        const isSwipe = Math.abs(deltaX) > 50 || (Math.abs(deltaX) > 20 && touchDuration < 200);
        
        if (isSwipe) {
          if (deltaX > 0) this.prevItem();
          else this.nextItem();
        } else {
          this.updateCarousel();
        }
        
        this.isDragging = false;
      };
      
      const carousel = this.$refs.carousel;
      carousel.addEventListener('touchstart', this.touchStartHandler);
      carousel.addEventListener('touchmove', this.touchMoveHandler);
      carousel.addEventListener('touchend', this.touchEndHandler);
    },
    
    setupMouseListeners() {
      this.mouseEnterHandler = () => {
        if (this.isPlaying) this.stopAutoplay();
      };
      
      this.mouseLeaveHandler = () => {
        if (!this.isPlaying) this.resetIdleTimer();
      };
      
      const carousel = this.$refs.carousel;
      carousel.addEventListener('mouseenter', this.mouseEnterHandler);
      carousel.addEventListener('mouseleave', this.mouseLeaveHandler);
    },
    
    removeEventListeners() {
      if (this.keydownHandler) {
        document.removeEventListener('keydown', this.keydownHandler);
      }
      if (this.touchStartHandler) {
        const carousel = this.$refs.carousel;
        carousel.removeEventListener('touchstart', this.touchStartHandler);
        carousel.removeEventListener('touchmove', this.touchMoveHandler);
        carousel.removeEventListener('touchend', this.touchEndHandler);
      }
      if (this.mouseEnterHandler) {
        const carousel = this.$refs.carousel;
        carousel.removeEventListener('mouseenter', this.mouseEnterHandler);
        carousel.removeEventListener('mouseleave', this.mouseLeaveHandler);
      }
    }
  }
}
</script>

<style scoped>
.carousel-image {
  width: 100%;
  height: 100%;
  object-fit: contain;
  max-width: 750px;   /* 限制最大宽度 */
  max-height: 500px;  /* 限制最大高度 */
  margin: 0 auto;      /* 居中显示 */
  border-radius: 25px;
  border: 3px solid #414566;
}

.book-page-section {
  padding: 0px 20px;
  background: var(--bg);
  color: var(--fg);
  font-family: Inter, ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", Arial, "Noto Sans", sans-serif;
}

.book-page-section .container {
  width: 100%;
  max-width: var(--container);
  margin: 0 auto;
  padding: 0 20px;
  text-align: center;
  margin-bottom: 40px;
}

.book-page-section .section-title {
  font-size: 28px;
  margin: 0 0 18px;
  color: var(--fg);
}

.book-page-section .section-subtitle {
  color: var(--muted);
  max-width: 820px;
  margin: 0 auto;
  font-size: 16px;
}

.carousel-container {
  position: relative;
  width: 100%;
  max-width: 1200px;
  height: 500px;
  overflow: hidden;
  margin: 0 auto;
}

.carousel-wrapper {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
}

.carousel-image {
  width: 100%;
  height: 100%;
  object-fit: contain;
}

.carousel-nav {
  position: absolute;
  top: 0;
  bottom: 0;
  width: 33.333%;
  z-index: 40;
  cursor: pointer;
}

.carousel-nav-left {
  left: 0;
}

.carousel-nav-right {
  right: 0;
}

.carousel-controls {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 16px;
  margin-top: 32px;
  z-index: 40;
}

.carousel-dots {
  display: flex;
  gap: 4px;
  margin-right: 16px;
}

.carousel-play-btn {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  border: 1px solid rgba(255, 255, 255, 0.3);
  display: flex;
  align-items: center;
  justify-content: center;
  background: transparent;
  color: var(--fg);
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.carousel-play-btn:hover {
  background: rgba(255, 255, 255, 0.1);
}

.book-page-section * {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

@media (min-width: 768px) {
  .carousel-container {
    height: 600px;
  }
}

.carousel-item {
  position: absolute;
  transition: transform 0.5s cubic-bezier(0.25, 0.46, 0.45, 0.94), 
              opacity 0.5s ease;
  will-change: transform, opacity;
}

.carousel-item-center {
  transform: scale(1) translateX(0);
  opacity: 1;
  z-index: 30;
}

.carousel-item-left {
  transform: scale(0.7) translateX(-80%);
  opacity: 0.6;
  z-index: 20;
}

.carousel-item-right {
  transform: scale(0.7) translateX(80%);
  opacity: 0.6;
  z-index: 20;
}

.carousel-item-hidden {
  transform: scale(0.5);
  opacity: 0;
  z-index: 10;
  pointer-events: none;
}

.nav-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background-color: rgba(255, 255, 255, 0.3);
  transition: all 0.3s ease;
  cursor: pointer;
}

.nav-dot.active {
  background-color: rgba(255, 255, 255, 0.8);
  width: 24px;
  border-radius: 4px;
}

@media (max-width: 768px) {
  .carousel-item-left,
  .carousel-item-right {
    transform: scale(0.6) translateX(0);
    opacity: 0.4;
  }
  .carousel-item-left { left: -30%; }
  .carousel-item-right { right: -30%; }
}
</style>
