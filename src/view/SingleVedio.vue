<template>
  <div class="video-container" ref="videoContainer">
    <video
      ref="videoPlayer"
      class="video-player"
      autoplay
      playsinline
      @ended="holdLastFrame"
    >
      <source src="/assets/vedio/iphoneVedio003.mp4" type="video/mp4" />
    </video>
  </div>
</template>

<script>
export default {
  name: "VideoPlayer",
  data() {
    return {
      lastScrollTop: 0,
      isInView: false,
    };
  },
  mounted() {
    window.addEventListener("scroll", this.handleScroll);

    const observer = new IntersectionObserver(
      (entries) => {
        entries.forEach((entry) => {
          this.isInView = entry.isIntersecting;
        });
      },
      { threshold: 0.5 }
    );

    observer.observe(this.$refs.videoContainer);
    this.observer = observer;
  },
  beforeUnmount() {
    window.removeEventListener("scroll", this.handleScroll);
    if (this.observer) this.observer.disconnect();
  },
  methods: {
    holdLastFrame() {
      const video = this.$refs.videoPlayer;
      if (video) {
        video.pause();
        video.currentTime = video.duration;
      }
    },
    handleScroll() {
      const currentScroll = window.scrollY;
      if (this.isInView && this.lastScrollTop - currentScroll > 150) {
        this.restartVideo();
      }
      this.lastScrollTop = currentScroll;
    },
    restartVideo() {
      const video = this.$refs.videoPlayer;
      if (video) {
        video.currentTime = 0;
        video.play();
      }
    },
  },
};
</script>

<style scoped>
.video-container {
  margin: 0;
  background: #000;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh; /* 刚好全屏 */
  width: 100vw;
}

.video-player {
  width: 100%;
  height: 100%;
  object-fit: contain; /* 完整显示视频，不裁切 */
  background: #000;    /* 避免比例不足时显示空白 */
}
</style>
