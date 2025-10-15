<script setup>
import { onMounted, ref, computed } from 'vue';
import BookPage from './view/BookPage.vue';
import PhoneShowCase from './view/PhoneShowCase.vue';
import FloatPhoto from './view/FloatPhoto.vue';
import IphoneTakePhoto from './view/IphoneTakePhoto.vue';
import webOpenHead from './view/webOpenHead.vue';
import aiseize from './view/AISEZE.vue';


onMounted(() => {
	// 移动端菜单
	const nav = document.querySelector('.nav');
	const toggle = document.querySelector('.nav-toggle');
	if (toggle) {
		toggle.addEventListener('click', () => {
			if (nav) nav.classList.toggle('open');
		});
	}
	// 加载进度条
	(function loadingBar() {
		const bar = document.getElementById('loadBar');
		const pct = document.getElementById('loadPct');
		if (!bar || !pct) return;
		let p = 0;
		const timer = setInterval(() => {
			p = Math.min(100, p + Math.random() * 12);
			bar.style.width = p.toFixed(0) + '%';
			pct.textContent = p.toFixed(0) + '%';
			if (p >= 100) clearInterval(timer);
		}, 200);
	})();

	//监听
	
	

	// 视差/微倾斜效果
	document.querySelectorAll('[data-tilt]').forEach((card) => {
		let rx = 0, ry = 0, fr = 20;
		const el = card;
		const onMove = (e) => {
			const r = el.getBoundingClientRect();
			const x = e.clientX - r.left;
			const y = e.clientY - r.top;
			ry = ((x / r.width) - 0.5) * fr;
			rx = -((y / r.height) - 0.5) * fr;
			el.style.transform = `perspective(800px) rotateX(${rx}deg) rotateY(${ry}deg) translateY(-4px)`;
		};
		const onLeave = () => {
			el.style.transform = '';
		};
		el.addEventListener('mousemove', onMove);
		el.addEventListener('mouseleave', onLeave);
	});
	// 背景动效
	(function fx() {
		const canvas = document.getElementById('fxCanvas');
		if (!canvas || !canvas.getContext) return;
		const ctx = canvas.getContext('2d');
		if (!ctx) return;
		let w, h, t = 0;
		const resize = () => {
			w = canvas.width = canvas.offsetWidth;
			h = canvas.height = canvas.offsetHeight;
		};
		window.addEventListener('resize', resize, { passive: true });
		resize();
		function noise(x, y, t) {
			return (Math.sin(x * 0.002 + t) + Math.cos(y * 0.002 - t*1.2) + Math.sin((x+y) * 0.001 - t*0.7)) / 3;
		}
		function draw() {
			t += 0.006;
			const img = ctx.createImageData(w, h);
			for (let y = 0; y < h; y+=2) {
				for (let x = 0; x < w; x+=2) {
					const n = noise(x, y, t);
					const i = ((y*w) + x) * 4;
					const c1 = 12 + n * 18;
					const c2 = 20 + n * 28;
					img.data[i] = 10;
					img.data[i+1] = c2;
					img.data[i+2] = c1;
					img.data[i+3] = 45;
				}
			}
			ctx.putImageData(img, 0, 0);
			requestAnimationFrame(draw);
		}
		draw();
	})();
});

// 对比区：机型选项与文案数据
const modelOptions = [
	{ value: 'iphone15pro', label: 'iPhone 15 Pro' },
	{ value: 'iphone15', label: 'iPhone 15' },
	{ value: 'iphone14pro', label: 'iPhone 14pro' },
	{ value: 'iphone14', label: 'iPhone 14' }
];
const selectedModel = ref('iphone15pro');

const compareCardsByModel = {
	iphone15pro: [
		{ img: '/assets/img/explan011.jpg',title: '一体成型机身' },
		{ img: '/assets/img/explan008.jpg', small: 'iPhone 17 Pro Max', title: '增加 14 小时', note: '视频播放最长' },
		{ img: '/assets/img/explan010.jpg', small: '后摄全上 4800 万像素', title: '相机控制功能' },
		{ img: '/assets/SJJH/ZS03.jpg'},
		{ img: '/assets/SJJH/ZS02.jpg' },
		{ img: '/assets/SJJH/ZS01.jpg'}
	],
	iphone15: [
		{ img: '/assets/img/explan011.jpg',title: '一体成型机身' },
		{ img: '/assets/img/explan008.jpg', small: 'iPhone 17 Pro Max', title: '增加 17 小时', note: '视频播放最长' },
		{ img: '/assets/img/explan010.jpg', small: '后摄全上 4800 万像素', title: '相机控制功能' },
		{ img: '/assets/SJJH/ZS03.jpg'},
		{ img: '/assets/SJJH/ZS02.jpg' },
		{ img: '/assets/SJJH/ZS01.jpg'}
	],
	iphone14pro: [
		{ img: '/assets/img/explan011.jpg',title: '一体成型机身' },
		{ img: '/assets/img/explan008.jpg', small: 'iPhone 17 Pro Max', title: '增加 14 小时', note: '视频播放最长' },
		{ img: '/assets/img/explan010.jpg', small: '后摄全上 4800 万像素', title: '相机控制功能' },
		{ img: '/assets/img/explan012.jpg',small: '处理器性能', title: '提速至2倍'},
		{ img: '/assets/SJJH/ZS01.jpg' },
		{ img: '/assets/SJJH/ZS03.jpg'}
	],
	iphone14: [
		{ img: '/assets/img/explan011.jpg',title: '一体成型机身' },
		{ img: '/assets/img/explan008.jpg', small: 'iPhone 17 Pro Max', title: '增加 17 小时', note: '视频播放最长' },
		{ img: '/assets/img/explan010.jpg', small: '后摄全上 4800 万像素', title: '相机控制功能' },
		{ img: '/assets/img/explan012.jpg',small: '处理器性能', title: '提速至2.2倍'},
		{ img: '/assets/SJJH/ZS01.jpg' },
		{ img: '/assets/SJJH/ZS04.jpg',small: 'Pro 级显示屏支持 ProMotion 技术自适应刷新率最高达 120Hz'}
	]
};

const currentCards = computed(() => compareCardsByModel[selectedModel.value] || []);
</script>

<template>
	<header class="site-header">
			<div class="loading">
				<span class="pct" id="loadPct">%</span>
				<div class="line"><span class="bar" id="loadBar"></span></div>
			</div>
		<div class="container">
			<img src="/assets/img/iPhoneLogo2.jpg" class="iphone-img">
			<a href="#" class="logo">A<span>p</span>p<span>l</span>e</a>
			<nav class="nav">
				<button class="nav-toggle" aria-label="Toggle menu">
					<span></span><span></span><span></span>
				</button>
				<ul class="nav-list">
					<li><a href="/src/view/OtherWeb/JSZC.html">技术支持</a></li>
					<li><a href="/src/view/OtherWeb/JT02.html">家庭</a></li>
					<li><a href="/src/view/OtherWeb/PLQ.html">评论区</a></li>
					<li><a href="/src/view/OtherWeb/YL02.html">Apple服务</a></li>
					<li><a href="#connect" class="btn btn-small btn-primary">登录</a></li>
				</ul>
			</nav>
		</div>
	</header>

	<main>
		<section class="hero" id="top">
			<div class="webopen-wrapper">
      <!-- 背景模块 -->
      <web-open-head></web-open-head>

      <!-- 购买按钮 -->
      <a href="https://www.apple.com.cn/shop/buy-iphone/iphone-17-pro" class="btn btn-primary  buy-btn">购买</a>
	  <div class="location-web">RMB 375/月起或 RMB 8999 起**<br>
		iPhone 16 Pro Max 现仅可在线购买，并可选择免费送货或前往 Apple Store 零售店取货。</div>
    </div>
			<div class="container">
				<h3 class="headline fade-in-delay" style="margin-left: 260px;margin-top: 25px;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
					一体成型机身<br></br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
					锻造一身强悍</h3>
				<p class="subhead fade-in-delay-2" style="margin-left: 350px;">
					iPhone 16 Pro 和 iPhone 16 Pro Max 强悍登场，由内而外精心设计，<br>
					再创 iPhone 实力巅峰。全新设计以五级钛金属一体成型机身为核心，<br>
					将性能、电池容量和耐用性全面拉满。
				</p>
				<br><br>
				<article class="card project" data-tilt>
					           <a href="#" class="card-link">
						<div class="media ratio-16x9 shimmer">
							<img src="/assets/img/explan007.jpg" alt="Trince — Web3 audit platform" loading="lazy">
						</div>
						<div class="card-body">
							<h3>预购中</h3>
							<p class="muted">RMB 375/月起或 RMB 8999 起*</p>
							<ul class="meta">
								<li>将于 2025</li>
								<li>9 月 19 日发售</li>
								<li>现仅可在线预购，并可选择免费送货或前往 Apple Store 零售店取货。</li>
							</ul>
						</div>
					</a>
				</article>
			</div>
			<div class="hero-bg" aria-hidden="true">
				<canvas id="fxCanvas" ></canvas>
			</div>
		</section>



		<section class="section approach" id="services">
			<div class="container">
				<div style="margin-left: 480px;">
				<h2 class="section-title" style="margin-left: 35px;">Iphone 重点介绍</h2>
				

				<div class="chips">
					<span class="chip">机身</span>
					<span class="chip">摄影</span>
					<span class="chip">UI/UX</span>
					<span class="chip">产品</span>
				</div></div>
				<book-page></book-page>
			</div>
		</section>

		<!-- 3d手机模型介绍等 -->
		<section class="section work" id="work">
			<div class="container">
				<div class="section-head">
					<h1 class="section-title-01" style="margin-left: 550px;">定睛细看</h1>
				</div>
				<phone-show-case></phone-show-case>
			</div>
		</section>

		<!-- 摄影按钮介绍 1-->
		<section class="section work" id="work">
			<div class="container">
				
				<h3 class="new-section-title-YB moving-text">16 pro 与其他手机拍照的照片显示</h3>
				
				<div class="section-head">
					
					<float-photo></float-photo>
				</div>
			</div>
		</section>
		<!-- 摄影按钮介绍2 -->
		<section class="section work" id="work">
			<div class="container">
				
				<h2 class="new-section-title-big-tow">摄像头<br>变焦，大幅推进</h2>
				<h8 class="new-section-title" >
					深入 iPhone 16 Pro 的拍摄系统，处处可见淋漓尽致的创新。尤其是新的长焦镜头，采用我们新一代的<br></br>
					四重反射棱镜设计，并搭载增大 56% 的传感器。从而实现最大达 200 毫米的等效焦距，即 8 倍的光学品质变焦，<br></br>
					这是 iPhone 迄今最长的长焦，整个光学变焦范围达到了 16 倍。给你更长远的构图视野，带你探索更宽广的创意表现。</h8>
				<div class="section-head01">
					
					<iphone-take-photo></iphone-take-photo>
				</div>
			</div>
		</section>
		<section class="section work" style="margin-top: -180px;" id="work">
			<div class="container">
				<div class="section-head">
					<h2 class="section-title">安全功能，
					有备无患。</h2>
					<a href="#" class="link">更多 →</a>
				</div>

				<div class="grid projects">

					<article class="card project" data-tilt>
						<a href="#" class="card-link">
							<div class="media ratio-16x9 shimmer">
								<img src="/assets/img/Dengerous01.jpg" alt="19NINETEEN — Residential Complex" loading="lazy">
							</div>
							<div class="card-body">
								<h3>标题222</h3>
								<p class="muted">车祸检测。硬件传感器结合先进的运动算法，可检测到严重车祸，并主动为你拨打求救电话。</p>
							</div>
						</a>
					</article>

					<article class="card project" data-tilt>
						<a href="#" class="card-link">
							<div class="media ratio-16x9 shimmer">
								<img src="/assets/img/Dengerous02.jpg" alt="BOUMO — Personalized Perfume" loading="lazy">
							</div>
							<div class="card-body">
								<h3>标题333</h3>
								<p class="muted">查找 App。看看挂着 AirTag 的东西在哪里；以安全可靠的方式与亲友共享你的位置；还能帮你找设备，即使设备离线也没问题。</p>
							</div>
						</a>
					</article>
				</div>
			</div>
		</section>

		<section class="section posters" id="posters">
			<div class="container">
				<div class="section-head" style="align-items:flex-start;gap:16px;">
					<h2 class="section-title">升级值不值？<br>百分百值。</h2>
					<div style="margin-left:auto;display:flex;align-items:center;gap:12px;">
						<span class="muted">将 iPhone 16 Pro 与下列 iPhone 比比看：</span>
						<select v-model="selectedModel" class="select" aria-label="选择机型">
							<option v-for="opt in modelOptions" :key="opt.value" :value="opt.value">{{ opt.label }}</option>
						</select>
					</div>
				</div>
				<div class="grid posters-grid">
					<div class="poster shimmer" v-for="(card,idx) in currentCards" :key="idx" data-tilt>
						<img :src="card.img" alt="compare" loading="lazy">
						<div class="card-body" style="position:absolute;top: 10px;left:60px;bottom:16px;color:#fff;text-shadow:0 2px 6px rgba(0,0,0,.5);">
							<div class="muted" style="opacity:.9">{{ card.small }}</div>
							<div style="font-size:28px;font-weight:800;line-height:1.1;">{{ card.title }}</div>
							<div v-if="card.note" class="muted" style="margin-top:4px;opacity:.9">{{ card.note }}</div>
						</div>
					</div>
				</div>
			</div>
		</section>

		<section class="section about" id="about">
			
			
				<aiseize></aiseize>
			
		</section>

		<section class="section connect" id="connect">
			<div class="container" style="margin-left: 350px;">
				<h2 class="section-title" style="margin-left: 200px;">我们的价值观，为我们导航。</h2>
				<p class="body">I'm always ready for any format of interaction — from concept development to post-production support. Just contact me, and we will discuss all the details.</p>
				<div class="links" style="margin-left: 150px;">
					<a href="#" class="social">QQ</a>
					<a href="#" class="social">微信</a>
					<a href="#" class="social">小红书</a>
					<a href="#" class="social">抖音</a>
					<a href="#" class="social">微博</a>
					<a href="mailto:hello@example.com" class="btn btn-primary">分享</a>
				</div>
			</div>
		</section>
	</main>

	<footer class="site-footer">
		<div class="container">
			<p class="tiny muted" style="margin-left: 500px;">© 2025 antnvslv — All rights reserved.</p>
		</div>
	</footer>
</template>

