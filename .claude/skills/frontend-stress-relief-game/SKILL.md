---
name: frontend-stress-relief-game
description: 生成视觉精美、交互流畅的前端解压小游戏。当用户提到"解压游戏"、"小游戏"、"捏泡泡"、"消除游戏"、"减压"、"放松小游戏"或需要创建可玩的HTML页面时使用。输出单个自包含的HTML文件，无需后端，打开即玩。
allowed-tools: Read, Write, Edit
---

# 前端解压小游戏生成器

本 Skill 用于创建视觉精美、交互流畅的 HTML5 解压小游戏。所有游戏均为单文件 HTML，包含完整样式和逻辑，可直接在浏览器中运行。

## 设计原则

### 视觉美学
- **色彩**：使用柔和渐变色或高饱和度治愈系配色，避免刺眼的纯色
- **质感**：毛玻璃效果（backdrop-filter）、柔和阴影（box-shadow）、圆角设计（border-radius）
- **动画**：平滑的过渡动画（transition）、弹性缓动（cubic-bezier）
- **字体**：系统默认无衬线字体，配合适当的字重和间距

### 解压体验
- **即时反馈**：点击/触摸时立即产生视觉变化（缩放、粒子、涟漪）
- **声音反馈**：使用 Web Audio API 生成简单的振荡器音效（可选）
- **物理感**：动画应符合直觉，有"弹力"或"惯性"感
- **满足感**：完成时的庆祝动画、分数累计、连击提示

### 技术要求
- **单文件**：所有 CSS 和 JavaScript 内联
- **响应式**：适配移动端和桌面端，使用 viewport 和媒体查询
- **性能**：使用 requestAnimationFrame 处理动画
- **触摸友好**：touch-action 优化，按钮尺寸不小于 44px

## 游戏类型参考

根据用户需求选择或组合以下类型：

1. **捏泡泡纸** - 网格气泡，点击爆破，全部爆破后重置
2. **弹球消除** - 底部挡板接球，撞击砖块消除
3. **滑动拼图** - 图片碎片，滑动交换位置
4. **无限点击器** - 点击产生粒子特效和分数累计
5. **绘画涂鸦** - 鼠标/触摸绘制彩色线条或粒子
6. **切水果/割绳子风格** - 滑动切割或拖拽割断
7. **减压沙盘** - 粒子掉落堆积，可扰动
8. **音乐节奏点击** - 下落音符点击得分

## 输出规范

### 文件结构
生成的 HTML 文件应按以下顺序组织：

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>游戏名称 | 解压小游戏</title>
    <style>
        /* 全局样式 + 游戏特定样式 */
    </style>
</head>
<body>
    <!-- 游戏容器 -->
    <div class="game-container">
        <!-- 头部：分数、重置按钮 -->
        <!-- 游戏区域：canvas 或 DOM 元素 -->
        <!-- 底部：提示文字 -->
    </div>
    <!-- 粒子效果画布（如需要） -->
    <canvas id="particle-canvas"></canvas>
    <script>
        (function(){
            "use strict";
            // 游戏逻辑
        })();
    </script>
</body>
</html>


## 必须包含的元素
- 计分显示：分数/进度，字体醒目
- 重置/新游戏按钮：重新开始游戏
- 游戏标题/说明：简洁的游戏名称和操作提示
- 完成反馈：胜利/完成时的庆祝效果

## 可选增强元素
- 连击显示：连续操作计数
- 等级/难度：随分数提升的难度变化
- 最高分记录：localStorage 持久化
- 音效开关：静音按钮

## 代码质量要求
### CSS 规范
- 使用 CSS 变量定义主题色
- 动画使用 transform 和 opacity（避免触发重排）
- 移动端 hover 效果需配合 @media (hover: hover)

## JavaScript 规范
- 使用 IIFE 或模块模式避免全局污染
- 事件监听器需考虑触摸事件（touchstart、touchmove、touchend）
- 粒子系统需限制最大粒子数，避免性能问题
- 音效初始化需在用户首次交互时进行（浏览器自动播放策略）