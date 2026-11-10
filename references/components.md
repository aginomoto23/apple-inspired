# 组件规范：交互状态与无障碍

出品与维护：AIGC-DAO

本文档详细说明常用 UI 组件的实现规范。阅读前提：已阅读 [SKILL.md](../SKILL.md) 入口文件。

---

## 1. 按钮 Button

### 1.1 层级

```
┌─────────────────────────────────────────────────┐
│ Primary    最重要的操作，每个视图最多一个        │
│            实色背景 + 白色文字                   │
├─────────────────────────────────────────────────┤
│ Secondary  次要操作                              │
│            边框 + 强调色文字，或浅色背景          │
├─────────────────────────────────────────────────┤
│ Tertiary   辅助操作、链接式按钮                  │
│            无背景，仅文字 + hover 下划线/背景     │
├─────────────────────────────────────────────────┤
│ Danger     破坏性操作（删除等）                  │
│            红色系，需确认时再用                   │
└─────────────────────────────────────────────────┘
```

### 1.2 状态

```css
.btn {
  /* 基础样式 */
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: var(--space-2);
  padding: var(--space-2) var(--space-4);
  font-weight: var(--font-medium);
  border-radius: var(--radius-default);
  cursor: pointer;
  transition: background-color 150ms, box-shadow 150ms;
}

/* Primary */
.btn-primary {
  background: var(--accent-500);
  color: white;
}
.btn-primary:hover {
  background: var(--accent-600);
}
.btn-primary:active {
  background: var(--accent-700);
}

/* Focus - 所有按钮 */
.btn:focus-visible {
  outline: var(--ring-width) solid var(--ring-color);
  outline-offset: var(--ring-offset);
}

/* Disabled - 所有按钮 */
.btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
  pointer-events: none;
}

/* Loading */
.btn[aria-busy="true"] {
  position: relative;
  color: transparent;
  pointer-events: none;
}
.btn[aria-busy="true"]::after {
  content: "";
  position: absolute;
  width: 1em;
  height: 1em;
  border: 2px solid currentColor;
  border-right-color: transparent;
  border-radius: 50%;
  animation: spin 600ms linear infinite;
}
```

### 1.3 尺寸

```css
.btn-sm { padding: var(--space-1) var(--space-3); font-size: var(--text-sm); }
.btn-md { padding: var(--space-2) var(--space-4); font-size: var(--text-base); }
.btn-lg { padding: var(--space-3) var(--space-6); font-size: var(--text-lg); }
```

### 1.4 无障碍

```html
<!-- 正确：使用 button 元素 -->
<button type="button" class="btn btn-primary">保存</button>

<!-- 正确：提交表单用 submit -->
<button type="submit" class="btn btn-primary">提交</button>

<!-- 正确：加载状态 -->
<button type="button" class="btn" aria-busy="true" aria-label="正在保存...">
  保存
</button>

<!-- 错误：不要用 div -->
<div class="btn" onclick="save()">保存</div>
```

---

## 2. 表单控件 Form Controls

### 2.1 输入框 Input

```css
.input {
  display: block;
  width: 100%;
  padding: var(--space-2) var(--space-3);
  font-size: var(--text-base);
  line-height: 1.5;
  color: var(--gray-900);
  background: white;
  border: var(--border-width) solid var(--border-color);
  border-radius: var(--radius-default);
  transition: border-color 150ms, box-shadow 150ms;
}

.input::placeholder {
  color: var(--gray-400);
}

.input:hover:not(:disabled) {
  border-color: var(--gray-300);
}

.input:focus {
  outline: none;
  border-color: var(--accent-500);
  box-shadow: 0 0 0 3px var(--accent-100);
}

.input:disabled {
  background: var(--gray-100);
  cursor: not-allowed;
}

/* 错误状态 */
.input[aria-invalid="true"] {
  border-color: var(--error);
}
.input[aria-invalid="true"]:focus {
  box-shadow: 0 0 0 3px var(--error-bg);
}
```

### 2.2 表单布局

```html
<div class="form-field">
  <label for="email" class="form-label">
    邮箱地址
    <span class="required" aria-hidden="true">*</span>
  </label>
  <input 
    type="email" 
    id="email" 
    class="input" 
    required
    aria-describedby="email-hint email-error"
  />
  <p id="email-hint" class="form-hint">我们不会分享你的邮箱</p>
  <p id="email-error" class="form-error" role="alert" hidden>
    请输入有效的邮箱地址
  </p>
</div>
```

```css
.form-field {
  display: flex;
  flex-direction: column;
  gap: var(--space-1);
}

.form-label {
  font-size: var(--text-sm);
  font-weight: var(--font-medium);
  color: var(--gray-700);
}

.form-hint {
  font-size: var(--text-sm);
  color: var(--gray-500);
}

.form-error {
  font-size: var(--text-sm);
  color: var(--error);
}
```

### 2.3 选择器 Select

```css
.select {
  appearance: none;
  background-image: url("data:image/svg+xml,..."); /* 下拉箭头 */
  background-repeat: no-repeat;
  background-position: right var(--space-3) center;
  padding-right: var(--space-10);
  /* 其他样式同 input */
}
```

### 2.4 复选框和单选框

```css
.checkbox,
.radio {
  width: 1.125rem;
  height: 1.125rem;
  accent-color: var(--accent-500);
  cursor: pointer;
}

/* 自定义样式需要更复杂实现，但原生 accent-color 已足够多数场景 */
```

---

## 3. 导航 Navigation

### 3.1 顶部导航栏

```html
<header class="navbar">
  <nav aria-label="主导航">
    <a href="/" class="navbar-brand">Logo</a>
    <ul class="navbar-nav" role="list">
      <li><a href="/products" aria-current="page">产品</a></li>
      <li><a href="/about">关于</a></li>
      <li><a href="/contact">联系</a></li>
    </ul>
    <button 
      type="button" 
      class="navbar-toggle"
      aria-expanded="false"
      aria-controls="mobile-menu"
      aria-label="打开菜单"
    >
      <span class="icon-menu"></span>
    </button>
  </nav>
</header>
```

```css
.navbar {
  position: sticky;
  top: 0;
  z-index: 100;
  background: white;
  border-bottom: var(--border-width) solid var(--border-color);
}

.navbar-nav a {
  padding: var(--space-2) var(--space-3);
  color: var(--gray-600);
  text-decoration: none;
  border-radius: var(--radius-default);
}

.navbar-nav a:hover {
  color: var(--gray-900);
  background: var(--gray-100);
}

.navbar-nav a[aria-current="page"] {
  color: var(--accent-600);
  font-weight: var(--font-medium);
}
```

### 3.2 面包屑

```html
<nav aria-label="面包屑">
  <ol class="breadcrumb" role="list">
    <li><a href="/">首页</a></li>
    <li><a href="/products">产品</a></li>
    <li aria-current="page">详情</li>
  </ol>
</nav>
```

---

## 4. 列表与表格

### 4.1 列表

```css
.list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.list-item {
  padding: var(--space-3) var(--space-4);
  border-bottom: var(--border-width) solid var(--border-color);
}

.list-item:last-child {
  border-bottom: none;
}

/* 可点击列表项 */
.list-item-interactive {
  cursor: pointer;
  transition: background-color 150ms;
}

.list-item-interactive:hover {
  background: var(--gray-50);
}

.list-item-interactive:focus-visible {
  outline: var(--ring-width) solid var(--ring-color);
  outline-offset: -2px;
}
```

### 4.2 表格

```html
<div class="table-container">
  <table class="table">
    <caption class="sr-only">用户列表</caption>
    <thead>
      <tr>
        <th scope="col">姓名</th>
        <th scope="col">邮箱</th>
        <th scope="col" class="text-right">操作</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>张三</td>
        <td>zhang@example.com</td>
        <td class="text-right">
          <button type="button" class="btn btn-sm">编辑</button>
        </td>
      </tr>
    </tbody>
  </table>
</div>
```

```css
.table-container {
  overflow-x: auto;
  -webkit-overflow-scrolling: touch;
}

.table {
  width: 100%;
  border-collapse: collapse;
  font-size: var(--text-sm);
}

.table th,
.table td {
  padding: var(--space-3) var(--space-4);
  text-align: left;
  border-bottom: var(--border-width) solid var(--border-color);
}

.table th {
  font-weight: var(--font-medium);
  color: var(--gray-600);
  background: var(--gray-50);
}

.table tbody tr:hover {
  background: var(--gray-50);
}

/* 数字右对齐 */
.table .text-right {
  text-align: right;
}
```

---

## 5. 弹窗与模态框

### 5.1 对话框 Dialog

```html
<dialog class="dialog" aria-labelledby="dialog-title" aria-describedby="dialog-desc">
  <header class="dialog-header">
    <h2 id="dialog-title">确认删除</h2>
    <button 
      type="button" 
      class="dialog-close" 
      aria-label="关闭"
      onclick="this.closest('dialog').close()"
    >
      ×
    </button>
  </header>
  <div class="dialog-body">
    <p id="dialog-desc">确定要删除这个项目吗？此操作无法撤销。</p>
  </div>
  <footer class="dialog-footer">
    <button type="button" class="btn btn-secondary" onclick="this.closest('dialog').close()">
      取消
    </button>
    <button type="button" class="btn btn-danger">
      删除
    </button>
  </footer>
</dialog>
```

```css
.dialog {
  position: fixed;
  max-width: min(90vw, 500px);
  padding: 0;
  border: none;
  border-radius: var(--radius-xl);
  box-shadow: var(--shadow-xl);
}

.dialog::backdrop {
  background: rgb(0 0 0 / 0.5);
}

.dialog-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: var(--space-4);
  border-bottom: var(--border-width) solid var(--border-color);
}

.dialog-body {
  padding: var(--space-4);
}

.dialog-footer {
  display: flex;
  justify-content: flex-end;
  gap: var(--space-3);
  padding: var(--space-4);
  border-top: var(--border-width) solid var(--border-color);
}
```

### 5.2 原生 Dialog 优势

- 自动管理焦点陷阱（不会 Tab 到背景）
- 按 Esc 自动关闭
- 正确的 ARIA 角色
- 无需 JavaScript 库

**打开方式**：
```javascript
document.querySelector('.dialog').showModal();
```

---

## 6. 通知与反馈

### 6.1 Toast 消息

```html
<div class="toast" role="alert" aria-live="polite">
  <span class="toast-icon">✓</span>
  <p class="toast-message">保存成功</p>
  <button type="button" class="toast-close" aria-label="关闭">×</button>
</div>
```

```css
.toast {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-3) var(--space-4);
  background: var(--gray-900);
  color: white;
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-lg);
}

/* 变体 */
.toast-success { background: var(--success); }
.toast-error { background: var(--error); }
.toast-warning { background: var(--warning); color: var(--gray-900); }
```

### 6.2 内联提示

```html
<div class="alert alert-info" role="alert">
  <span class="alert-icon">ℹ</span>
  <div class="alert-content">
    <p class="alert-title">提示</p>
    <p class="alert-message">请在 30 天内完成验证。</p>
  </div>
</div>
```

---

## 7. 空状态与加载

### 7.1 空状态

```html
<div class="empty-state">
  <div class="empty-state-icon">📭</div>
  <h3 class="empty-state-title">暂无数据</h3>
  <p class="empty-state-desc">创建你的第一个项目开始使用</p>
  <button type="button" class="btn btn-primary">创建项目</button>
</div>
```

```css
.empty-state {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: var(--space-12) var(--space-4);
  text-align: center;
}

.empty-state-icon {
  font-size: 3rem;
  margin-bottom: var(--space-4);
}

.empty-state-title {
  font-size: var(--text-lg);
  font-weight: var(--font-semibold);
  color: var(--gray-900);
}

.empty-state-desc {
  color: var(--gray-500);
  margin-bottom: var(--space-6);
}
```

### 7.2 加载状态

```css
/* 骨架屏 - 仅用于已知布局 */
.skeleton {
  background: linear-gradient(
    90deg,
    var(--gray-200) 25%,
    var(--gray-100) 50%,
    var(--gray-200) 75%
  );
  background-size: 200% 100%;
  animation: skeleton-shimmer 1.5s infinite;
  border-radius: var(--radius-default);
}

@keyframes skeleton-shimmer {
  0% { background-position: 200% 0; }
  100% { background-position: -200% 0; }
}

/* Spinner */
.spinner {
  width: 1.5rem;
  height: 1.5rem;
  border: 2px solid var(--gray-200);
  border-top-color: var(--accent-500);
  border-radius: 50%;
  animation: spin 600ms linear infinite;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}
```

**规则**：
- 不用假按钮、假指标填充
- 加载时间超过 200ms 再显示加载指示
- 骨架屏需要反映真实布局结构

---

## 8. 键盘与焦点管理

### 8.1 焦点可见性

```css
/* 键盘焦点可见 */
:focus-visible {
  outline: var(--ring-width) solid var(--ring-color);
  outline-offset: var(--ring-offset);
}

/* 鼠标点击不显示焦点环（现代浏览器自动处理） */
:focus:not(:focus-visible) {
  outline: none;
}
```

### 8.2 跳过链接

```html
<a href="#main" class="skip-link">跳到主要内容</a>
<!-- ... 导航 ... -->
<main id="main" tabindex="-1">
  <!-- 内容 -->
</main>
```

```css
.skip-link {
  position: absolute;
  top: -100%;
  left: 0;
  z-index: 1000;
  padding: var(--space-2) var(--space-4);
  background: var(--gray-900);
  color: white;
}

.skip-link:focus {
  top: 0;
}
```

### 8.3 Tab 顺序

- 遵循视觉阅读顺序
- 不使用正的 `tabindex`（只用 0 或 -1）
- 模态框内使用焦点陷阱
- 关闭模态框后返回触发元素

---

## 9. 触控与移动端

### 9.1 触控目标

```css
/* 最小触控区域 44×44px */
.touch-target {
  min-width: 44px;
  min-height: 44px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
}

/* 小图标按钮扩大点击区域 */
.icon-btn {
  position: relative;
  padding: var(--space-2);
}

.icon-btn::before {
  content: "";
  position: absolute;
  inset: -8px; /* 扩展点击区域 */
}
```

### 9.2 移动端适配

```css
/* 移动端禁用 hover 效果 */
@media (hover: none) {
  .btn:hover {
    background: inherit; /* 不改变背景 */
  }
}

/* 触控设备增加间距防止误触 */
@media (pointer: coarse) {
  .list-item {
    padding: var(--space-4);
  }
  
  .btn {
    padding: var(--space-3) var(--space-5);
  }
}
```
