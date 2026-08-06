# 李敏个人作品集网站 - 技术文档

## 技术栈

### 核心
- **HTML5** — 单页静态网站
- **CSS3** — 原生 CSS，无预处理器
- **JavaScript (ES6+)** — 原生 JS，无框架

### 第三方库
| 库 | 版本 | 用途 |
|---|---|---|
| Lenis | 1.1.13 | 平滑滚动，带惯性和阻尼 |
| GSAP | 3.12.5 | 动画引擎 |
| GSAP ScrollTrigger | 3.12.5 | 滚动触发动画 |

### CDN 引入
```html
<script src="https://unpkg.com/lenis@1.1.13/dist/lenis.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/gsap.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/ScrollTrigger.min.js"></script>
```

## 关键实现

### 1. 平滑滚动（Lenis）
```javascript
const lenis = new Lenis({
    duration: 1.2,
    easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)),
    smoothWheel: true,
});

// Lenis 与 ScrollTrigger 联动
lenis.on('scroll', ScrollTrigger.update);
gsap.ticker.add((time) => lenis.raf(time * 1000));
gsap.ticker.lagSmoothing(0);
```

### 2. 3D Carousel 实现
- **CSS 3D Transforms**：`perspective: 1400px` + `transform-style: preserve-3d`
- **环形分布**：每张卡片 `rotateY(calc(var(--i) * 36deg)) translateZ(420px)`
- **整体倾斜**：`rotateX(-12deg) rotateZ(3deg)`
- **拖动交互**：监听 `mousedown/mousemove/mouseup` 和 `touch` 事件
- **自动旋转**：GSAP `onUpdate` 循环增加 `rotationY`

### 3. 字符拆分高亮
```javascript
function splitChars(el) {
    const text = el.textContent;
    el.innerHTML = '';
    for (let i = 0; i < text.length; i++) {
        const span = document.createElement('span');
        span.textContent = text[i] === ' ' ? '\u00A0' : text[i];
        span.style.display = 'inline-block';
        span.style.opacity = '0.25';
        el.appendChild(span);
    }
}
// GSAP ScrollTrigger scrub 控制 opacity
```

### 4. 鼠标视差
```javascript
hero.addEventListener('mousemove', (e) => {
    const x = (e.clientX - rect.left) / rect.width - 0.5;
    const y = (e.clientY - rect.top) / rect.height - 0.5;
    gsap.to(heroBg, { x: x * 20, y: y * 20, duration: 1.2 });
    gsap.to(heroTitle, { x: x * -12, y: y * -8, duration: 1.0 });
});
```

### 5. 视频背景处理
```css
.works-video {
    position: absolute;
    width: 100%;
    height: 100%;
    object-fit: cover;
    filter: brightness(0.35) saturate(0.7);
}
```

### 6. 瀑布流布局
```css
.aigc-masonry {
    column-count: 3;
    column-gap: 14px;
}
.aigc-masonry .masonry-item {
    break-inside: avoid;
}
```

## 文件结构
```
portfolio-site/
├── index.html          # 主页面
├── assets/
│   ├── hero-bg.png     # 首页背景图
│   ├── bg-video.mp4    # 作品区视频背景
│   └── bg.png          # 备用背景图
├── PRD.md              # 产品需求文档
├── design.md           # 设计文档
└── technology.md       # 技术文档
```

## 性能优化
- 图片使用 `object-fit: cover` 避免变形
- 视频背景压暗处理，减少视觉干扰
- GSAP 动画使用 `will-change` 提示浏览器优化
- 滚动动画使用 `scrub` 和 `toggleActions` 控制触发时机

## 浏览器兼容
- 现代浏览器（Chrome、Firefox、Safari、Edge）
- CSS `backdrop-filter` 需要较新版本浏览器
- 3D Transforms 需要支持 `preserve-3d`

## 待替换内容
- `placeholder` 元素 → 真实作品图片
- `抖音` 文字头像 → 真实抖音账号头像
- `href="#"` 链接 → 真实链接地址
