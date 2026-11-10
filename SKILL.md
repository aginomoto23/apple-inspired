---
name: apple-inspired
description: Apply restrained Apple-inspired aesthetics to product photography, image-editing prompts, posters, and explicitly requested web visuals. Use when the user asks for a clean, premium, minimalist product presentation with intentional whitespace, soft lighting, truthful materials, and clear hierarchy.
---

# Apple-inspired 视觉美学

出品与维护：AIGC-DAO

这是受产品摄影启发的视觉方法，不是 Apple 官方规范。核心：改呈现，不乱改产品。

## 1. 确定任务
先看用户原图、参考图及项目约束。区分提示词、实际改图、海报、网页实现。只要提示词时不要擅自生成图片。已有信息直接使用；只问会实质改变结果的必要问题。

默认保留产品轮廓、比例、配色、材质类型、按键数量及位置、接口、屏幕状态与机械结构。有人物时保留面部特征、肤色、姿态和接触点。不从参考风格图搬入其具体产品、界面或道具。

## 2. 六项视觉原则
- 主体明确：一个主角，一个视觉重点，去除无关道具。
- 留白有用途：给主体呼吸空间；海报提前预留文字区，不套固定留白比例。
- 光线塑形：大面积柔和棚拍光、克制高光、自然接触阴影，清楚但不生硬。
- 材质可信：玻璃保留厚度与反射，金属保留金属感，塑料保持塑料；不把一切换成银色金属。
- 色彩克制：暖白、浅灰或有层次的炭黑背景，以原产品色为焦点；避免灰蒙、霓虹、过曝和过度饱和。
- 排版清晰：一句主张、简短辅助说明、明确层级。使用真实文案，不编造参数、评价或背书；中文可单独排版。

## 3. 方向选择
直接修图默认浅色保真棚拍，不反复追问。探索新品牌方向时，可先提供"暖白生活感 / 冷白科技感 / 炭黑精密感"两三个方案。明确要求直接做时自行选择。功能展示的手部互动应保留，确保自然解剖与准确触碰。

## 4. 提示词组织
按"任务—保留项—背景与光线—材质色彩—构图—禁止改动"写，不只说 Apple style。替换模板中的选项；不要把占位符直接发给图像工具。

### 英文保真改图模板
```text
Edit the supplied product photograph, not the product design. Preserve the subject identity, silhouette, proportions, original colors, material types, component count, button layout, ports, screen state, and mechanical structure. If a person is present, preserve their identity, pose, natural skin, hand anatomy and precise contact with the product.
Create a refined minimalist editorial product photograph: a seamless warm-white backdrop, intentional negative space, large soft studio lighting, controlled highlights, subtle realistic contact shadows, clean edges and accurate material textures. Keep the original accent color and camera angle. Remove background stains, dust and distracting clutter without removing genuine product details. Natural exposure and neutral color balance; crisp but not oversharpened.
Do not redesign the product, invent components, replace plastic with metal, add props, logos, interface graphics, or text. Avoid neon glows, excessive bloom, blown highlights, plastic-looking skin and synthetic CGI finish.
```

### 中文保真改图模板
```text
对上传的产品实拍照做高端极简棚拍精修，不重新设计产品。严格保留外形、比例、原配色、真实材质、按键数量与布局、接口、屏幕状态和机械结构；有人物时保留面部特征、自然肤色、姿态和准确触碰位置。
背景改为干净无缝的暖白或浅灰，去除污渍与杂乱纹理。采用大面积柔和棚拍光、克制高光和自然接触阴影；玻璃、金属、塑料各自保留真实质感。保持原拍摄角度，留白有呼吸感，以原产品色作为焦点。曝光自然，不锐化过度。
不改结构，不增零件或道具，不把塑料改成金属，不加标志、界面或文字；不要霓虹、过度光晕、过曝和假皮肤。
```

深色版本：改为有层次的炭黑背景，用柔和侧光分离轮廓，保留黑色部件细节，不加入赛博霓虹。
海报版本：按用户要求加入画幅、主视觉位置、文字留白、真实文案和行动信息；不擅加 Apple 标志，不暗示官方合作。

## 5. Codex 执行适配
- 先检查当前环境实际提供的图像编辑或生成工具，不假定存在某个专有工具、API 或账号。
- 改图须提供原图作为输入；多参考图须明确原图负责主体真实性、风格图只负责表现手法。
- 不假定支持负面提示、强度、种子、参考权重；没有工具依据时不报通用数值。
- 仅写提示词不需要图像工具。要求实际改图但没有可用工具时，说明未生成图片，提供定制提示词；不要把文字当成成图交付。
- 用户明确要求网页或代码时，遵守项目约束，用上述原则实现，不擅自重写产品信息。产品图可按用户意图保留或采用可用素材，不伪造素材来源。
- 不把 AI 重绘称为无损精修，不承诺像素级一致。

## 6. 交付检查
对照原图检查轮廓、部件数量、按键、接口、方向、颜色、透明壳和接缝；检查人脸、手指及接触位置。检查背景污点、过曝、材质和落地阴影。检查文字准确性、可读性、裁切。明显变形则修正或明确说明，不隐藏偏差。

## 7. 返回结果
提示词任务交付一段可直接复制的提示词。图像任务交付实际文件及一两句改动说明，指出重要偏差。代码任务按项目惯例交付并验证。不要在完成后连续追问。
