# 动效与玻璃拟态

出品与维护：AIGC-DAO

本文档说明动效原则和玻璃拟态的正确使用。阅读前提：已阅读 [SKILL.md](../SKILL.md) 入口文件。

---

## 1. 动效原则

### 1.1 目的性

每个动效都应有明确目的：

| 目的 | 示例 |
|-----|------|
| 反馈操作 | 按钮点击时轻微缩放 |
| 引导注意 | 新元素淡入、错误字段抖动 |
| 表达状态 | loading spinner、进度条 |
| 维持上下文 | 页面过渡保持元素位置感 |
| 解释关系 | 展开/折叠动画显示父子关系 |

**不用于**：纯装饰、炫技、填充空白。

### 1.2 时长参考

```css
:root {
  /* 微交互 - hover、toggle、小按钮 */
  --duration-fast: 100ms;
  
  /* 默认 - 大多数过渡 */
  --duration-default: 150ms;
  
  /* 展开/折叠 */
  --duration-medium: 200ms;
  
  /* 页面过渡、模态框 */
  --duration-slow: 300ms;
  
  /* 复杂动画（慎用）*/
  --duration-slower: 400ms;
}
```

**原则**：
- 快比慢好，感觉迟钝则缩短
- 小元素动画快，大元素可稍慢
- 多个元素一起动时错开启动时间（stagger）

### 1.3 缓动函数

```css
:root {
  /* 默认：进入时快，结束时减速 */
  --ease-out: cubic-bezier(0.33, 1, 0.68, 1);
  
  /* 弹性：结束时轻微反弹 */
  --ease-out-back: cubic-bezier(0.34, 1.56, 0.64, 1);
  
  /* 进入/淡入 */
  --ease-in: cubic-bezier(0.32, 0, 0.67, 0);
  
  /* 线性：进度条、spinner */
  --ease-linear: linear;
}
```

### 1.4 常用动效实现

```css
/* 按钮 hover 提升 */
.btn {
  transition: transform var(--duration-fast) var(--ease-out),
              box-shadow var(--duration-fast) var(--ease-out);
}
.btn:hover {
  transform: translateY(-1px);
  box-shadow: var(--shadow-md);
}
.btn:active {
  transform: translateY(0);
}

/* 淡入 */
@keyframes fade-in {
  from { opacity: 0; }
  to { opacity: 1; }
}

.fade-in {
  animation: fade-in var(--duration-default) var(--ease-out);
}

/* 从下方滑入 */
@keyframes slide-up {
  from {
    opacity: 0;
    transform: translateY(8px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.slide-up {
  animation: slide-up var(--duration-medium) var(--ease-out);
}

/* 展开折叠 - 使用 details/summary */
details > summary {
  cursor: pointer;
  list-style: none;
}
details > summary::marker,
details > summary::-webkit-details-marker {
  display: none;
}

/* 内容区动画需要 JS 辅助或使用 grid 技巧 */
```

---

## 2. Reduced Motion

### 2.1 尊重用户偏好

```css
/* 减弱动效偏好 */
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

或者更精细的控制：

```css
@media (prefers-reduced-motion: reduce) {
  .btn {
    transition: none;
  }
  
  .fade-in {
    animation: none;
    opacity: 1;
  }
  
  /* 保留必要的状态指示器 */
  .spinner {
    animation: spin 600ms linear infinite;
  }
}
```

### 2.2 测试方法

- **Chrome DevTools**：Rendering → Emulate CSS media feature prefers-reduced-motion
- **macOS**：系统偏好设置 → 辅助功能 → 显示 → 减少动态效果
- **Windows**：设置 → 轻松使用 → 显示 → 在 Windows 中显示动画

---

## 3. 玻璃拟态 (Glassmorphism)

### 3.1 何时使用

✅ **适合**：
- 需要层次感的悬浮导航栏
- 覆盖在动态背景上的弹窗
- 需要"穿透感"的侧边栏

❌ **不适合**：
- 主要内容区域
- 所有卡片都用毛玻璃
- 背景是静态单色时
- 大面积使用

### 3.2 基础实现

```css
.glass {
  /* 背景毛玻璃效果 */
  background: rgb(255 255 255 / 0.7);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  
  /* 边框增强层次感 */
  border: 1px solid rgb(255 255 255 / 0.3);
  
  /* 圆角和阴影 */
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-lg);
}

/* 暗色模式 */
@media (prefers-color-scheme: dark) {
  .glass {
    background: rgb(30 30 30 / 0.7);
    border: 1px solid rgb(255 255 255 / 0.1);
  }
}
```

### 3.3 必须的回退

`backdrop-filter` 在某些浏览器不支持或被禁用（如 Firefox 需要手动开启）。

```css
.glass {
  /* 回退：实色背景 */
  background: rgb(245 245 245 / 0.95);
  
  /* 支持时使用毛玻璃 */
  @supports (backdrop-filter: blur(12px)) {
    background: rgb(255 255 255 / 0.7);
    backdrop-filter: blur(12px);
    -webkit-backdrop-filter: blur(12px);
  }
}
```

### 3.4 对比度保证

毛玻璃上的文字可能因背景不同而难以阅读。

```css
.glass {
  /* 较高基础不透明度 */
  background: rgb(255 255 255 / 0.85);
}

/* 或者添加文字阴影 */
.glass-text {
  text-shadow: 0 0 10px rgb(255 255 255 / 0.5);
}

/* 暗色模式下背景色更深 */
@media (prefers-color-scheme: dark) {
  .glass {
    background: rgb(20 20 20 / 0.9);
  }
}
```

### 3.5 性能注意

`backdrop-filter` 会触发合成层，大面积使用可能影响性能。

```css
/* 限制影响范围 */
.glass {
  will-change: backdrop-filter;
  contain: layout style paint;
}
```

---

## 4. 过渡效果示例

### 4.1 页面过渡

```css
/* 简单的淡入淡出 */
.page-enter {
  opacity: 0;
}

.page-enter-active {
  opacity: 1;
  transition: opacity var(--duration-slow) var(--ease-out);
}

.page-exit {
  opacity: 1;
}

.page-exit-active {
  opacity: 0;
  transition: opacity var(--duration-default) var(--ease-in);
}
```

### 4.2 模态框

```css
dialog {
  opacity: 0;
  transform: scale(0.95);
  transition: 
    opacity var(--duration-medium) var(--ease-out),
    transform var(--duration-medium) var(--ease-out);
}

dialog[open] {
  opacity: 1;
  transform: scale(1);
}

dialog::backdrop {
  background: rgb(0 0 0 / 0);
  transition: background var(--duration-medium) var(--ease-out);
}

dialog[open]::backdrop {
  background: rgb(0 0 0 / 0.5);
}
```

### 4.3 下拉菜单

```css
.dropdown-menu {
  opacity: 0;
  transform: translateY(-4px);
  pointer-events: none;
  transition: 
    opacity var(--duration-fast) var(--ease-out),
    transform var(--duration-fast) var(--ease-out);
}

.dropdown[open] .dropdown-menu {
  opacity: 1;
  transform: translateY(0);
  pointer-events: auto;
}
```

---

## 5. 不推荐的做法

### 5.1 过度使用

```css
/* ❌ 不要这样 - 所有东西都在动 */
* {
  transition: all 0.3s ease;
}

/* ❌ 不要这样 - 无意义的弹跳 */
.card:hover {
  animation: bounce 0.5s infinite;
}
```

### 5.2 忽略可访问性

```css
/* ❌ 不要这样 - 忽略 reduced motion 偏好 */
.hero {
  animation: float 3s ease-in-out infinite;
}

/* ✅ 应该这样 */
@media (prefers-reduced-motion: no-preference) {
  .hero {
    animation: float 3s ease-in-out infinite;
  }
}
```

### 5.3 玻璃拟态滥用

```css
/* ❌ 不要这样 - 到处都是毛玻璃 */
.card { backdrop-filter: blur(10px); }
.button { backdrop-filter: blur(5px); }
.input { backdrop-filter: blur(8px); }

/* ❌ 不要这样 - 没有回退 */
.glass {
  background: transparent;
  backdrop-filter: blur(20px);
}
```
