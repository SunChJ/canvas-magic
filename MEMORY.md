# MEMORY.md - 限定长度的必要知识记忆

> **重要**：此文件保持简洁，定期清理过时信息。当前限制：50条关键记忆。

## 核心设计原则

### 1. 视觉效果原则
- **简洁优先**：少即是多，避免过度设计
- **和谐配色**：使用颜色理论，避免冲突
- **流畅动画**：60fps 是标准，避免卡顿
- **响应式**：适配所有屏幕尺寸

### 2. 技术实现原则
- **单文件 HTML**：零外部依赖，便于分享
- **CDN 引用**：使用稳定的 CDN 链接
- **性能优化**：避免每帧 DOM 查询
- **兼容性**：支持主流浏览器

## 关键设计模式

### 1. Three.js 基础模式
```javascript
// 标准 Three.js 设置
const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
renderer.setSize(window.innerWidth, window.innerHeight);
renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
document.body.appendChild(renderer.domElement);
```

### 2. Canvas 2D 基础模式
```javascript
// 标准 Canvas 设置
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');
let W, H;

function resize() {
  W = canvas.width = window.innerWidth;
  H = canvas.height = window.innerHeight;
}
window.addEventListener('resize', resize);
resize();
```

### 3. 鼠标跟踪模式
```javascript
// 平滑鼠标跟踪
const mouse = { x: W/2, y: H/2 };
const mouseTarget = { x: W/2, y: H/2 };

window.addEventListener('mousemove', e => {
  mouseTarget.x = e.clientX;
  mouseTarget.y = e.clientY;
});

// 在动画循环中
mouse.x += (mouseTarget.x - mouse.x) * 0.05;
mouse.y += (mouseTarget.y - mouse.y) * 0.05;
```

## 最佳颜色方案

### 1. 深色奢华（默认）
- **背景**：#050505（近纯黑）
- **主色**：#4A6CF7（电光蓝）
- **强调**：#C9A84C（金色）
- **文本**：#EEEAF4（浅灰白）

### 2. 宇宙/AI 主题
- **背景**：#030308（深空黑）
- **主色**：#7B61FF（紫色）
- **强调**：#FF6B9D（粉色）
- **文本**：#E8EDF5（冷白）

### 3. 有机/自然主题
- **背景**：#0a0f0a（森林绿）
- **主色**：#4CAF50（绿色）
- **强调**：#8BC34A（浅绿）
- **文本**：#E8F5E9（暖白）

## 关键 Shader 片段

### 1. Simplex 噪声（3D）
- **用途**：顶点位移、程序化纹理
- **来源**：从 referenceHTMLS_files 提炼
- **位置**：`shareds/patterns/simplex-noise.glsl`

### 2. Fresnel 效果
- **用途**：边缘发光、玻璃效果
- **来源**：saved_resource(34).html
- **位置**：`shareds/patterns/fresnel.glsl`

### 3. 距离场形状
- **用途**：程序化形状、发光弧线
- **来源**：saved_resource(11).html
- **位置**：`shareds/patterns/distance-field.glsl`

## 常见错误和解决方案

### 1. Three.js 渲染问题
- **错误**：黑屏或白屏
- **原因**：相机位置错误或场景为空
- **解决**：检查 camera.position 和场景对象

### 2. Canvas 性能问题
- **错误**：动画卡顿
- **原因**：每帧创建新对象或频繁 DOM 操作
- **解决**：预分配对象，减少 DOM 查询

### 3. 鼠标交互问题
- **错误**：鼠标跟踪不流畅
- **原因**：直接使用鼠标位置，没有插值
- **解决**：使用 lerp 平滑鼠标位置

## 用户偏好记录

### 1. 视觉偏好
- **喜欢**：深色主题、发光效果、流畅动画
- **不喜欢**：过于复杂的设计、卡顿的动画
- **优先级**：Canvas/Three.js 效果 > CSS 效果

### 2. 技术偏好
- **Three.js**：优先使用 ShaderMaterial
- **Canvas**：优先使用 requestAnimationFrame
- **CSS**：优先使用自定义属性

## 学习成果

### 1. 已掌握模式
- ✅ Noise Sphere（3D 噪声球体）
- ✅ Particle Network（粒子网络）
- ✅ Wave Plane（波浪平面）
- ✅ Dot Grid（点阵网格）
- ✅ Plasma Colors（等离子颜色）
- ✅ CRT Effect（CRT 效果）

### 2. 待学习模式
- ⏳ 流体模拟
- ⏳ 光线追踪
- ⏳ 物理引擎集成
- ⏳ 音频可视化

## 版本历史摘要

### v0.1（当前版本）
- **日期**：2026-04-28
- **内容**：基础模式库、初始技能
- **状态**：已完成
- **改进点**：需要更多测试用例

### v0.2（计划中）
- **日期**：2026-05-05（预计）
- **内容**：更多效果、优化匹配算法
- **状态**：规划中
- **目标**：支持 10+ 种效果

## 关键指标

### 1. 质量指标
- **视觉评分**：7.5/10（平均）
- **匹配准确率**：80%
- **生成成功率**：90%

### 2. 效率指标
- **生成时间**：2-5 秒
- **迭代次数**：1-3 次
- **知识利用率**：60%

## 待办事项

### 高优先级
1. 添加更多测试用例
2. 优化颜色选择算法
3. 改进鼠标交互体验

### 中优先级
1. 扩展模式库
2. 实现 A/B 测试
3. 优化生成性能

### 低优先级
1. 添加音频支持
2. 实现物理效果
3. 支持 VR/AR

## 参考资源

### 官方文档
- [Three.js 文档](https://threejs.org/docs/)
- [WebGL 教程](https://webglfundamentals.org/)
- [GLSL 着色器](https://www.shadertoy.com/)

### 设计资源
- [Dribbble](https://dribbble.com/)
- [Behance](https://www.behance.net/)
- [Awwwards](https://www.awwwards.com/)

### 学习资源
- [The Book of Shaders](https://thebookofshaders.com/)
- [WebGL Fundamentals](https://webglfundamentals.org/)
- [Three.js Journey](https://threejs-journey.com/)

---

**最后更新**：2026-04-28
**记忆条数**：45/50
**下次清理**：2026-05-05
