---
name: apple-inspired
description: Apply restrained Apple-inspired aesthetics to UI/frontend development, web pages, applications, components, product photography, and image-editing prompts. Use when the user asks for clean, premium, minimalist interfaces or visuals with intentional whitespace, clear hierarchy, accessible interactions, and truthful materials.
---

# Apple-inspired 视觉美学

出品与维护：AIGC-DAO

这是受 Apple 产品设计语言启发的视觉与前端工程方法，不是 Apple 官方规范，不声称完全符合 Apple Human Interface Guidelines。核心：改呈现，不乱改内容；先理解项目，再落实设计。

---

## 快速导航

本 Skill 分为入口（本文件）和按需加载的参考文档。**先读完入口**，再根据任务类型阅读对应参考。

| 参考文档 | 何时阅读 |
|---------|---------|
| [references/ui-foundations.md](references/ui-foundations.md) | 新建页面、设计系统、理解视觉原则时 |
| [references/components.md](references/components.md) | 实现或审查具体组件（按钮、表单、弹窗等）时 |
| [references/motion-glass.md](references/motion-glass.md) | 添加动效、玻璃拟态、过渡效果时 |
| [references/responsive-a11y.md](references/responsive-a11y.md) | 响应式、暗色模式、无障碍、性能优化时 |
| [references/photography.md](references/photography.md) | 产品摄影、图像精修、提示词生成时 |
| [references/workflow.md](references/workflow.md) | 完整工作流程、验收清单、交付检查时 |
| [examples/tokens.css](examples/tokens.css) | 需要 CSS 变量/设计令牌参考时 |
| [examples/demo.html](examples/demo.html) | 查看可交互的组件示例时 |

---

## 1. 任务调研（必做）

开始任何实现前，先回答：

### 1.1 了解项目现状
- **技术栈**：项目使用什么框架（React/Vue/Svelte/原生）？有无现有设计系统或组件库？
- **构建工具**：Vite/Webpack/其他？是否有 CSS 预处理器或 Tailwind？
- **代码规范**：项目的文件结构、命名惯例、测试框架是什么？

**规则**：不擅自引入新框架或重写现有架构。在现有技术栈内实现。

### 1.2 明确界面类型
不同界面类型有不同的设计优先级：

| 类型 | 特征 | 设计重点 |
|-----|------|---------|
| 营销页/落地页 | 说服、展示、转化 | 视觉冲击、留白、大字体、清晰 CTA |
| 工作型应用 | 频繁操作、效率优先 | 信息密度、快速扫描、状态清晰 |
| 数据密集界面 | 表格、仪表盘、监控 | 紧凑但可读、对齐、数据可辨认 |
| 内容阅读 | 文章、文档、博客 | 阅读舒适、行高、段落间距 |

**规则**：不一律套用巨型 hero 或大面积卡片。根据界面类型选择合适的布局密度。

### 1.3 确定设计意图
向用户确认或从上下文推断：
- 目标受众是谁？（专业用户/普通消费者/开发者）
- 主要操作路径是什么？
- 是否有品牌色/现有视觉规范需要遵守？

**输出**：一份简短的设计意图声明（2-3 句），作为后续实现的依据。

---

## 2. 核心视觉原则

以下原则转化为可执行的设计决策。详细数值和示例见 [references/ui-foundations.md](references/ui-foundations.md)。

### 2.1 信息层级
- **一个主焦点**：每个视图有且仅有一个视觉重心
- **三级层次**：主标题 → 副标题/关键信息 → 正文/辅助信息
- **内容优先**：先确定内容结构，再决定视觉样式

### 2.2 字体系统
```css
/* 系统字体栈，不依赖商业字体分发 */
--font-sans: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, "Noto Sans SC", "PingFang SC", "Microsoft YaHei", sans-serif;
--font-mono: ui-monospace, SFMono-Regular, "SF Mono", Menlo, Consolas, monospace;
```

**中文排版要点**：
- 中文正文 16-18px，行高 1.7-1.8
- 中英混排时英文字体优先（系统字体栈自动处理）
- 避免中文斜体（多数中文字体无斜体设计）

### 2.3 间距与布局
使用 4px 基准的间距比例：4, 8, 12, 16, 24, 32, 48, 64, 96

**规则**：
- 相关元素间距小，不相关元素间距大
- 组件内部间距 < 组件之间间距 < 区块之间间距
- 对齐使用隐形网格，不依赖可见边框

### 2.4 色彩
- **有限色板**：1 个强调色 + 中性灰度系列
- **语义色**：成功/警告/错误/信息各一个，不滥用
- **对比度**：正文至少 4.5:1，大字至少 3:1（WCAG AA）

### 2.5 圆角层级
小组件小圆角，大容器大圆角，保持视觉比例：
- 按钮/输入框：6-8px
- 卡片/弹窗：12-16px
- 大容器/模态：16-24px

### 2.6 阴影与边框
- **阴影**：用于层级提升（悬浮、弹窗），不用于装饰
- **边框**：用于分隔内容或表示交互状态，颜色轻淡
- 避免同时使用强边框和强阴影

---

## 3. 组件实现要点

详细规范见 [references/components.md](references/components.md)。

### 3.1 状态完整性
每个交互组件必须覆盖：
- **基础状态**：default, hover, focus-visible, active, disabled
- **业务状态**：loading, error, success, empty

### 3.2 无障碍基础
- 使用语义化 HTML（button 用 `<button>`，不用 `<div onclick>`）
- 焦点可见：`:focus-visible` 显示轮廓，`:focus:not(:focus-visible)` 可隐藏
- 键盘可达：Tab 顺序合理，Enter/Space 触发操作
- ARIA 仅在语义 HTML 不足时补充

### 3.3 触控适配
- 触控目标最小 44×44px
- 间距防止误触
- 移动端避免依赖 hover

---

## 4. 动效与玻璃拟态

详见 [references/motion-glass.md](references/motion-glass.md)。

### 4.1 动效原则
- **目的性**：动效传达状态变化或引导注意力，不做纯装饰
- **时长**：微交互 100-200ms，页面过渡 200-400ms
- **尊重偏好**：`prefers-reduced-motion: reduce` 时禁用或简化

### 4.2 玻璃拟态限制
- 仅用于需要视觉层次的悬浮元素（导航栏、弹窗）
- 必须提供 `backdrop-filter` 不支持时的回退（实色背景）
- 保证文字对比度，背景太乱时降低透明度或加深背景

---

## 5. 响应式与性能

详见 [references/responsive-a11y.md](references/responsive-a11y.md)。

### 5.1 响应式原则
- **内容驱动断点**：根据内容何时需要重排来设断点，不硬套 Bootstrap 数值
- **移动优先或桌面优先**：根据项目主要用户选择，保持一致
- **测试真实内容**：中文标题可能比英文长 50%，数据可能超出预期

### 5.2 避免常见问题
- ❌ 固定高度导致内容溢出
- ❌ 假按钮、假指标（加载时）
- ❌ 无意义的骨架屏
- ❌ 营销文案虚构数据

### 5.3 暗色模式
- 不是简单反转颜色
- 背景用深灰（#1c1c1e）不用纯黑
- 降低白色文字亮度，减少高对比疲劳
- 阴影改用更深的黑色或降低不透明度

---

## 6. CSS 设计令牌

完整示例见 [examples/tokens.css](examples/tokens.css)。

```css
:root {
  /* 间距比例 */
  --space-1: 0.25rem;  /* 4px */
  --space-2: 0.5rem;   /* 8px */
  --space-3: 0.75rem;  /* 12px */
  --space-4: 1rem;     /* 16px */
  --space-6: 1.5rem;   /* 24px */
  --space-8: 2rem;     /* 32px */
  
  /* 字体大小 */
  --text-sm: 0.875rem;
  --text-base: 1rem;
  --text-lg: 1.125rem;
  --text-xl: 1.25rem;
  --text-2xl: 1.5rem;
  
  /* 中性色 */
  --gray-50: #fafafa;
  --gray-100: #f5f5f5;
  --gray-200: #e5e5e5;
  --gray-500: #737373;
  --gray-700: #404040;
  --gray-900: #171717;
}
```

这些是示例起点，可根据项目调整。不是 Apple 官方数值。

---

## 7. 工作流程

详见 [references/workflow.md](references/workflow.md)。

### 7.1 实现步骤
1. **调研**：了解项目栈、设计系统、界面类型
2. **设计意图**：写 2-3 句概括目标
3. **结构先行**：HTML 语义结构
4. **样式实现**：从 tokens 开始，复用变量
5. **状态补全**：交互状态、响应式、暗色
6. **无障碍检查**：键盘、对比度、屏幕阅读器
7. **视觉审查**：对照原则逐项检查

### 7.2 验收清单
- [ ] 主要断点截图（移动/平板/桌面）
- [ ] 键盘 Tab 遍历，焦点可见
- [ ] 文字对比度 ≥ 4.5:1
- [ ] `prefers-reduced-motion` 测试
- [ ] 浏览器控制台无错误
- [ ] 项目现有测试通过

**诚实说明未测到什么**：如无真机测试、无屏幕阅读器测试、无性能分析等。

---

## 8. Codex 调用示例

### 新建页面
> 使用 apple-inspired skill，为这个 React 项目创建一个产品详情页。先阅读现有代码结构和样式惯例。

### 改进现有组件
> 使用 apple-inspired skill，审查并改进这个按钮组件的状态覆盖和无障碍。

### 审查页面
> 使用 apple-inspired skill，审查这个页面的视觉层级、间距一致性和响应式表现。

### 产品摄影提示词
> 使用 apple-inspired skill，为这张产品图写 Flux 精修提示词，保留结构和配色。

### 区分安装与使用
- **安装**：将 SKILL.md 和 references/ 目录添加到 AI 助手的 skills 目录
- **使用**：在对话中说"使用 apple-inspired skill"触发

---

## 9. 产品摄影（简版）

完整指南见 [references/photography.md](references/photography.md)。

### 核心原则
- 保留产品真实：轮廓、比例、配色、材质、按键、接口
- 背景：干净无缝（暖白/浅灰/炭黑）
- 光线：大面积柔光、克制高光、自然阴影
- 不改结构、不加 logo、不虚构参数

### 中文提示词模板
```text
对上传的产品实拍照做高端极简棚拍精修，不重新设计产品。严格保留外形、比例、原配色、真实材质、按键数量与布局、接口、屏幕状态和机械结构。
背景改为干净无缝的暖白或浅灰，去除污渍与杂乱纹理。采用大面积柔和棚拍光、克制高光和自然接触阴影。曝光自然，不锐化过度。
不改结构，不增零件或道具，不加标志、界面或文字。
```

---

## 附录：不做什么

- ❌ 不假定 SF Pro/SF Mono 可自由下载分发
- ❌ 不伪称数值是 Apple 官方规范
- ❌ 不擅自重写项目架构或换框架
- ❌ 不在无图像工具时假装已生成图片
- ❌ 不承诺像素级还原或无损精修
- ❌ 不虚构业务数据、用户评价或合作背书
- ❌ 不模仿 Apple 官网或暗示官方关系
