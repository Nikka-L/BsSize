<template>
  <div class="video-container">
    <video
      ref="videoPlayer"
      class="video-player"
      autoplay
      muted
      playsinline
    >
      <source src="/assets/vedio/iphoneWebOpen.mp4" type="video/mp4" />
      您的浏览器不支持 video 标签。
    </video>
  </div>
</template>

<script>
export default {
  name: "AutoPlayVideo",
  mounted() {
    const video = this.$refs.videoPlayer;
    // 播放结束后定格最后一帧
    video.addEventListener("ended", () => {
      video.pause();
      video.currentTime = video.duration - 0.05;
    });
  },
};
</script>

<style scoped>
.video-container {
  margin: 0;
  background: black;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh; /* 全屏 */
  max-height: 480px;
  width: 100%;
}

.video-player {
  width: 100%;
  height: auto;
  object-fit: cover; /* 改成 contain 可完整显示 */
}
</style>
