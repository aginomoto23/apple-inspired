# UI 基础：视觉原则与设计决策

出品与维护：AIGC-DAO

本文档详细说明 apple-inspired skill 的视觉基础原则。阅读前提：已阅读 [SKILL.md](../SKILL.md) 入口文件。

---

## 1. 信息层级

### 1.1 单一焦点原则
每个页面/视图应有一个明确的视觉重心：
- 首页：核心价值主张或主要 CTA
- 列表页：列表内容本身
- 详情页：主要信息区块
- 操作页：当前步骤或表单

**执行方式**：
- 主焦点使用最大字号、最强对比、最显眼位置
- 次要元素在视觉上"退后"：更小、更灰、更边缘
- 移除与当前任务无关的装饰

### 1.2 三级层次系统

```
┌─────────────────────────────────────┐
│  主标题（最大、最重）                   │
│  24-48px, font-weight: 600-700      │
├─────────────────────────────────────┤
│  副标题/关键信息（中等）                │
│  16-20px, font-weight: 500-600      │
├─────────────────────────────────────┤
│  正文/辅助信息（正常）                  │
│  14-16px, font-weight: 400          │
│  颜色可用灰色降低视觉权重               │
└─────────────────────────────────────┘
```

### 1.3 空间层次
通过间距表达关系：
- **紧密关联**：8-12px（标题与其描述）
- **同组元素**：16-24px（列表项之间）
- **不同区块**：32-64px（导航与内容、内容与页脚）

---

## 2. 字体系统

### 2.1 系统字体栈

```css
:root {
  /* 主字体栈 - 不依赖商业字体 */
  --font-sans: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, 
               "Helvetica Neue", Arial, "Noto Sans SC", "PingFang SC", 
               "Microsoft YaHei", sans-serif;
  
  /* 等宽字体栈 */
  --font-mono: ui-monospace, SFMono-Regular, "SF Mono", Menlo, 
               Consolas, "Liberation Mono", monospace;
}
```

**说明**：
- SF Pro/SF Mono 是系统字体，macOS/iOS 自动使用，无需（也不应）手动引入
- Windows 会回退到 Segoe UI，Linux 回退到系统 sans-serif
- 中文字体：macOS 用 PingFang SC，Windows 用 Microsoft YaHei

### 2.2 字体大小比例

使用和谐的比例关系（约 1.25 倍递增）：

```css
:root {
  --text-xs: 0.75rem;    /* 12px - 辅助信息、标签 */
  --text-sm: 0.875rem;   /* 14px - 次要正文 */
  --text-base: 1rem;     /* 16px - 主正文 */
  --text-lg: 1.125rem;   /* 18px - 强调正文 */
  --text-xl: 1.25rem;    /* 20px - 小标题 */
  --text-2xl: 1.5rem;    /* 24px - 区块标题 */
  --text-3xl: 1.875rem;  /* 30px - 页面标题 */
  --text-4xl: 2.25rem;   /* 36px - 大标题 */
  --text-5xl: 3rem;      /* 48px - 展示标题 */
}
```

### 2.3 中文排版要点

```css
/* 中文正文优化 */
.chinese-body {
  font-size: 1rem;        /* 16px 起步，可用 17-18px */
  line-height: 1.75;      /* 中文需要更大行高 */
  letter-spacing: 0.02em; /* 轻微字间距改善阅读 */
}

/* 中文标题 */
.chinese-heading {
  font-weight: 600;       /* 不用 700/800，中文字体过重不好看 */
  letter-spacing: 0.04em; /* 标题可稍大字间距 */
}
```

**规则**：
- 避免中文斜体（多数中文字体无斜体设计）
- 中英混排时标点用中文标点
- 数字和英文会自动使用英文字体（字体栈顺序保证）

### 2.4 字重使用

```css
:root {
  --font-normal: 400;    /* 正文 */
  --font-medium: 500;    /* 强调、副标题 */
  --font-semibold: 600;  /* 标题、按钮 */
  --font-bold: 700;      /* 特别强调（慎用）*/
}
```

**规则**：只用必要的字重区分层级，不为装饰加粗。

---

## 3. 间距系统

### 3.1 基于 4px 的比例

```css
:root {
  --space-0: 0;
  --space-px: 1px;
  --space-0-5: 0.125rem;  /* 2px */
  --space-1: 0.25rem;     /* 4px */
  --space-1-5: 0.375rem;  /* 6px */
  --space-2: 0.5rem;      /* 8px */
  --space-2-5: 0.625rem;  /* 10px */
  --space-3: 0.75rem;     /* 12px */
  --space-4: 1rem;        /* 16px */
  --space-5: 1.25rem;     /* 20px */
  --space-6: 1.5rem;      /* 24px */
  --space-8: 2rem;        /* 32px */
  --space-10: 2.5rem;     /* 40px */
  --space-12: 3rem;       /* 48px */
  --space-16: 4rem;       /* 64px */
  --space-20: 5rem;       /* 80px */
  --space-24: 6rem;       /* 96px */
}
```

### 3.2 间距使用指南

| 场景 | 推荐间距 |
|-----|---------|
| 图标与文字 | 4-8px |
| 表单标签与输入框 | 4-8px |
| 按钮内边距 | 8-12px 垂直, 16-24px 水平 |
| 列表项之间 | 8-16px |
| 卡片内边距 | 16-24px |
| 卡片之间 | 16-24px |
| 区块之间 | 32-64px |
| 页面边距（移动端）| 16-20px |
| 页面边距（桌面）| 24-64px |

### 3.3 对齐原则

- 建立隐形网格，元素边缘对齐
- 文字左对齐（中英文都是）
- 数字右对齐（表格中）
- 避免视觉孤岛（单独飘着的小元素）

---

## 4. 色彩系统

### 4.1 中性色板

```css
:root {
  /* 亮色模式 */
  --gray-50: #fafafa;   /* 页面背景 */
  --gray-100: #f5f5f5;  /* 卡片背景、斑马纹 */
  --gray-200: #e5e5e5;  /* 边框、分隔线 */
  --gray-300: #d4d4d4;  /* 禁用边框 */
  --gray-400: #a3a3a3;  /* 占位符文字 */
  --gray-500: #737373;  /* 次要文字 */
  --gray-600: #525252;  /* 图标 */
  --gray-700: #404040;  /* 次标题 */
  --gray-800: #262626;  /* 主标题 */
  --gray-900: #171717;  /* 强调文字 */
  --gray-950: #0a0a0a;  /* 最深 */
}
```

### 4.2 强调色

选择一个主强调色，用于 CTA、链接、选中状态：

```css
:root {
  /* 示例：蓝色系（可替换） */
  --accent-50: #eff6ff;
  --accent-100: #dbeafe;
  --accent-200: #bfdbfe;
  --accent-500: #3b82f6;  /* 主色 */
  --accent-600: #2563eb;  /* hover */
  --accent-700: #1d4ed8;  /* active */
  --accent-900: #1e3a8a;  /* 深色 */
}
```

### 4.3 语义色

```css
:root {
  /* 成功 */
  --success: #22c55e;
  --success-bg: #f0fdf4;
  
  /* 警告 */
  --warning: #f59e0b;
  --warning-bg: #fffbeb;
  
  /* 错误 */
  --error: #ef4444;
  --error-bg: #fef2f2;
  
  /* 信息 */
  --info: #3b82f6;
  --info-bg: #eff6ff;
}
```

### 4.4 对比度检查

文字颜色与背景的对比度要求：
- 正文（< 18px）：≥ 4.5:1
- 大字（≥ 18px 或 14px 加粗）：≥ 3:1
- 非文字元素（图标、边框）：≥ 3:1

工具：在浏览器 DevTools 中选中元素可查看对比度。

---

## 5. 圆角系统

### 5.1 比例原则

组件越大，圆角越大：

```css
:root {
  --radius-none: 0;
  --radius-sm: 0.25rem;   /* 4px - 小标签、徽章 */
  --radius-default: 0.375rem; /* 6px - 按钮、输入框 */
  --radius-md: 0.5rem;    /* 8px - 卡片内元素 */
  --radius-lg: 0.75rem;   /* 12px - 卡片 */
  --radius-xl: 1rem;      /* 16px - 大卡片、弹窗 */
  --radius-2xl: 1.5rem;   /* 24px - 特大容器 */
  --radius-full: 9999px;  /* 胶囊形 */
}
```

### 5.2 使用指南

| 组件 | 推荐圆角 |
|-----|---------|
| 小按钮、标签 | 4-6px |
| 普通按钮 | 6-8px |
| 输入框 | 6-8px |
| 卡片 | 12-16px |
| 弹窗/模态框 | 16-24px |
| 头像 | 50%（圆形）或 8-12px |
| 大容器背景 | 16-24px |

---

## 6. 阴影系统

### 6.1 层级阴影

```css
:root {
  /* 细微提升 - 卡片悬浮 */
  --shadow-sm: 0 1px 2px 0 rgb(0 0 0 / 0.05);
  
  /* 默认提升 - 下拉菜单 */
  --shadow-default: 0 1px 3px 0 rgb(0 0 0 / 0.1), 
                    0 1px 2px -1px rgb(0 0 0 / 0.1);
  
  /* 中等提升 - 弹出层 */
  --shadow-md: 0 4px 6px -1px rgb(0 0 0 / 0.1), 
               0 2px 4px -2px rgb(0 0 0 / 0.1);
  
  /* 较高提升 - 弹窗 */
  --shadow-lg: 0 10px 15px -3px rgb(0 0 0 / 0.1), 
               0 4px 6px -4px rgb(0 0 0 / 0.1);
  
  /* 最高提升 - 模态框 */
  --shadow-xl: 0 20px 25px -5px rgb(0 0 0 / 0.1), 
               0 8px 10px -6px rgb(0 0 0 / 0.1);
}
```

### 6.2 使用原则

- 阴影表示层级，不用于装饰
- 静止元素少用/不用阴影
- hover 时可增加阴影表示"提起"
- 暗色模式下阴影更深、透明度更高

---

## 7. 边框系统

```css
:root {
  --border-width: 1px;
  --border-color: var(--gray-200);
  --border-color-strong: var(--gray-300);
  
  /* 焦点环 */
  --ring-width: 2px;
  --ring-color: var(--accent-500);
  --ring-offset: 2px;
}
```

### 7.1 使用指南

- 分隔内容区块：用 `border-bottom` 或 `border-top`
- 表单元素：细边框 + 焦点时强调色
- 卡片：可用细边框，或不用边框只用阴影
- 避免：同时使用强边框 + 强阴影

---

## 8. 布局密度

### 8.1 按界面类型选择

```css
/* 宽松 - 营销页、落地页 */
.density-loose {
  --content-padding: var(--space-8);
  --item-gap: var(--space-6);
  --section-gap: var(--space-16);
}

/* 默认 - 普通应用 */
.density-default {
  --content-padding: var(--space-4);
  --item-gap: var(--space-3);
  --section-gap: var(--space-8);
}

/* 紧凑 - 数据密集、工具型 */
.density-compact {
  --content-padding: var(--space-2);
  --item-gap: var(--space-2);
  --section-gap: var(--space-4);
}
```

### 8.2 内容宽度

```css
:root {
  --content-width-sm: 640px;   /* 阅读内容 */
  --content-width-md: 768px;   /* 表单、详情 */
  --content-width-lg: 1024px;  /* 列表、仪表盘 */
  --content-width-xl: 1280px;  /* 全宽应用 */
}
```

**规则**：
- 阅读型内容限制 60-80 字符宽
- 数据表格可更宽但要有水平滚动
- 不要让内容在超宽屏上无限拉伸
