# 响应式、无障碍与性能

出品与维护：AIGC-DAO

本文档说明响应式设计、无障碍和性能优化。阅读前提：已阅读 [SKILL.md](../SKILL.md) 入口文件。

---

## 1. 响应式设计

### 1.1 内容驱动断点

不要硬套 Bootstrap 或其他框架的断点，根据**内容何时需要重排**来决定：

```css
/* 示例断点 - 根据实际内容调整 */
:root {
  --breakpoint-sm: 640px;   /* 单栏 → 可考虑两栏 */
  --breakpoint-md: 768px;   /* 移动导航 → 桌面导航 */
  --breakpoint-lg: 1024px;  /* 侧边栏出现 */
  --breakpoint-xl: 1280px;  /* 内容最大宽度 */
}
```

**测试方法**：缩放浏览器窗口，观察内容何时"挤坏"，在那里设断点。

### 1.2 移动优先 vs 桌面优先

根据项目主要用户选择，保持一致：

```css
/* 移动优先：基础样式为移动端 */
.container {
  padding: var(--space-4);
}

@media (min-width: 768px) {
  .container {
    padding: var(--space-8);
  }
}

/* 桌面优先：基础样式为桌面端 */
.container {
  padding: var(--space-8);
}

@media (max-width: 767px) {
  .container {
    padding: var(--space-4);
  }
}
```

### 1.3 弹性布局

使用 CSS Grid 和 Flexbox 实现自适应：

```css
/* 自适应网格 - 卡片布局 */
.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: var(--space-6);
}

/* 响应式 flex */
.row {
  display: flex;
  flex-wrap: wrap;
  gap: var(--space-4);
}

.col {
  flex: 1 1 300px; /* 最小 300px，可增长 */
}
```

### 1.4 避免常见问题

#### 固定高度溢出

```css
/* ❌ 不要这样 */
.card {
  height: 200px;
  overflow: hidden;
}

/* ✅ 使用 min-height 或让内容决定高度 */
.card {
  min-height: 200px;
}
```

#### 中文标题过长

```css
/* 中文可能比英文长 50%，预留空间 */
.title {
  /* 允许换行 */
  word-break: break-word;
  
  /* 或使用省略 */
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  max-width: 100%;
}

/* 多行省略 */
.description {
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
```

#### 数据溢出

```css
/* 表格水平滚动 */
.table-container {
  overflow-x: auto;
  -webkit-overflow-scrolling: touch;
}

/* 长文本自动换行 */
.data-cell {
  word-break: break-word;
}

/* 数字防止换行 */
.number {
  white-space: nowrap;
  font-variant-numeric: tabular-nums;
}
```

---

## 2. 暗色模式

### 2.1 基本实现

```css
:root {
  /* 亮色模式（默认）*/
  --color-bg: var(--gray-50);
  --color-surface: white;
  --color-text: var(--gray-900);
  --color-text-secondary: var(--gray-600);
  --color-border: var(--gray-200);
}

@media (prefers-color-scheme: dark) {
  :root {
    --color-bg: #0a0a0a;
    --color-surface: #1c1c1e;
    --color-text: #f5f5f5;
    --color-text-secondary: #a3a3a3;
    --color-border: #2c2c2e;
  }
}
```

### 2.2 不是简单反转

```css
/* ❌ 不要这样 - 纯黑背景太刺眼 */
@media (prefers-color-scheme: dark) {
  body { background: #000; color: #fff; }
}

/* ✅ 使用深灰色 */
@media (prefers-color-scheme: dark) {
  body {
    background: #0a0a0a;  /* 或 #1c1c1e */
    color: #f5f5f5;       /* 不用纯白，降低对比度 */
  }
}
```

### 2.3 阴影调整

```css
:root {
  --shadow-default: 0 1px 3px rgb(0 0 0 / 0.1);
}

@media (prefers-color-scheme: dark) {
  :root {
    /* 暗色模式下阴影更深 */
    --shadow-default: 0 1px 3px rgb(0 0 0 / 0.3);
  }
}
```

### 2.4 手动切换

```css
/* 支持手动切换和系统偏好 */
:root {
  color-scheme: light dark;
}

[data-theme="light"] {
  --color-bg: var(--gray-50);
  /* ... */
}

[data-theme="dark"] {
  --color-bg: #0a0a0a;
  /* ... */
}
```

```javascript
// 切换主题
function toggleTheme() {
  const current = document.documentElement.dataset.theme;
  const next = current === 'dark' ? 'light' : 'dark';
  document.documentElement.dataset.theme = next;
  localStorage.setItem('theme', next);
}

// 初始化：优先用户选择，其次系统偏好
function initTheme() {
  const saved = localStorage.getItem('theme');
  if (saved) {
    document.documentElement.dataset.theme = saved;
  } else if (window.matchMedia('(prefers-color-scheme: dark)').matches) {
    document.documentElement.dataset.theme = 'dark';
  }
}
```

---

## 3. 无障碍基础

### 3.1 语义化 HTML

```html
<!-- ✅ 正确 -->
<header>...</header>
<nav aria-label="主导航">...</nav>
<main>
  <article>
    <h1>标题</h1>
    <p>内容</p>
  </article>
</main>
<aside>...</aside>
<footer>...</footer>

<!-- ❌ 错误 -->
<div class="header">...</div>
<div class="nav">...</div>
<div class="main">
  <div class="article">
    <div class="title">标题</div>
    <div class="content">内容</div>
  </div>
</div>
```

### 3.2 标题层级

```html
<!-- ✅ 正确的层级 -->
<h1>页面主标题</h1>
  <h2>一级区块</h2>
    <h3>二级区块</h3>
  <h2>另一个一级区块</h2>

<!-- ❌ 跳级 -->
<h1>页面主标题</h1>
  <h3>直接到 h3？</h3>
```

### 3.3 图片替代文本

```html
<!-- 内容图片：描述内容 -->
<img src="product.jpg" alt="黑色无线耳机，放在白色背景上">

<!-- 装饰图片：空 alt 或用 CSS -->
<img src="decoration.svg" alt="" role="presentation">

<!-- 复杂图片：aria-describedby -->
<figure>
  <img src="chart.png" alt="2024 年销售趋势图" aria-describedby="chart-desc">
  <figcaption id="chart-desc">
    销售额从 1 月的 100 万增长到 12 月的 300 万，呈稳定上升趋势。
  </figcaption>
</figure>
```

### 3.4 表单标签

```html
<!-- ✅ 显式关联 -->
<label for="email">邮箱</label>
<input type="email" id="email" name="email">

<!-- ✅ 隐式关联 -->
<label>
  邮箱
  <input type="email" name="email">
</label>

<!-- ❌ 无标签 -->
<input type="email" placeholder="邮箱">
```

### 3.5 ARIA 补充

只在语义 HTML 不足时使用：

```html
<!-- 自定义开关 -->
<button 
  type="button"
  role="switch"
  aria-checked="false"
  aria-label="启用通知"
>
  <span class="switch-track">
    <span class="switch-thumb"></span>
  </span>
</button>

<!-- 可展开区域 -->
<button 
  type="button"
  aria-expanded="false"
  aria-controls="panel-1"
>
  展开详情
</button>
<div id="panel-1" hidden>
  详细内容...
</div>

<!-- 加载状态 -->
<div aria-live="polite" aria-busy="true">
  正在加载...
</div>
```

### 3.6 隐藏内容

```css
/* 视觉隐藏但屏幕阅读器可读 */
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}

/* 完全隐藏（屏幕阅读器也忽略）*/
.hidden {
  display: none;
}

/* 或使用 hidden 属性 */
<div hidden>不可见</div>
```

---

## 4. 对比度

### 4.1 要求

- **正文**（< 18px）：对比度 ≥ 4.5:1
- **大字**（≥ 18px 或 14px 加粗）：对比度 ≥ 3:1
- **非文字元素**（图标、边框等）：对比度 ≥ 3:1

### 4.2 检查方法

**浏览器 DevTools**：
1. 选中元素
2. 在 Styles 面板点击颜色方块
3. 查看 Contrast ratio

**在线工具**：
- WebAIM Contrast Checker
- Contrast Ratio by Lea Verou

### 4.3 常见问题

```css
/* ❌ 灰色文字对比度不足 */
.hint {
  color: #bbb; /* 在白色背景上对比度只有 1.9:1 */
}

/* ✅ 调深灰色 */
.hint {
  color: #737373; /* 对比度 4.6:1 */
}
```

---

## 5. 性能基础

### 5.1 CSS 性能

```css
/* ❌ 避免昂贵的选择器 */
.container div span a { }
*[class^="btn-"] { }

/* ✅ 使用类选择器 */
.btn-primary { }

/* 减少重绘重排 */
.animated {
  will-change: transform, opacity;
  contain: layout style paint;
}
```

### 5.2 图片优化

```html
<!-- 响应式图片 -->
<img 
  src="image-800.jpg"
  srcset="image-400.jpg 400w, image-800.jpg 800w, image-1200.jpg 1200w"
  sizes="(max-width: 640px) 100vw, 800px"
  alt="产品图片"
  loading="lazy"
  decoding="async"
>

<!-- 现代格式 + 回退 -->
<picture>
  <source srcset="image.avif" type="image/avif">
  <source srcset="image.webp" type="image/webp">
  <img src="image.jpg" alt="产品图片">
</picture>
```

### 5.3 字体加载

```css
/* 字体显示策略 */
@font-face {
  font-family: 'Custom Font';
  src: url('font.woff2') format('woff2');
  font-display: swap; /* 先用系统字体，加载完成后替换 */
}

/* 或者使用系统字体栈，无需加载 */
body {
  font-family: var(--font-sans);
}
```

### 5.4 减少布局抖动

```css
/* 预留图片空间 */
.image-container {
  aspect-ratio: 16 / 9;
  background: var(--gray-100);
}

/* 骨架屏尺寸匹配真实内容 */
.skeleton-title {
  height: 1.5em;
  width: 60%;
}
```

---

## 6. 缩放支持

```css
/* 使用相对单位 */
body {
  font-size: 100%; /* 用户可调整基准大小 */
}

.text {
  font-size: 1rem; /* 相对于根元素 */
}

/* 不要禁用缩放 */
/* ❌ 不要这样 */
<meta name="viewport" content="width=device-width, maximum-scale=1">

/* ✅ 允许缩放 */
<meta name="viewport" content="width=device-width, initial-scale=1">
```

---

## 7. 测试清单

### 7.1 响应式

- [ ] 手机竖屏（320-428px）
- [ ] 手机横屏 / 小平板（480-768px）
- [ ] 平板（768-1024px）
- [ ] 桌面（1024px+）
- [ ] 超宽屏（1920px+）- 内容不要无限拉伸

### 7.2 无障碍

- [ ] 纯键盘操作：Tab 顺序正确，焦点可见
- [ ] 屏幕阅读器：标题、表单、动态内容可理解
- [ ] 对比度：正文 ≥ 4.5:1
- [ ] 缩放：200% 缩放仍可用

### 7.3 暗色模式

- [ ] 所有颜色有暗色对应
- [ ] 阴影调整
- [ ] 图片/图标在暗色下可见

### 7.4 Reduced Motion

- [ ] 动画被禁用或简化
- [ ] 功能不依赖动画
