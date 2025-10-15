<template>
  <div class="bg-apple-dark text-white overflow-x-hidden">
    <!-- 导航栏 -->
    <header 
      class="fixed top-0 left-0 right-0 z-50 transition-all duration-500"
      ref="navbarRef"
      id="navbar"
    >
      <nav class="py-4 px-6 flex items-center justify-between backdrop-blur-xl bg-apple-dark/70">
        <div class="flex items-center space-x-6">
          <button 
            class="md:hidden opacity-80 hover:opacity-100 hover:text-apple-blue transition-all"
            @click="handleMenuToggle"
            id="menu-toggle"
          >
            <i class="fa fa-bars"></i>
          </button>
        </div>
      </nav>
      
      <!-- 移动端菜单 -->
      <div 
        class="md:hidden fixed inset-0 bg-apple-dark z-40 transform translate-x-full transition-transform duration-500 ease-in-out"
        ref="mobileMenuRef"
        id="mobile-menu"
      >
        <div class="p-6 flex justify-end">
          <button 
            class="text-2xl" 
            @click="handleMenuClose"
            id="menu-close"
          >
            <i class="fa fa-times"></i>
          </button>
        </div>
      </div>
    </header>

    <main>
      <!-- 英雄区域 -->
      <section class="relative min-h-screen flex items-center justify-center overflow-hidden pt-20">
        <!-- 背景光效 -->
        <div class="absolute top-1/4 left-1/4 w-[600px] h-[600px] bg-apple-blue/20 rounded-full filter blur-[120px]"></div>
        <div class="absolute bottom-1/4 right-1/4 w-[500px] h-[500px] bg-purple-500/10 rounded-full filter blur-[100px]"></div>
        
        <div class="container mx-auto px-6 z-10 text-center">
          <h1 class="text-4xl md:text-6xl lg:text-7xl font-semibold mb-6 tracking-tighter reveal">
            iPhone 16 Pro
          </h1>
          <h2 class="text-xl md:text-3xl text-apple-silver mb-16 max-w-2xl mx-auto reveal">
            Siri，以更智能的方式，回应你的每一个需求
          </h2>
          
          <!-- 手机模型（视差效果） -->
          <div class="relative max-w-md mx-auto perspective reveal">
            <div 
              class="relative w-full aspect-[9/19] rounded-[48px] overflow-hidden border-8 border-apple-dark shadow-[0_0_60px_rgba(0,113,227,0.15)] animate-float preserve-3d rotate-y"
              ref="phoneModelRef"
              id="phone-model"
            >
              <!-- 手机屏幕内容 -->
              <img 
                src="https://picsum.photos/id/1/800/1600" 
                alt="iPhone 16 Pro展示Siri界面" 
                class="w-full h-full object-cover"
              >
              
              <!-- Siri交互界面覆盖层 -->
              <div class="absolute inset-0 bg-gradient-to-t from-black/70 to-transparent flex flex-col justify-end p-6">
                <!-- 对话气泡 -->
                <div class="bg-white/10 backdrop-blur-xl rounded-2xl p-4 mb-4 text-left max-w-[80%] self-start">
                  <p class="text-sm mb-2">我可以帮你做什么？</p>
                  <div class="flex items-center space-x-3 mt-3">
                    <div class="siri-wave">
                      <span></span>
                      <span></span>
                      <span></span>
                    </div>
                    <p class="text-xs text-apple-silver">正在聆听...</p>
                  </div>
                </div>
                
                <!-- 语音按钮（粒子效果） -->
                <div class="voice-particles" ref="particlesContainerRef"></div>
                <div 
                  class="bg-apple-blue/90 backdrop-blur-xl rounded-full w-16 h-16 mx-auto mb-8 flex items-center justify-center animate-glow cursor-pointer"
                  @click="createParticles"
                  id="voice-button"
                >
                  <i class="fa fa-microphone text-white text-xl"></i>
                </div>
              </div>
            </div>
          </div>
        </div>
        
        <!-- 向下滚动指示器 -->
        <div class="absolute bottom-10 left-1/2 transform -translate-x-1/2 animate-bounce">
          <a href="#features" class="text-apple-silver hover:text-white transition-colors">
            <i class="fa fa-chevron-down"></i>
          </a>
        </div>
      </section>

      <!-- 功能亮点 -->
      <section id="features" class="py-24 relative overflow-hidden">
        <!-- 背景装饰 -->
        <div class="absolute top-0 right-0 w-1/3 h-1/3 bg-apple-blue/5 rounded-full filter blur-[100px]"></div>
        
        <div class="container mx-auto px-6">
          <h2 class="text-3xl md:text-5xl font-semibold text-center mb-20 reveal">
            重新定义智能助理体验
          </h2>
          
          <!-- 亮点1：瞬间响应 -->
          <div class="grid md:grid-cols-2 gap-20 items-center mb-32">
            <div class="order-2 md:order-1">
              <h3 class="text-2xl md:text-3xl font-semibold mb-6 reveal">瞬间响应，自然对话</h3>
              <p class="text-apple-silver text-lg mb-8 leading-relaxed reveal">
                A18 Pro芯片的神经网络引擎让Siri响应速度提升30%，对话如真人般自然流畅，支持更长上下文理解。
              </p>
              <div class="flex items-center space-x-2 text-apple-blue reveal cursor-pointer">
                <span>了解更多</span>
                <i class="fa fa-arrow-right text-sm"></i>
              </div>
            </div>
            <div class="order-1 md:order-2 perspective reveal">
              <div class="relative w-full aspect-square max-w-md mx-auto rounded-[36px] overflow-hidden border-4 border-apple-dark shadow-xl transform hover:rotate-y-12 transition-transform duration-700">
                <img src="https://picsum.photos/id/20/600/600" alt="Siri快速响应演示" class="w-full h-full object-cover">
                <div class="absolute inset-0 bg-gradient-to-t from-black/40 to-transparent"></div>
              </div>
            </div>
          </div>
          
          <!-- 亮点2：隐私保护 -->
          <div class="grid md:grid-cols-2 gap-20 items-center mb-32">
            <div class="perspective reveal">
              <div class="relative w-full aspect-square max-w-md mx-auto rounded-[36px] overflow-hidden border-4 border-apple-dark shadow-xl transform hover:-rotate-y-12 transition-transform duration-700">
                <img src="https://picsum.photos/id/96/600/600" alt="Siri隐私保护" class="w-full h-full object-cover">
                <div class="absolute inset-0 bg-gradient-to-t from-black/40 to-transparent"></div>
              </div>
            </div>
            <div>
              <h3 class="text-2xl md:text-3xl font-semibold mb-6 reveal">本地处理，隐私至上</h3>
              <p class="text-apple-silver text-lg mb-8 leading-relaxed reveal">
                所有指令在设备端处理，无需上传云端。全新隐私控制面板，让你精确管理Siri可访问的数据。
              </p>
              <div class="flex items-center space-x-2 text-apple-blue reveal cursor-pointer">
                <span>隐私详情</span>
                <i class="fa fa-arrow-right text-sm"></i>
              </div>
            </div>
          </div>
          
          <!-- 亮点3：深度集成 -->
          <div class="grid md:grid-cols-2 gap-20 items-center">
            <div class="order-2 md:order-1">
              <h3 class="text-2xl md:text-3xl font-semibold mb-6 reveal">深度集成，无缝体验</h3>
              <p class="text-apple-silver text-lg mb-8 leading-relaxed reveal">
                与系统功能深度融合，从相机控制到健康数据分析，一声指令即可完成复杂操作。
              </p>
              <div class="flex items-center space-x-2 text-apple-blue reveal cursor-pointer">
                <span>功能集成</span>
                <i class="fa fa-arrow-right text-sm"></i>
              </div>
            </div>
            <div class="order-1 md:order-2 perspective reveal">
              <div class="relative w-full aspect-square max-w-md mx-auto rounded-[36px] overflow-hidden border-4 border-apple-dark shadow-xl transform hover:rotate-y-12 transition-transform duration-700">
                <img src="https://picsum.photos/id/3/600/600" alt="Siri系统集成" class="w-full h-full object-cover">
                <div class="absolute inset-0 bg-gradient-to-t from-black/40 to-transparent"></div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- 交互演示（Siri对话） -->
      <section class="py-24 bg-gradient-to-b from-apple-dark to-apple-dark/95 relative overflow-hidden">
        <div class="absolute -bottom-40 -left-40 w-[600px] h-[600px] bg-apple-blue/10 rounded-full filter blur-[120px]"></div>
        
        <div class="container mx-auto px-6 relative z-10">
          <h2 class="text-3xl md:text-5xl font-semibold text-center mb-6 reveal">亲身体验</h2>
          <p class="text-apple-silver text-lg text-center max-w-2xl mx-auto mb-16 reveal">
            与Siri对话，感受前所未有的智能交互体验
          </p>
          
          <div class="max-w-2xl mx-auto bg-apple-glass backdrop-blur-xl rounded-3xl p-6 md:p-8 border border-white/10 shadow-xl reveal">
            <!-- 对话界面 -->
            <div 
              class="space-y-4 mb-6 max-h-[400px] overflow-y-auto pr-2"
              ref="chatContainerRef"
              id="chatContainer"
            >
              <!-- 初始对话 -->
              <div class="flex items-start space-x-3">
                <div class="w-8 h-8 rounded-full bg-apple-blue flex items-center justify-center flex-shrink-0">
                  <i class="fa fa-microphone text-white text-xs"></i>
                </div>
                <div class="bg-white/5 rounded-2xl px-4 py-3 max-w-[80%]">
                  <p class="text-sm">你好！我是Siri，有什么可以帮你的吗？</p>
                </div>
              </div>
              <div class="flex items-start justify-end space-x-3">
                <div class="bg-apple-blue/20 rounded-2xl px-4 py-3 max-w-[80%]">
                  <p class="text-sm">今天天气怎么样？</p>
                </div>
              </div>
              <div class="flex items-start space-x-3">
                <div class="w-8 h-8 rounded-full bg-apple-blue flex items-center justify-center flex-shrink-0">
                  <i class="fa fa-microphone text-white text-xs"></i>
                </div>
                <div class="bg-white/5 rounded-2xl px-4 py-3 max-w-[80%]">
                  <p class="text-sm">今天天气晴朗，气温25°C，适合户外活动。需要为你规划出行路线吗？</p>
                </div>
              </div>
            </div>
            
            <!-- 输入区域 -->
            <div class="relative">
              <input 
                type="text" 
                placeholder="试着问Siri一些问题..." 
                class="w-full bg-white/5 border border-white/10 rounded-full px-5 py-4 pr-14 focus:outline-none focus:border-apple-blue transition-colors"
                v-model="inputValue"
                @keypress="handleInputKeypress"
                id="inputField"
              >
              <button 
                class="absolute right-3 top-1/2 transform -translate-y-1/2 w-10 h-10 rounded-full bg-apple-blue flex items-center justify-center hover:bg-apple-blue/90 transition-colors"
                @click="handleSendMessage"
                id="sendButton"
              >
                <i class="fa fa-paper-plane text-white text-xs"></i>
              </button>
            </div>
          </div>
        </div>
      </section>

      <!-- 行动召唤 -->
      <section class="py-24 relative overflow-hidden">
        <div class="container mx-auto px-6 text-center">
          <h2 class="text-3xl md:text-5xl font-semibold mb-8 reveal">
            与更智能的Siri相遇
          </h2>
          <p class="text-apple-silver text-xl max-w-2xl mx-auto mb-12 reveal">
            仅在iPhone 16 Pro，体验下一代智能助理
          </p>
          
          <a 
            href="#" 
            class="inline-block bg-apple-blue hover:bg-apple-blue/90 text-white py-4 px-12 rounded-full text-lg font-medium transition-all hover:shadow-lg hover:shadow-apple-blue/20 hover:scale-105 reveal"
          >
            购买 iPhone 16 Pro
          </a>
        </div>
      </section>
    </main>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, nextTick } from 'vue'

// 元素引用（同时保留ID选择器兼容性）
const navbarRef = ref(null)
const mobileMenuRef = ref(null)
const phoneModelRef = ref(null)
const particlesContainerRef = ref(null)
const chatContainerRef = ref(null)
const inputValue = ref('')

// 在组件挂载后加载外部资源
onMounted(() => {
  // 加载Tailwind CSS
  const tailwindScript = document.createElement('script')
  tailwindScript.src = 'https://cdn.tailwindcss.com'
  tailwindScript.onload = configureTailwind
  document.head.appendChild(tailwindScript)
  
  // 加载Font Awesome
  const fontAwesomeLink = document.createElement('link')
  fontAwesomeLink.rel = 'stylesheet'
  fontAwesomeLink.href = 'https://cdn.jsdelivr.net/npm/font-awesome@4.7.0/css/font-awesome.min.css'
  document.head.appendChild(fontAwesomeLink)
  
  nextTick(() => {
    // 绑定事件监听
    window.addEventListener('scroll', handleNavbarScroll)
    window.addEventListener('scroll', checkReveal)
    window.addEventListener('mousemove', handleParallax)
    window.addEventListener('load', checkReveal)
    
    // 初始化检测动画元素
    setTimeout(checkReveal, 100)
  })
})

// 配置Tailwind
function configureTailwind() {
  window.tailwind.config = {
    theme: {
      extend: {
        colors: {
          apple: {
            blue: '#0071e3',
            dark: '#111111',
            light: '#f5f5f7',
            gray: '#86868b',
            silver: '#e2e2e7',
            glass: 'rgba(255, 255, 255, 0.05)'
          }
        },
        fontFamily: {
          sans: ['SF Pro Display', '-apple-system', 'BlinkMacSystemFont', 'Segoe UI', 'Roboto', 'Helvetica Neue', 'Arial', 'sans-serif']
        },
        animation: {
          'float': 'float 6s ease-in-out infinite',
          'pulse-slow': 'pulse 4s cubic-bezier(0.4, 0, 0.6, 1) infinite',
          'glow': 'glow 2s ease-in-out infinite alternate'
        },
        keyframes: {
          float: {
            '0%, 100%': { transform: 'translateY(0)' },
            '50%': { transform: 'translateY(-20px)' }
          },
          glow: {
            '0%': { boxShadow: '0 0 15px rgba(0, 113, 227, 0.5)' },
            '100%': { boxShadow: '0 0 30px rgba(0, 113, 227, 0.8)' }
          }
        }
      }
    }
  }
}

onUnmounted(() => {
  // 移除事件监听
  window.removeEventListener('scroll', handleNavbarScroll)
  window.removeEventListener('scroll', checkReveal)
  window.removeEventListener('mousemove', handleParallax)
  window.removeEventListener('load', checkReveal)
})

// 导航栏滚动效果
const handleNavbarScroll = () => {
  const navbar = navbarRef.value || document.getElementById('navbar')
  if (navbar) {
    if (window.scrollY > 50) {
      navbar.classList.add('bg-apple-dark/90', 'shadow-lg')
    } else {
      navbar.classList.remove('bg-apple-dark/90', 'shadow-lg')
    }
  }
}

// 移动端菜单控制
const handleMenuToggle = () => {
  const mobileMenu = mobileMenuRef.value || document.getElementById('mobile-menu')
  if (mobileMenu) {
    mobileMenu.classList.remove('translate-x-full')
    document.body.style.overflow = 'hidden'
  }
}

const handleMenuClose = () => {
  const mobileMenu = mobileMenuRef.value || document.getElementById('mobile-menu')
  if (mobileMenu) {
    mobileMenu.classList.add('translate-x-full')
    document.body.style.overflow = ''
  }
}

// 滚动显示动画
const checkReveal = () => {
  const windowHeight = window.innerHeight
  const revealPoint = 150
  const revealElements = document.querySelectorAll('.reveal')
  
  revealElements.forEach(element => {
    const revealTop = element.getBoundingClientRect().top
    if (revealTop < windowHeight - revealPoint) {
      element.classList.add('active')
    }
  })
}

// 手机模型视差效果
const handleParallax = (e) => {
  const phoneModel = phoneModelRef.value || document.getElementById('phone-model')
  if (phoneModel) {
    const x = (window.innerWidth - e.pageX * 2) / 90
    const y = (window.innerHeight - e.pageY * 2) / 90
    phoneModel.style.transform = `rotateY(${x}deg) rotateX(${y}deg)`
  }
}

// 语音按钮粒子效果
const createParticles = () => {
  const particlesContainer = particlesContainerRef.value || document.querySelector('.voice-particles')
  if (!particlesContainer) return
  
  // 清除现有粒子
  while (particlesContainer.firstChild) {
    particlesContainer.removeChild(particlesContainer.firstChild)
  }
  
  // 创建新粒子
  for (let i = 0; i < 20; i++) {
    const particle = document.createElement('div')
    particle.classList.add('particle')
    
    // 随机大小
    const size = Math.random() * 8 + 2
    particle.style.width = `${size}px`
    particle.style.height = `${size}px`
    
    // 从按钮中心出发
    const voiceButton = document.getElementById('voice-button')
    const buttonRect = voiceButton.getBoundingClientRect()
    const containerRect = particlesContainer.getBoundingClientRect()
    
    particle.style.left = `${buttonRect.left + buttonRect.width/2 - containerRect.left}px`
    particle.style.top = `${buttonRect.top + buttonRect.height/2 - containerRect.top}px`
    
    // 随机方向和距离
    const angle = Math.random() * Math.PI * 2
    const distance = Math.random() * 100 + 50
    const destX = Math.cos(angle) * distance
    const destY = Math.sin(angle) * distance
    
    particlesContainer.appendChild(particle)
    
    // 动画
    setTimeout(() => {
      particle.style.transform = `translate(${destX}px, ${destY}px)`
      particle.style.opacity = '1'
      particle.style.transition = 'all 1s ease-out'
      
      // 淡出
      setTimeout(() => {
        particle.style.opacity = '0'
        particle.style.transition = 'opacity 0.5s ease-out'
        
        // 移除粒子
        setTimeout(() => {
          if (particlesContainer.contains(particle)) {
            particlesContainer.removeChild(particle)
          }
        }, 500)
      }, 800)
    }, 10)
  }
}

// 对话交互
const handleSendMessage = () => {
  const text = inputValue.value.trim()
  const chatContainer = chatContainerRef.value || document.getElementById('chatContainer')
  if (!text || !chatContainer) return
  
  // 添加用户消息
  addMessage(text, true)
  inputValue.value = ''
  
  // 模拟Siri回复
  setTimeout(() => {
    const replies = [
      "这是个好问题！让我为你查找相关信息。",
      "我明白了，我会帮你处理这个请求。",
      "好的，已经完成了。还有其他需要帮助的吗？",
      "根据我的了解，答案是这样的...",
      "我不太确定，但我可以帮你搜索更多信息。"
    ]
    
    const randomReply = replies[Math.floor(Math.random() * replies.length)]
    addMessage(randomReply)
    
    // 添加Siri正在输入的动画效果
    const waveDiv = document.createElement('div')
    waveDiv.className = 'flex items-center space-x-3 mt-2 ml-10'
    waveDiv.innerHTML = `
      <div class="siri-wave" style="width: 40px; height: 40px;">
        <span></span>
        <span></span>
        <span></span>
      </div>
    `
    
    chatContainer.appendChild(waveDiv)
    chatContainer.scrollTop = chatContainer.scrollHeight
    
    // 移除正在输入动画
    setTimeout(() => {
      if (chatContainer.contains(waveDiv)) {
        chatContainer.removeChild(waveDiv)
      }
    }, 1000)
  }, 800)
}

// 添加消息到对话界面
const addMessage = (text, isUser = false) => {
  const chatContainer = chatContainerRef.value || document.getElementById('chatContainer')
  if (!chatContainer) return
  
  const messageDiv = document.createElement('div')
  messageDiv.className = isUser 
    ? 'flex items-start justify-end space-x-3' 
    : 'flex items-start space-x-3'
  
  let messageContent = ''
  
  if (!isUser) {
    messageContent = `
      <div class="w-8 h-8 rounded-full bg-apple-blue flex items-center justify-center flex-shrink-0">
        <i class="fa fa-microphone text-white text-xs"></i>
      </div>
      <div class="bg-white/5 rounded-2xl px-4 py-3 max-w-[80%]">
        <p class="text-sm">${text}</p>
      </div>
    `
  } else {
    messageContent = `
      <div class="bg-apple-blue/20 rounded-2xl px-4 py-3 max-w-[80%]">
        <p class="text-sm">${text}</p>
      </div>
    `
  }
  
  messageDiv.innerHTML = messageContent
  chatContainer.appendChild(messageDiv)
  chatContainer.scrollTop = chatContainer.scrollHeight
  return messageDiv
}

// 输入框回车事件
const handleInputKeypress = (e) => {
  if (e.key === 'Enter') {
    handleSendMessage()
  }
}
</script>

<style scoped>
/* 保留所有原CSS样式，并添加scoped兼容处理 */
@layer utilities {
  .content-auto {
    content-visibility: auto;
  }
  .text-shadow {
    text-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  }
  .backdrop-blur-xl {
    backdrop-filter: blur(24px);
  }
  .mask-radial {
    mask-image: radial-gradient(circle, black 50%, transparent 100%);
  }
  .perspective {
    perspective: 1000px;
  }
  .preserve-3d {
    transform-style: preserve-3d;
  }
  .rotate-y {
    transform: rotateY(var(--rotate-y, 0deg));
  }
}

/* 基础样式 */
html {
  scroll-behavior: smooth;
}

body {
  font-family: 'SF Pro Display', '-apple-system', 'BlinkMacSystemFont', sans-serif;
  overflow-x: hidden;
}

/* 自定义滚动条 */
::-webkit-scrollbar {
  width: 6px;
}

::-webkit-scrollbar-track {
  background: transparent;
}

::-webkit-scrollbar-thumb {
  background: rgba(0, 113, 227, 0.3);
  border-radius: 3px;
}

::-webkit-scrollbar-thumb:hover {
  background: rgba(0, 113, 227, 0.5);
}

/* 动画元素 */
.reveal {
  opacity: 0;
  transform: translateY(30px) scale(0.98);
  transition: all 0.8s cubic-bezier(0.23, 1, 0.32, 1);
}

.reveal.active {
  opacity: 1;
  transform: translateY(0) scale(1);
}

.parallax {
  transition: transform 0.5s cubic-bezier(0.23, 1, 0.32, 1);
}

/* Siri波浪动画 */
.siri-wave {
  position: relative;
  width: 80px;
  height: 80px;
}

.siri-wave span {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  border-radius: 50%;
  border: 2px solid rgba(0, 113, 227, 0.8);
  animation: wave 2.5s infinite ease-in-out;
}

.siri-wave span:nth-child(2) {
  animation-delay: 0.4s;
}

.siri-wave span:nth-child(3) {
  animation-delay: 0.8s;
}

@keyframes wave {
  0% {
    transform: scale(0.1);
    opacity: 0;
  }
  50% {
    opacity: 1;
  }
  100% {
    transform: scale(1.2);
    opacity: 0;
  }
}

/* 语音粒子动画 */
.voice-particles {
  position: absolute;
  width: 100%;
  height: 100%;
  pointer-events: none;
  overflow: hidden;
}

.particle {
  position: absolute;
  background: rgba(0, 113, 227, 0.7);
  border-radius: 50%;
  opacity: 0;
}

/* 解决scoped样式穿透问题 */
::v-deep .animate-float {
  animation: float 6s ease-in-out infinite;
}

::v-deep .animate-glow {
  animation: glow 2s ease-in-out infinite alternate;
}

@keyframes float {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-20px); }
}

@keyframes glow {
  0% { box-shadow: 0 0 15px rgba(0, 113, 227, 0.5); }
  100% { box-shadow: 0 0 30px rgba(0, 113, 227, 0.8); }
}
</style>
    