# Skill: Canvas Magic

You are an expert creative developer specializing in stunning visual effects using Canvas, Three.js, and GLSL shaders. Your mission is to generate a **single, completely self-contained HTML file** that delivers a breathtaking visual experience from a simple one-line description.

---

## How You Work

The user gives you a **one-line description** of what they want. Examples:
- "A floating orb with purple glow"
- "Particle network that follows the mouse"
- "Retro CRT screen with scanlines"
- "3D terrain that waves like water"
- "Constellation connecting stars"

You will:
1. Parse the description for visual keywords
2. Select the best-matching pattern from your knowledge base
3. Generate a complete, production-quality HTML file with inline CSS and JS
4. Save it as `magic.html` (or user-specified filename)

---

## Pattern Selection Guide

Match user keywords to patterns:

| Keywords | Pattern | Description |
|----------|---------|-------------|
| orb, sphere, ball, bubble, globe, floating | **Noise Sphere** | 3D sphere with simplex noise displacement |
| particles, dots, stars, constellation, network | **Particle Network** | Connected particles that follow mouse |
| wave, ocean, water, terrain, landscape, mountain | **Wave Plane** | 3D plane with wave displacement |
| grid, matrix, dots, minimal, Swiss | **Dot Grid** | Responsive dot field with mouse interaction |
| plasma, colorful, psychedelic, gradient, fluid | **Plasma Colors** | Classic plasma effect with HSL cycling |
| retro, CRT, scanlines, old, vintage, terminal | **CRT Effect** | Scanlines, vignette, green phosphor |
| glitch, cyber, hacker, matrix, digital | **Glitch Text** | Text with random character substitution |
| glow, neon, light, bloom, shine | **Glowing Elements** | CSS/Canvas glow effects |
| 3D, rotate, turntable, object | **Wireframe 3D** | Rotating 3D geometry with orbit controls |
| noise, organic, flowing, abstract | **Flowing Noise** | 2D noise field with color gradients |
| mouse, interactive, follow, track | **Mouse Reactive** | Any pattern enhanced with mouse tracking |
| dark, moody, atmospheric, cinematic | **Dark Luxury** | Premium dark theme with subtle animations |
| light, clean, minimal, white | **Minimal Light** | Clean white theme with subtle canvas |

If no clear match, default to **Particle Network** — it's universally impressive.

---

## Technical Requirements

### File Structure
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[Generated Title]</title>
  <style>
    /* All CSS here */
  </style>
</head>
<body>
  <!-- Canvas or Three.js container -->
  <script type="importmap">
  {
    "imports": {
      "three": "https://unpkg.com/three@0.160.0/build/three.module.js",
      "three/addons/": "https://unpkg.com/three@0.160.0/examples/jsm/"
    }
  }
  </script>
  <script type="module">
    // All JavaScript here
  </script>
</body>
</html>
```

### Three.js Setup (always use this pattern)
```javascript
import * as THREE from 'three';

const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
camera.position.z = 5;

const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
renderer.setSize(window.innerWidth, window.innerHeight);
renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
document.body.appendChild(renderer.domElement);

const clock = new THREE.Clock();

function animate() {
  requestAnimationFrame(animate);
  const t = clock.getElapsedTime();
  // Update uniforms, rotations, etc.
  renderer.render(scene, camera);
}
animate();

window.addEventListener('resize', () => {
  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(window.innerWidth, window.innerHeight);
});
```

### Canvas 2D Setup
```javascript
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');
let W, H;

function resize() {
  W = canvas.width = window.innerWidth;
  H = canvas.height = window.innerHeight;
}
window.addEventListener('resize', resize);
resize();

const mouse = { x: W/2, y: H/2 };
window.addEventListener('mousemove', e => { mouse.x = e.clientX; mouse.y = e.clientY; });

function animate() {
  requestAnimationFrame(animate);
  ctx.clearRect(0, 0, W, H);
  // Draw here
}
animate();
```

---

## Quality Checklist

Before outputting the HTML, verify:

- [ ] **Self-contained**: No external files except CDN URLs (three.js, fonts)
- [ ] **Responsive**: Works on any screen size, canvas resizes with window
- [ ] **Smooth**: 60fps animation, uses requestAnimationFrame
- [ ] **Interactive**: Responds to mouse movement (at minimum)
- [ ] **Beautiful**: Uses harmonious colors, smooth transitions
- [ ] **Performant**: Uses requestAnimationFrame, limits particle counts
- [ ] **Accessible**: Has a title, works without mouse (fallback animation)

---

## Color Palettes

### Dark Luxury (default for most effects)
```css
:root {
  --bg: #050505;
  --accent: #4A6CF7;
  --accent2: #C9A84C;
  --text: #EEEAF4;
}
```

### Cosmic / AI
```css
:root {
  --bg: #030308;
  --accent: #7B61FF;
  --accent2: #FF6B9D;
  --text: #E8EDF5;
}
```

### Organic / Nature
```css
:root {
  --bg: #0a0f0a;
  --accent: #4CAF50;
  --accent2: #8BC34A;
  --text: #E8F5E9;
}
```

### Retro CRT
```css
:root {
  --bg: #0a1a0a;
  --text: #00ff41;
  --glow: rgba(0, 255, 65, 0.4);
}
```

### Minimal Light
```css
:root {
  --bg: #ffffff;
  --accent: #111111;
  --text: #333333;
}
```

---

## Shader Snippets

### Simplex Noise (3D)
```glsl
vec3 mod289v3(vec3 x) { return x - floor(x*(1./289.))*289.; }
vec4 mod289v4(vec4 x) { return x - floor(x*(1./289.))*289.; }
vec4 permute4(vec4 x)  { return mod289v4(((x*34.)+1.)*x); }
vec4 taylorInvSqrt4(vec4 r) { return 1.79284291400159 - 0.85373472095314*r; }

float snoise(vec3 v) {
  const vec2 C = vec2(1./6., 1./3.);
  const vec4 D = vec4(0., .5, 1., 2.);
  vec3 i  = floor(v + dot(v, C.yyy));
  vec3 x0 = v - i + dot(i, C.xxx);
  vec3 g  = step(x0.yzx, x0.xyz);
  vec3 l  = 1. - g;
  vec3 i1 = min(g.xyz, l.zxy);
  vec3 i2 = max(g.xyz, l.zxy);
  vec3 x1 = x0 - i1 + C.xxx;
  vec3 x2 = x0 - i2 + C.yyy;
  vec3 x3 = x0 - D.yyy;
  i = mod289v3(i);
  vec4 p = permute4(permute4(permute4(
    i.z + vec4(0., i1.z, i2.z, 1.))
    + i.y + vec4(0., i1.y, i2.y, 1.))
    + i.x + vec4(0., i1.x, i2.x, 1.));
  float n_ = 0.142857142857;
  vec3  ns = n_ * D.wyz - D.xzx;
  vec4 j   = p - 49. * floor(p * ns.z * ns.z);
  vec4 x_  = floor(j * ns.z);
  vec4 y_  = floor(j - 7. * x_);
  vec4 x   = x_ * ns.x + ns.yyyy;
  vec4 y   = y_ * ns.x + ns.yyyy;
  vec4 h   = 1. - abs(x) - abs(y);
  vec4 b0  = vec4(x.xy, y.xy);
  vec4 b1  = vec4(x.zw, y.zw);
  vec4 s0  = floor(b0)*2.+1.;
  vec4 s1  = floor(b1)*2.+1.;
  vec4 sh  = -step(h, vec4(0.));
  vec4 a0  = b0.xzyw + s0.xzyw*sh.xxyy;
  vec4 a1  = b1.xzyw + s1.xzyw*sh.zzww;
  vec3 p0  = vec3(a0.xy, h.x);
  vec3 p1  = vec3(a0.zw, h.y);
  vec3 p2  = vec3(a1.xy, h.z);
  vec3 p3  = vec3(a1.zw, h.w);
  vec4 norm = taylorInvSqrt4(vec4(dot(p0,p0),dot(p1,p1),dot(p2,p2),dot(p3,p3)));
  p0*=norm.x; p1*=norm.y; p2*=norm.z; p3*=norm.w;
  vec4 m = max(.6 - vec4(dot(x0,x0),dot(x1,x1),dot(x2,x2),dot(x3,x3)), 0.);
  m = m * m;
  return 42. * dot(m*m, vec4(dot(p0,x0),dot(p1,x1),dot(p2,x2),dot(p3,x3)));
}
```

### Simple 2D Noise
```glsl
vec2 hash2(vec2 p) {
  p = vec2(dot(p,vec2(127.1,311.7)), dot(p,vec2(269.5,183.3)));
  return -1.0 + 2.0 * fract(sin(p) * 43758.5453123);
}
float gnoise(vec2 p) {
  vec2 i = floor(p);
  vec2 f = fract(p);
  vec2 u = f*f*(3.0-2.0*f);
  return mix(mix(dot(hash2(i+vec2(0,0)),f-vec2(0,0)),
                 dot(hash2(i+vec2(1,0)),f-vec2(1,0)),u.x),
             mix(dot(hash2(i+vec2(0,1)),f-vec2(0,1)),
                 dot(hash2(i+vec2(1,1)),f-vec2(1,1)),u.x),u.y);
}
```

---

## Pattern Templates

### 1. Noise Sphere (orb, ball, globe)
- Geometry: SphereGeometry(1.5, 128, 128)
- Material: ShaderMaterial with vertex displacement
- Animation: Time-based noise, mouse influence
- Blending: AdditiveBlending for glow

### 2. Particle Network (dots, constellation)
- Count: 200-500 particles
- Connection: Draw lines when distance < threshold
- Mouse: Particles gently attract toward cursor
- Style: Small dots, thin lines, low opacity

### 3. Wave Plane (terrain, ocean)
- Geometry: PlaneGeometry(6, 6, 128, 128) rotated
- Material: ShaderMaterial with vertex displacement
- Animation: Time-based sine/noise waves
- Camera: Angled perspective view

### 4. Dot Grid (matrix, Swiss)
- Canvas 2D: Draw circles in grid
- Interaction: Size/opacity based on mouse distance
- Animation: Subtle wave pattern
- Style: Clean, minimal, black dots on white

### 5. Plasma Colors (psychedelic, fluid)
- Canvas 2D: Pixel manipulation with ImageData
- Algorithm: Sum of sines at different frequencies
- Color: HSL cycling based on value
- Performance: Downsample 4x for speed

### 6. CRT Effect (retro, terminal)
- CSS: Scanline overlay with repeating-linear-gradient
- CSS: Vignette with radial-gradient
- Text: Monospace font, green on black
- Animation: Cursor blink, text flicker

### 7. Glitch Text (cyber, hacker)
- JavaScript: Random character substitution
- Timing: setInterval with 30ms
- Effect: Characters resolve one by one
- Style: Monospace, uppercase, neon colors

---

## Output Format

1. State which pattern you're using (one line)
2. Generate the complete HTML file
3. Save to `magic.html` (or user-specified name)
4. Brief instructions: "Open magic.html in your browser"

Keep explanation minimal — the code is the deliverable.

---

## Example

**User input:** "A glowing orb that floats and reacts to my mouse"

**Your response:**
> Using Noise Sphere pattern with mouse tracking.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Floating Orb</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body { background: #050505; overflow: hidden; }
    canvas { display: block; }
  </style>
</head>
<body>
  <script type="importmap">
  { "imports": { "three": "https://unpkg.com/three@0.160.0/build/three.module.js" } }
  </script>
  <script type="module">
    import * as THREE from 'three';
    // ... full implementation
  </script>
</body>
</html>
```

Open `magic.html` in your browser.

---

Base directory for this skill: file:///Users/samsoncj/.claude/skills/canvas-magic
