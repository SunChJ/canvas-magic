# Canvas Magic 测试用例

> 参考 Prompt Artisan 的测试用例格式，为每个效果类型定义明确的输入、期望输出和测试要点。

---

## 基础效果测试

### TC-001: 发光球体
```json
{
  "id": 1,
  "prompt": "一个会发光的紫色球体，跟随鼠标移动",
  "expected": "匹配 Noise Sphere 模式；使用 Three.js + ShaderMaterial；紫色系配色（#7B61FF 或类似）；鼠标跟踪平滑插值（lerp 0.05）；AdditiveBlending 实现发光；60fps 流畅动画",
  "tests": "模式匹配正确 + Three.js 初始化完整 + 鼠标交互 + 发光效果 + 性能达标"
}
```

### TC-002: 粒子网络
```json
{
  "id": 2,
  "prompt": "粒子之间有连线，鼠标靠近会吸引它们",
  "expected": "匹配 Particle Network 模式；Canvas 2D 实现；80-100 个粒子；距离阈值 140px 内连线；鼠标吸引半径 1.5 倍；线条透明度随距离衰减",
  "tests": "Canvas 2D 路径正确 + 粒子数量合理 + 连线逻辑 + 鼠标吸引 + 性能"
}
```

### TC-003: 波浪地形
```json
{
  "id": 3,
  "prompt": "一个像海浪一样起伏的 3D 地形",
  "expected": "匹配 Wave Plane 模式；PlaneGeometry(6,6,128,128) 旋转水平；顶点着色器位移；正弦波叠加产生波浪；透视相机 45 度俯视；可选 GridHelper 参考线",
  "tests": "几何体细分足够 + 着色器位移 + 相机角度 + 动画流畅"
}
```

### TC-004: 点阵网格
```json
{
  "id": 4,
  "prompt": "整齐排列的点，鼠标靠近会变大",
  "expected": "匹配 Dot Grid 模式；Canvas 2D 实现；间距 30px；鼠标影响半径 200px；点大小 1-6px 变化；白色背景黑色点（或反色）",
  "tests": "网格计算正确 + 鼠标距离影响 + 大小变化平滑 + 响应式"
}
```

### TC-005: 等离子颜色
```json
{
  "id": 5,
  "prompt": "像老电视彩色条纹一样的流动颜色",
  "expected": "匹配 Plasma Colors 模式；Canvas 2D + ImageData 像素操作；4x4 降采样优化；正弦函数叠加；HSL 颜色循环；鼠标影响中心偏移",
  "tests": "像素操作正确 + 颜色过渡平滑 + 性能可接受（降采样） + 鼠标交互"
}
```

---

## 风格化效果测试

### TC-006: 复古终端
```json
{
  "id": 6,
  "prompt": "一个绿色的黑客终端，有扫描线效果",
  "expected": "匹配 CRT Effect 模式；VT323 字体；#00ff41 终端绿；CSS 扫描线（repeating-linear-gradient）；径向渐变暗角；光标闪烁动画；可选打字机效果",
  "tests": "字体加载 + CRT 效果 CSS 完整 + 颜色正确 + 动画节奏"
}
```

### TC-007: 赛博故障
```json
{
  "id": 7,
  "prompt": "文字有故障效果，像黑客帝国那样",
  "expected": "匹配 Glitch Text 模式；随机字符替换动画；30ms 间隔；字符逐个解析；等宽字体；霓虹色（青、粉、黄）；可选闪烁背景",
  "tests": "字符替换逻辑 + 时序正确 + 视觉冲击力 + 性能"
}
```

### TC-008: 深空宇宙
```json
{
  "id": 8,
  "prompt": "深色背景，有星空和一个发光的星云",
  "expected": "匹配 Dark Luxury + 组合模式；Three.js 星场（900 粒子）+ 全屏 Shader 背景；近纯黑背景 #030308；星云用噪声函数；AdditiveBlending 叠加",
  "tests": "星场生成 + Shader 噪声 + 颜色搭配 + 层次感"
}
```

---

## 交互测试

### TC-009: 鼠标跟踪球体
```json
{
  "id": 9,
  "prompt": "一个球体跟着鼠标转，但很丝滑不是直接跟",
  "expected": "鼠标跟踪 + 平滑插值；targetMouse 更新；每帧 lerp 0.05；相机或物体跟随；响应式 resize 处理",
  "tests": "插值算法正确 + 跟踪延迟合理 + resize 不出错"
}
```

### TC-010: 点击触发效果
```json
{
  "id": 10,
  "prompt": "点击屏幕会喷出一堆粒子",
  "expected": "Canvas 2D 粒子系统；点击事件绑定；粒子从点击位置发射；速度随机；生命周期衰减；可选拖尾效果",
  "tests": "事件绑定 + 粒子发射逻辑 + 生命周期 + 性能（大量粒子）"
}
```

### TC-011: 滚动视差
```json
{
  "id": 11,
  "prompt": "页面滚动时背景有视差效果",
  "expected": "Three.js + HTML 混合布局；滚动事件监听；相机 Z 轴或物体 Y 轴偏移；视差系数 0.3-0.5；requestAnimationFrame 节流",
  "tests": "滚动事件处理 + 视差计算 + 性能优化 + 布局不冲突"
}
```

---

## 颜色方案测试

### TC-012: 暖色调
```json
{
  "id": 12,
  "prompt": "温暖的橙色渐变背景，有流动感",
  "expected": "颜色方案匹配暖色系；橙色 #FF6B35 或类似；渐变流动用噪声；避免 AI slop（不用紫渐变）；CSS 或 Shader 实现",
  "tests": "颜色选择准确 + 流动效果 + 反 AI slop 检查"
}
```

### TC-013: 冷色调
```json
{
  "id": 13,
  "prompt": "冷酷的科技感，蓝色为主",
  "expected": "颜色方案匹配冷色系；深蓝背景 #0a1628；亮蓝强调 #00BCD4；科技感标签；等宽字体",
  "tests": "颜色搭配 + 科技感元素 + 字体选择"
}
```

### TC-014: 单色极简
```json
{
  "id": 14,
  "prompt": "黑白极简风格，只有点和线",
  "expected": "Monochrome 配色；纯黑 #000 或纯白 #fff；几何图形（点、线、圆）；大量留白；瑞士设计风格",
  "tests": "配色极简 + 几何元素 + 留白比例 + 整体协调"
}
```

---

## 性能测试

### TC-015: 低端设备
```json
{
  "id": 15,
  "prompt": "做一个粒子效果，但要保证流畅",
  "expected": "自动降级策略；粒子数量 ≤ 100；Canvas 2D 优先（非 Three.js）；requestAnimationFrame；避免每帧创建对象",
  "tests": "粒子数量控制 + 帧率监测 + 内存稳定"
}
```

### TC-016: 大屏幕适配
```json
{
  "id": 16,
  "prompt": "全屏效果，在 4K 显示器上也要流畅",
  "expected": "resize 事件处理；renderer.setPixelRatio(Math.min(devicePixelRatio, 2))；Canvas 尺寸更新；相机 aspect 更新",
  "tests": "resize 处理完整 + 像素比限制 + 4K 性能"
}
```

---

## 组合效果测试

### TC-017: 球体 + 粒子
```json
{
  "id": 17,
  "prompt": "一个发光球体，周围有旋转的粒子环",
  "expected": "组合模式：Noise Sphere + Orbital Particles；球体用 ShaderMaterial；粒子用 Points + BufferGeometry；统一动画循环；AdditiveBlending 叠加",
  "tests": "两种效果组合 + 性能可接受 + 视觉协调"
}
```

### TC-018: 背景 + 前景
```json
{
  "id": 18,
  "prompt": "流动的背景，前面有固定的 UI 文字",
  "expected": "Three.js 全屏背景 + HTML overlay；position: fixed 分层；pointer-events: none 允许穿透；z-index 管理",
  "tests": "分层正确 + 事件穿透 + 视觉层次"
}
```

---

## 边界情况测试

### TC-019: 模糊描述
```json
{
  "id": 19,
  "prompt": "做一个好看的效果",
  "expected": "默认匹配 Particle Network（万能模式）；深色奢华配色；鼠标交互；不报错，有合理默认值",
  "tests": "默认模式选择 + 不崩溃 + 效果可接受"
}
```

### TC-020: 冲突描述
```json
{
  "id": 20,
  "prompt": "极简风格但要有复杂动画",
  "expected": "优先极简视觉（黑白、留白）；动画可以复杂但视觉简洁；避免过度设计；问用户澄清或给出平衡方案",
  "tests": "冲突处理 + 视觉简洁 + 动画丰富度平衡"
}
```

---

## 反 AI Slop 检查清单

每个测试用例都应检查：

- [ ] **不用紫渐变**：除非用户明确要求紫色
- [ ] **不用 Inter 字体**：除非用户明确要求
- [ ] **不用 emoji 装饰**：保持专业
- [ ] **不用默认圆角**：根据风格选择
- [ ] **不用默认阴影**：避免通用 box-shadow
- [ ] **颜色有理由**：每个颜色选择有依据
- [ ] **字体有搭配**：标题 + 正文 + 等宽有区分
- [ ] **动画有意义**：不是为了动而动

---

## 测试执行方法

### 手动测试
1. 在 opencode 中加载 canvas-magic 技能
2. 输入 prompt
3. 检查生成的 HTML 是否符合 expected
4. 对照 tests 逐项验证

### 自动测试（未来）
```bash
# 运行测试用例
npm test -- --filter TC-001

# 运行所有测试
npm test

# 生成测试报告
npm test -- --reporter html
```

---

**最后更新**：2026-04-28
**测试用例数**：20
**覆盖范围**：基础效果、风格化、交互、颜色、性能、组合、边界
