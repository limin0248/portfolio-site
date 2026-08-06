# 李敏个人作品集网站 - 设计文档

## 视觉风格

### 色彩体系
- **主色（深绿）**: `#1a3a2a` / `#164a32` / `#124028` — 背景、卡片底色
- **强调色（黄）**: `#fff200` — 标题、装饰点、高亮文字
- **文字色**: `#fff`（主文字）、`rgba(255,255,255,0.7)`（次要文字）
- **玻璃质感**: `rgba(255,255,255,0.05)` + `backdrop-filter: blur(12px)` + 半透明白色边框

### 字体
- **中文**: `'Noto Sans SC', sans-serif`
- **英文/标题**: `'Playfair Display', serif`
- 大标题使用 Playfair Display，营造优雅感

### 背景处理
- 首页：全屏图片背景，使用 `object-fit: cover` 铺满
- 作品区：视频背景，压暗降饱和滤镜
- 关于我/联系我：深绿渐变背景

## 页面结构

### 首页（Hero）
```
┌─────────────────────────────────────┐
│  ● 下载简历              [HELLO]    │
│                                     │
│           P O R T                   │
│          F O L I O                  │
│                                     │
│                                     │
│  ┌────┐                             │
│  │照片│  ● 个人作品集               │
│  └────┘                             │
│         2023~2026                   │
│         portfolio                   │
│         hello@limin.com             │
└─────────────────────────────────────┘
```

### 作品展示区（统一视频背景）
- AIGC 作品：3列瀑布流
- 文字稿件：左侧时间线
- 视频作品：横向轮播卡片
- 平面设计：错落拼贴网格
- 摄影作品：3D 环绕 Carousel

## 各板块独特设计

### AIGC 作品 — 瀑布流画廊
- `column-count: 3`，`column-gap: 14px`
- 卡片 `break-inside: avoid`
- 三种比例交替：`3/4`、`1/1`、`4/3`
- Hover：`transform: translateY(-4px)` + `scale(1.08)`
- 底部遮罩渐变 + 标题/标签滑入

### 文字稿件 — 时间线
- 左侧 `2px` 竖线（黄色渐变）
- 圆点节点：`10px` 圆形，hover 发光
- 文字 hover 右移 + 变色

### 视频作品 — 横向轮播
- `flex` 布局，`overflow-x: auto`
- 卡片固定宽度 `280px`
- 播放按钮三角形 hover 浮现

### 平面设计 — 错落拼贴
- `grid` 布局，`3列`
- 每张卡片预设不同 `rotate` 和 `translateY`
- Hover：`rotate(0deg)` + `translateY(-8px)` + `scale(1.03)`

### 摄影作品 — 3D Carousel
- 10 张卡片，`rotateY` 间隔 `36deg`
- `translateZ(420px)` 形成环形
- 整体倾斜：`rotateX(-12deg) rotateZ(3deg)`
- 3D 丝带：`translateZ` 前后两层（文字层 + 模糊阴影层）
- 右下角抖音按钮：圆形头像 +  pill 形状按钮

## 动画设计

### 入场动画
- 板块标题：3D 翻转进入（`rotateX: 12° → 0°`）
- 瀑布流卡片：stagger 依次从下方浮现
- 时间线：从左侧滑入
- 拼贴：随机旋转归正
- 摄影 carousel：整体淡入 + 持续旋转

### 滚动动画
- 标题字符拆分高亮：滚动时字符逐个从 `opacity: 0.25` 变为 `1`
- 摄影作品视差：内部图片在画框内缓慢位移
- 滚动加速 carousel 旋转

### 交互动画
- 鼠标视差（首页）：背景偏移 `20px`，标题反向偏移 `12px`
- 导航点击：Lenis 丝滑滚动
- 卡片 hover：浮起、阴影加深、边框高亮

## 玻璃质感规范
```css
glass-card:
  background: rgba(255,255,255,0.05)
  backdrop-filter: blur(12px)
  border: 1px solid rgba(255,255,255,0.15)
  border-radius: 16px
  box-shadow: 0 4px 30px rgba(0,0,0,0.1)
```

## 响应式断点
- 桌面：`> 768px`
- 平板/移动端：`<= 768px`
  - 瀑布流：`column-count: 2`
  - 拼贴网格：`grid-template-columns: repeat(2, 1fr)`
