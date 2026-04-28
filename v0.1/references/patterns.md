# Patterns Reference — Extracted from High-Quality HTML Examples

These patterns are extracted from real-world, production-quality HTML files. Each pattern represents a proven visual technique that delivers stunning results.

---

## 1. Noise Sphere (Three.js + Shader)

**Source:** saved_resource(34).html, saved_resource(25).html
**Best for:** AI products, cosmic themes, luxury hero sections
**Keywords:** orb, sphere, ball, floating, glowing, 3D

### Core Implementation
```javascript
const geometry = new THREE.IcosahedronGeometry(1.8, 64);
const material = new THREE.ShaderMaterial({
  uniforms: {
    uTime: { value: 0 },
    uMouse: { value: new THREE.Vector2(0, 0) },
    uColor1: { value: new THREE.Color('#FFB6C1') },
    uColor2: { value: new THREE.Color('#7B2CBF') },
    uColor3: { value: new THREE.Color('#E63946') }
  },
  vertexShader: `
    uniform float uTime;
    uniform vec2 uMouse;
    varying float vElevation;
    varying vec3 vNormal;
    
    // Simplex noise function here
    
    void main() {
      vNormal = normal;
      float t = uTime * 0.35;
      float n = snoise(position * 0.7 + vec3(t*0.28, t*0.18, t*0.22))
              + snoise(position * 1.4 + vec3(-t*0.18, t*0.32, -t*0.14)) * 0.5;
      float elevation = n * 0.14;
      float m = dot(normalize(position), vec3(uMouse.x, uMouse.y, 0.6));
      elevation += m * 0.04;
      vElevation = elevation;
      gl_Position = projectionMatrix * modelViewMatrix * vec4(position + normal * elevation, 1.0);
    }
  `,
  fragmentShader: `
    uniform float uTime;
    uniform vec3 uColor1;
    uniform vec3 uColor2;
    uniform vec3 uColor3;
    varying float vElevation;
    varying vec3 vNormal;
    
    void main() {
      float fresnel = pow(1.0 - max(dot(normalize(vNormal), vec3(0.,0.,1.)), 0.), 2.2);
      float t = clamp(vElevation * 5.0 + 0.4, 0., 1.);
      vec3 col = mix(uColor1, uColor2, t);
      col = mix(col, uColor3, clamp(vElevation * 8.0, 0., 0.25));
      col += uColor1 * fresnel * 0.7;
      float alpha = (0.55 + fresnel * 0.45) * (sin(uTime * 0.75) * 0.08 + 0.88);
      gl_FragColor = vec4(col, alpha);
    }
  `,
  transparent: true,
  blending: THREE.AdditiveBlending,
  depthWrite: false
});
```

### Key Details
- Use `IcosahedronGeometry` or `SphereGeometry` with high segments (64+)
- Additive blending for glow effect
- Mouse influence via dot product
- Fresnel for edge glow

---

## 2. Particle Network (Canvas 2D)

**Source:** saved_resource(14).html, saved_resource(2).html
**Best for:** Data visualization, AI themes, interactive backgrounds
**Keywords:** particles, dots, network, constellation, connected

### Core Implementation
```javascript
class Particle {
  constructor() {
    this.x = Math.random() * W;
    this.y = Math.random() * H;
    this.vx = (Math.random() - 0.5) * 0.4;
    this.vy = (Math.random() - 0.5) * 0.4;
    this.r = 2 + Math.random() * 2;
  }
  update() {
    this.x += this.vx;
    this.y += this.vy;
    if (this.x < 0 || this.x > W) this.vx *= -1;
    if (this.y < 0 || this.y > H) this.vy *= -1;
  }
}

const nodes = Array.from({ length: 80 }, () => new Particle());
const MAX_DIST = 140;

function animate() {
  requestAnimationFrame(animate);
  ctx.clearRect(0, 0, W, H);
  
  // Draw connections
  for (let i = 0; i < nodes.length; i++) {
    for (let j = i + 1; j < nodes.length; j++) {
      const dx = nodes[i].x - nodes[j].x;
      const dy = nodes[i].y - nodes[j].y;
      const d = Math.sqrt(dx*dx + dy*dy);
      if (d < MAX_DIST) {
        const alpha = (1 - d / MAX_DIST) * 0.4;
        ctx.beginPath();
        ctx.moveTo(nodes[i].x, nodes[i].y);
        ctx.lineTo(nodes[j].x, nodes[j].y);
        ctx.strokeStyle = `rgba(74, 136, 255, ${alpha})`;
        ctx.lineWidth = 0.5;
        ctx.stroke();
      }
    }
    // Mouse connection
    const mdx = nodes[i].x - mouse.x;
    const mdy = nodes[i].y - mouse.y;
    const md = Math.sqrt(mdx*mdx + mdy*mdy);
    if (md < MAX_DIST * 1.5) {
      const alpha = (1 - md / (MAX_DIST * 1.5)) * 0.7;
      ctx.beginPath();
      ctx.moveTo(nodes[i].x, nodes[i].y);
      ctx.lineTo(mouse.x, mouse.y);
      ctx.strokeStyle = `rgba(201, 168, 76, ${alpha})`;
      ctx.lineWidth = 0.8;
      ctx.stroke();
    }
  }
  
  // Draw nodes
  nodes.forEach(n => {
    n.update();
    ctx.beginPath();
    ctx.arc(n.x, n.y, n.r, 0, Math.PI * 2);
    ctx.fillStyle = 'rgba(74, 108, 247, 0.7)';
    ctx.fill();
  });
}
```

### Key Details
- 50-100 particles for good performance
- Distance-based line opacity
- Mouse interaction with larger radius
- Bounce off edges

---

## 3. Wave Plane (Three.js + Shader)

**Source:** saved_resource(1).html, saved_resource(17).html
**Best for:** Terrain visualization, ocean effects, data landscapes
**Keywords:** wave, terrain, ocean, mountain, landscape, water

### Core Implementation
```javascript
const geometry = new THREE.PlaneGeometry(3, 3, 128, 128);
geometry.rotateX(-Math.PI * 0.5);

const material = new THREE.ShaderMaterial({
  uniforms: {
    uTime: { value: 0 },
    uResolution: { value: new THREE.Vector2(container.offsetWidth, container.offsetHeight) }
  },
  vertexShader: `
    varying vec2 vUv;
    varying float vElevation;
    uniform float uTime;
    
    void main() {
      vUv = uv;
      vec4 modelPosition = modelMatrix * vec4(position, 1.0);
      
      float elevation = sin(modelPosition.x * 3.0 + uTime) * 
                        sin(modelPosition.z * 2.0 + uTime * 0.5) * 0.4;
      
      modelPosition.y += elevation;
      vElevation = elevation;
      
      vec4 viewPosition = viewMatrix * modelPosition;
      vec4 projectionPosition = projectionMatrix * viewPosition;
      gl_Position = projectionPosition;
    }
  `,
  fragmentShader: `
    varying vec2 vUv;
    varying float vElevation;
    uniform float uTime;
    
    void main() {
      float light = vElevation * 2.0 + 0.5;
      vec3 color = mix(vec3(0.96), vec3(0.1), light);
      gl_FragColor = vec4(color, 1.0);
    }
  `,
  transparent: true
});

const mesh = new THREE.Mesh(geometry, material);
scene.add(mesh);

// Add grid helper for reference
const grid = new THREE.GridHelper(3, 30, 0xdddddd, 0xeeeeee);
grid.position.y = -0.41;
scene.add(grid);
```

### Key Details
- High segment count (128x128) for smooth waves
- Rotate geometry to horizontal
- Use sine waves for displacement
- Optional grid helper for depth reference

---

## 4. Dot Grid (Canvas 2D)

**Source:** saved_resource(14).html
**Best for:** Swiss design, minimal backgrounds, interactive art
**Keywords:** grid, dots, matrix, minimal, Swiss, pattern

### Core Implementation
```javascript
function animate() {
  requestAnimationFrame(animate);
  ctx.fillStyle = '#ffffff';
  ctx.fillRect(0, 0, W, H);
  
  const t = Date.now() * 0.001;
  const COLS = Math.floor(W / 30);
  const ROWS = Math.floor(H / 30);
  const cellW = W / COLS;
  const cellH = H / ROWS;
  
  for (let col = 0; col < COLS; col++) {
    for (let row = 0; row < ROWS; row++) {
      const x = col * cellW + cellW / 2;
      const y = row * cellH + cellH / 2;
      
      // Distance from mouse
      const dx = x - mouse.x;
      const dy = y - mouse.y;
      const dist = Math.sqrt(dx*dx + dy*dy);
      const influence = Math.max(0, 1 - dist / 200);
      
      // Dot size pulses with wave + mouse
      const wave = Math.sin(col * 0.4 + row * 0.3 + t * 1.2) * 0.5 + 0.5;
      const r = 1 + wave * 3 + influence * 5;
      const alpha = 0.15 + influence * 0.6 + wave * 0.1;
      
      ctx.beginPath();
      ctx.arc(x, y, r, 0, Math.PI * 2);
      ctx.fillStyle = `rgba(0, 0, 0, ${alpha})`;
      ctx.fill();
    }
  }
}
```

### Key Details
- Grid spacing of 20-30px
- Mouse influence radius of 200px
- Wave animation for organic feel
- Black dots on white background (or inverse)

---

## 5. Plasma Colors (Canvas 2D)

**Source:** saved_resource(7).html, saved_resource(30).html
**Best for:** Psychedelic art, vibrant backgrounds, generative art
**Keywords:** plasma, colorful, gradient, fluid, psychedelic

### Core Implementation
```javascript
let imageData, data;

function init() {
  imageData = ctx.createImageData(W, H);
  data = imageData.data;
}
init();

function plasma(x, y, t) {
  return Math.sin(x * 0.02 + t)
       + Math.sin(y * 0.02 + t * 0.7)
       + Math.sin((x + y) * 0.015 + t * 1.3)
       + Math.sin(Math.sqrt(
           (x - W/2 - mouse.x*0.2)**2 +
           (y - H/2 - mouse.y*0.2)**2
         ) * 0.03);
}

function hsl2rgb(h, s, l) {
  h /= 360; s /= 100; l /= 100;
  let r, g, b;
  if (s === 0) { r = g = b = l; }
  else {
    const hue2rgb = (p, q, t) => {
      if (t < 0) t += 1; if (t > 1) t -= 1;
      if (t < 1/6) return p + (q-p)*6*t;
      if (t < 1/2) return q;
      if (t < 2/3) return p + (q-p)*(2/3-t)*6;
      return p;
    };
    const q = l < 0.5 ? l*(1+s) : l+s-l*s;
    const p = 2*l - q;
    r = hue2rgb(p,q,h+1/3); g = hue2rgb(p,q,h); b = hue2rgb(p,q,h-1/3);
  }
  return [r*255|0, g*255|0, b*255|0];
}

function animate() {
  requestAnimationFrame(animate);
  const t = Date.now() * 0.001;
  
  // Downsampled for performance (4px blocks)
  const step = 4;
  for (let y = 0; y < H; y += step) {
    for (let x = 0; x < W; x += step) {
      const v = plasma(x, y, t);
      const hue = (v * 60 + 200) % 360;
      const [r, g, b] = hsl2rgb(hue, 70, 50);
      for (let dy = 0; dy < step && y+dy < H; dy++) {
        for (let dx = 0; dx < step && x+dx < W; dx++) {
          const idx = ((y+dy)*W + (x+dx)) * 4;
          data[idx]=r; data[idx+1]=g; data[idx+2]=b; data[idx+3]=255;
        }
      }
    }
  }
  ctx.putImageData(imageData, 0, 0);
}
```

### Key Details
- Downsample 4x for performance
- Sum of sines at different frequencies
- HSL color cycling
- Mouse influence on center offset

---

## 6. Full-Screen Shader (Three.js Orthographic)

**Source:** saved_resource(11).html, saved_resource(15).html
**Best for:** Login pages, atmospheric backgrounds, SSO screens
**Keywords:** background, atmosphere, gradient, noise, shader

### Core Implementation
```javascript
const camera = new THREE.OrthographicCamera(-1, 1, 1, -1, 0.1, 10);
camera.position.z = 1;

const geometry = new THREE.PlaneGeometry(2, 2);
const material = new THREE.ShaderMaterial({
  uniforms: {
    uTime: { value: 0 },
    uResolution: { value: new THREE.Vector2(window.innerWidth, window.innerHeight) },
    uMouse: { value: new THREE.Vector2(0.5, 0.5) },
    uColorCore: { value: new THREE.Color('#FFFFFF') },
    uColorFringe: { value: new THREE.Color('#4A88FF') }
  },
  vertexShader: `
    varying vec2 vUv;
    void main() {
      vUv = uv;
      gl_Position = vec4(position, 1.0);
    }
  `,
  fragmentShader: `
    uniform float uTime;
    uniform vec2 uResolution;
    uniform vec2 uMouse;
    uniform vec3 uColorCore;
    uniform vec3 uColorFringe;
    varying vec2 vUv;
    
    // Noise functions here
    
    void main() {
      vec2 uv = gl_FragCoord.xy / uResolution.xy;
      vec2 st = uv;
      st.x *= uResolution.x / uResolution.y;
      
      vec2 mouseOffset = (uMouse - 0.5) * 0.1;
      st += mouseOffset;
      
      // Create glowing arcs
      vec2 center = vec2(0.2, 0.5);
      float d1 = length(st - center) - 0.8;
      float d2 = length(st - center) - 0.82;
      
      float coreGlow = exp(-abs(d1) * 40.0);
      float fringeGlow = exp(-abs(d2) * 15.0);
      
      vec3 finalColor = uColorCore * coreGlow + uColorFringe * fringeGlow;
      float alpha = clamp(coreGlow + fringeGlow, 0.0, 1.0);
      
      gl_FragColor = vec4(finalColor, alpha);
    }
  `,
  transparent: true,
  blending: THREE.NormalBlending
});

scene.add(new THREE.Mesh(geometry, material));
```

### Key Details
- OrthographicCamera for flat shader
- Full-screen quad (PlaneGeometry 2x2)
- Mouse parallax effect
- Exponential glow falloff

---

## 7. Swarm Particles (Canvas 2D)

**Source:** saved_resource(14).html
**Best for:** Interactive art, mouse-reactive backgrounds
**Keywords:** swarm, particles, mouse, interactive, repulsion

### Core Implementation
```javascript
const numParticles = 400;
let particles = [];

for (let i = 0; i < numParticles; i++) {
  particles.push({
    x: Math.random() * width,
    y: Math.random() * height,
    originX: Math.random() * width,
    originY: Math.random() * height,
    size: Math.random() * 2 + 1,
    vx: 0, vy: 0
  });
}

function animate() {
  ctx.clearRect(0, 0, width, height);
  ctx.fillStyle = '#000';
  
  particles.forEach(p => {
    // Spring back to origin
    let dx = p.originX - p.x;
    let dy = p.originY - p.y;
    p.vx += dx * 0.01;
    p.vy += dy * 0.01;
    
    // Mouse repulsion
    if (isHovering) {
      let mdx = mouseX - p.x;
      let mdy = mouseY - p.y;
      let dist = Math.sqrt(mdx * mdx + mdy * mdy);
      if (dist < 80) {
        let force = (80 - dist) / 80;
        p.vx -= (mdx / dist) * force * 2;
        p.vy -= (mdy / dist) * force * 2;
      }
    }
    
    // Damping
    p.vx *= 0.85;
    p.vy *= 0.85;
    
    p.x += p.vx;
    p.y += p.vy;
    
    ctx.beginPath();
    ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2);
    ctx.fill();
  });
  
  requestAnimationFrame(animate);
}
```

### Key Details
- Particles have origin positions
- Spring force pulls back to origin
- Mouse repulsion within radius
- Velocity damping for smooth motion

---

## 8. Dithered Terrain (Three.js + Custom Shader)

**Source:** saved_resource(1).html
**Best for:** Technical aesthetics, retro-futuristic, data visualization
**Keywords:** dither, terrain, retro, technical, bitmap

### Core Implementation
```javascript
const fragmentShader = `
  varying vec2 vUv;
  varying float vElevation;
  uniform float uTime;
  
  float dither(vec2 position, float brightness) {
    // 4x4 Bayer Matrix
    int x = int(mod(position.x, 4.0));
    int y = int(mod(position.y, 4.0));
    int index = x + y * 4;
    float limit = 0.0;
    
    // Bayer matrix values
    if (index == 0) limit = 0.0625;
    if (index == 8) limit = 0.5625;
    // ... etc
    
    return brightness < limit ? 0.0 : 1.0;
  }
  
  void main() {
    float light = vElevation * 2.0 + 0.5;
    float scanline = sin(vUv.y * 800.0) * 0.1;
    light += scanline;
    
    vec2 screenPos = gl_FragCoord.xy / 2.0;
    float finalColor = dither(screenPos, light);
    
    vec3 color = mix(vec3(0.96), vec3(0.1), finalColor);
    gl_FragColor = vec4(color, 1.0);
  }
`;
```

### Key Details
- Bayer matrix for dithering effect
- Scanline overlay
- Black and white only
- Technical, printed aesthetic

---

## 9. Theme Switching (CSS + JavaScript)

**Source:** saved_resource(11).html
**Best for:** SSO pages, dashboards, configurable UIs
**Keywords:** theme, dark, light, toggle, switch

### Core Implementation
```javascript
// CSS Custom Properties
[data-theme="dark"] {
  --bg-base: #030407;
  --text-primary: #FFFFFF;
  --shader-core: #FFFFFF;
  --shader-fringe: #4A88FF;
}

[data-theme="light"] {
  --bg-base: #F5F7FA;
  --text-primary: #050608;
  --shader-core: #050608;
  --shader-fringe: #8BA3CC;
}

// JavaScript toggle
themeBtn.addEventListener('click', () => {
  const currentTheme = htmlElement.getAttribute('data-theme');
  const newTheme = currentTheme === 'dark' ? 'light' : 'dark';
  htmlElement.setAttribute('data-theme', newTheme);
  
  // Update shader uniforms
  setTimeout(() => {
    const colors = getComputedColors();
    if (material) {
      material.uniforms.u_colorCore.value = colors.core;
      material.uniforms.u_colorFringe.value = colors.fringe;
    }
  }, 50);
});
```

### Key Details
- CSS custom properties for theming
- JavaScript reads computed styles
- Shader uniforms update on theme change
- Smooth CSS transitions

---

## 10. Interactive Canvas with Mouse Tracking

**Source:** Multiple files
**Best for:** Any interactive visual effect
**Keywords:** mouse, interactive, track, follow, cursor

### Core Implementation
```javascript
const mouse = { x: W/2, y: H/2 };
const mouseTarget = { x: W/2, y: H/2 };

window.addEventListener('mousemove', e => {
  mouseTarget.x = e.clientX;
  mouseTarget.y = e.clientY;
});

// Smooth interpolation in animate()
mouse.x += (mouseTarget.x - mouse.x) * 0.05;
mouse.y += (mouseTarget.y - mouse.y) * 0.05;

// Three.js version
const mouse3D = new THREE.Vector2(0, 0);
const mouseTarget3D = new THREE.Vector2(0, 0);

window.addEventListener('mousemove', e => {
  mouseTarget3D.x = (e.clientX / window.innerWidth - 0.5) * 2;
  mouseTarget3D.y = -(e.clientY / window.innerHeight - 0.5) * 2;
});

// In animate()
mouse3D.x += (mouseTarget3D.x - mouse3D.x) * 0.05;
mouse3D.y += (mouseTarget3D.y - mouse3D.y) * 0.05;
```

### Key Details
- Smooth interpolation with lerp
- Normalize coordinates for Three.js
- Update every frame
- Use for camera parallax or object movement
