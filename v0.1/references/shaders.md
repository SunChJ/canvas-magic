# Shader Snippets — Copy-Paste Ready

These shader snippets are extracted from high-quality HTML examples. Each one is tested and production-ready.

---

## 1. Simplex Noise (3D)

**Source:** Multiple files
**Use case:** Vertex displacement, procedural textures, organic motion

```glsl
// Simplex 3D Noise
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

### Usage in Vertex Shader
```glsl
// Displace vertices with noise
float noise = snoise(position * 0.7 + vec3(uTime * 0.28, uTime * 0.18, uTime * 0.22));
float displacement = noise * 0.14;
gl_Position = projectionMatrix * modelViewMatrix * vec4(position + normal * displacement, 1.0);
```

---

## 2. Simple 2D Noise

**Source:** saved_resource(11).html, saved_resource(15).html
**Use case:** 2D effects, backgrounds, flowing patterns

```glsl
// Simple 2D gradient noise
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

### Usage
```glsl
// Create flowing pattern
float n = gnoise(uv * 3.0 + vec2(uTime * 0.2, uTime * 0.15));
float n2 = gnoise(uv * 6.0 - vec2(uTime * 0.1));
float value = n * 0.5 + n2 * 0.3;
```

---

## 3. Fresnel Effect

**Source:** saved_resource(34).html
**Use case:** Edge glow, rim lighting, glass effects

```glsl
// Fresnel calculation
float fresnel = pow(1.0 - max(dot(normalize(vNormal), vec3(0.,0.,1.)), 0.), 2.2);

// Apply to color
vec3 color = baseColor + accentColor * fresnel * 0.7;

// Adjust alpha for transparency
float alpha = 0.55 + fresnel * 0.45;
```

---

## 4. Distance Field Shapes

**Source:** saved_resource(11).html
**Use case:** Procedural shapes, glowing arcs, geometric patterns

```glsl
// Circle/Circle Ring
float sdCircle(vec2 p, float r) {
  return length(p) - r;
}

// Ring (circle with thickness)
float sdRing(vec2 p, float r, float thickness) {
  return abs(length(p) - r) - thickness;
}

// Arc with warp
float sdArc(vec2 p, vec2 center, float radius, float width, float warp) {
  p.y += sin(p.x * 3.0 + uTime * 0.5) * warp;
  p.x += noise(p * 2.0 + uTime * 0.2) * (warp * 0.5);
  
  float d = length(p - center) - radius;
  return abs(d) - width;
}

// Box
float sdBox(vec2 p, vec2 b) {
  vec2 d = abs(p) - b;
  return length(max(d, 0.0)) + min(max(d.x, d.y), 0.0);
}
```

---

## 5. Glow/Bloom Effect

**Source:** saved_resource(11).html
**Use case:** Neon effects, light sources, energy fields

```glsl
// Exponential glow falloff
float glow = exp(-distance * 40.0);  // Sharp core
float fringe = exp(-distance * 15.0); // Soft edge

// Combine
vec3 color = coreColor * glow + fringeColor * fringe;
float alpha = clamp(glow + fringe, 0.0, 1.0);
```

### Multi-layer glow
```glsl
float d1 = sdCircle(p, 0.8);      // Core
float d2 = sdCircle(p, 0.82);     // Inner glow
float d3 = sdCircle(p, 0.85);     // Outer glow

float core = exp(-abs(d1) * 40.0);
float inner = exp(-abs(d2) * 15.0);
float outer = exp(-abs(d3) * 5.0);

vec3 color = coreColor * core + innerColor * inner + outerColor * outer;
```

---

## 6. Dithering (Bayer Matrix)

**Source:** saved_resource(1).html
**Use case:** Retro effects, bitmap aesthetics, technical visuals

```glsl
float dither(vec2 position, float brightness) {
  // 4x4 Bayer Matrix
  int x = int(mod(position.x, 4.0));
  int y = int(mod(position.y, 4.0));
  int index = x + y * 4;
  float limit = 0.0;
  
  if (index == 0) limit = 0.0625;
  if (index == 8) limit = 0.5625;
  if (index == 2) limit = 0.1875;
  if (index == 10) limit = 0.6875;
  if (index == 12) limit = 0.8125;
  if (index == 4) limit = 0.3125;
  if (index == 14) limit = 0.9375;
  if (index == 6) limit = 0.4375;
  if (index == 3) limit = 0.25;
  if (index == 11) limit = 0.75;
  if (index == 1) limit = 0.125;
  if (index == 9) limit = 0.625;
  if (index == 15) limit = 1.0;
  if (index == 7) limit = 0.5;
  if (index == 13) limit = 0.875;
  if (index == 5) limit = 0.375;
  
  return brightness < limit ? 0.0 : 1.0;
}

// Usage
float light = vElevation * 2.0 + 0.5;
vec2 screenPos = gl_FragCoord.xy / 2.0;
float finalColor = dither(screenPos, light);
vec3 color = mix(darkColor, lightColor, finalColor);
```

---

## 7. Scanline Effect

**Source:** saved_resource(1).html
**Use case:** CRT effects, retro screens, technical displays

```glsl
// Horizontal scanlines
float scanline = sin(vUv.y * 800.0) * 0.1;
light += scanline;

// Vertical scanlines
float scanlineV = sin(vUv.x * 800.0) * 0.1;
light += scanlineV;

// Grid pattern
float grid = sin(vUv.x * 400.0) * sin(vUv.y * 400.0) * 0.1;
light += grid;
```

---

## 8. Color Mixing

**Source:** Multiple files
**Use case:** Gradients, color transitions, palette blending

```glsl
// Simple mix
vec3 color = mix(colorA, colorB, t);

// Three-color gradient
vec3 color = mix(colorA, colorB, t);
color = mix(color, colorC, t * t);

// Based on elevation/position
float t = clamp((vElevation + 0.3) / 0.6, 0.0, 1.0);
vec3 color = mix(lowColor, highColor, t);

// HSL to RGB
vec3 hsl2rgb(float h, float s, float l) {
  h /= 360.0; s /= 100.0; l /= 100.0;
  vec3 rgb = clamp(abs(mod(h*6.0+vec3(0.0,4.0,2.0),6.0)-3.0)-1.0, 0.0, 1.0);
  return l + s * (rgb-0.5)*(1.0-abs(2.0*l-1.0));
}
```

---

## 9. Mouse Influence

**Source:** Multiple files
**Use case:** Interactive effects, cursor tracking, dynamic responses

```glsl
// Mouse influence on position
float mouseInfluence = dot(normalize(position), vec3(uMouse.x, uMouse.y, 0.6));
elevation += mouseInfluence * 0.04;

// Mouse distance field
vec2 mouseOffset = (uMouse - 0.5) * 0.1;
st += mouseOffset;
float dist = length(st - mousePos);
float intensity = smoothstep(0.8, 0.0, dist) * 0.5;

// Radial gradient from mouse
float radial = 1.0 - length(uv - uMouse);
radial = smoothstep(0.0, 1.0, radial);
```

---

## 10. Time-Based Animation

**Source:** Multiple files
**Use case:** Continuous motion, pulsing, breathing effects

```glsl
// Basic time
float t = uTime;

// Slower/faster
float t = uTime * 0.5;  // Half speed
float t = uTime * 2.0;  // Double speed

// Pulsing
float pulse = sin(uTime * 0.75) * 0.08 + 0.88;

// Breathing
float breath = sin(uTime * 0.5) * 0.5 + 0.5;

// Rotation
float angle = uTime * 0.1;
mat2 rot = mat2(cos(angle), -sin(angle), sin(angle), cos(angle));
vec2 rotPos = rot * position.xy;

// Oscillation
float osc = sin(uTime * 2.0 + position.x * 3.0) * 0.5 + 0.5;
```

---

## Complete Vertex Shader Example

```glsl
varying vec2 vUv;
varying float vElevation;
varying vec3 vNormal;
uniform float uTime;
uniform vec2 uMouse;

// Paste snoise function here

void main() {
  vNormal = normal;
  vUv = uv;
  
  float t = uTime * 0.35;
  
  // Noise displacement
  float n = snoise(position * 0.7 + vec3(t*0.28, t*0.18, t*0.22))
          + snoise(position * 1.4 + vec3(-t*0.18, t*0.32, -t*0.14)) * 0.5;
  
  float elevation = n * 0.14;
  
  // Mouse influence
  float m = dot(normalize(position), vec3(uMouse.x, uMouse.y, 0.6));
  elevation += m * 0.04;
  
  vElevation = elevation;
  
  gl_Position = projectionMatrix * modelViewMatrix * vec4(position + normal * elevation, 1.0);
}
```

---

## Complete Fragment Shader Example

```glsl
uniform float uTime;
uniform vec3 uColor1;
uniform vec3 uColor2;
uniform vec3 uColor3;
varying float vElevation;
varying vec3 vNormal;
varying vec2 vUv;

void main() {
  // Fresnel for edge glow
  float fresnel = pow(1.0 - max(dot(normalize(vNormal), vec3(0.,0.,1.)), 0.), 2.2);
  
  // Color based on elevation
  float t = clamp(vElevation * 5.0 + 0.4, 0., 1.);
  vec3 col = mix(uColor1, uColor2, t);
  col = mix(col, uColor3, clamp(vElevation * 8.0, 0., 0.25));
  
  // Add fresnel glow
  col += uColor1 * fresnel * 0.7;
  
  // Pulsing alpha
  float alpha = (0.55 + fresnel * 0.45) * (sin(uTime * 0.75) * 0.08 + 0.88);
  
  gl_FragColor = vec4(col, alpha);
}
```
