# Color Palettes — Curated for Visual Effects

These color palettes are extracted from high-quality HTML examples. Each palette is designed to work harmoniously for specific visual effects.

---

## 1. Dark Luxury (Default)

**Best for:** Most visual effects, premium feel, Three.js scenes
**Background:** Near-perfect black
**Accent:** Electric blue, gold highlights

```css
:root {
  --bg: #050505;
  --bg-2: #0a0a0f;
  --surface: rgba(255,255,255,0.03);
  --surface-2: rgba(255,255,255,0.06);
  --border: rgba(255,255,255,0.07);
  --text: #EEEAF4;
  --text-muted: rgba(238,234,244,0.5);
  --text-dim: rgba(238,234,244,0.2);
  --accent: #4A6CF7;
  --accent-dim: rgba(74,108,247,0.15);
  --gold: #C9A84C;
  --gold-dim: rgba(201,168,76,0.12);
}
```

### Three.js Colors
```javascript
const colors = {
  primary: new THREE.Color(0x4A6CF7),    // Electric blue
  secondary: new THREE.Color(0xC9A84C),  // Gold
  dark: new THREE.Color(0x050505),       // Background
  light: new THREE.Color(0xEEEAF4)       // Text
};
```

---

## 2. Cosmic / AI

**Best for:** AI products, space themes, futuristic effects
**Background:** Deep space black
**Accent:** Purple, pink, cyan

```css
:root {
  --bg: #030308;
  --bg-2: #070a12;
  --surface: rgba(255,255,255,0.04);
  --border: rgba(255,255,255,0.08);
  --text: #E8EDF5;
  --text-muted: rgba(232,237,245,0.5);
  --text-dim: rgba(232,237,245,0.2);
  --accent: #7B61FF;
  --accent-2: #FF6B9D;
  --accent-3: #00D4FF;
  --green: #00ff9d;
}
```

### Three.js Colors
```javascript
const colors = {
  primary: new THREE.Color(0x7B61FF),    // Purple
  secondary: new THREE.Color(0xFF6B9D),  // Pink
  tertiary: new THREE.Color(0x00D4FF),   // Cyan
  dark: new THREE.Color(0x030308)        // Background
};
```

---

## 3. Organic / Nature

**Best for:** Nature themes, botanical effects, earthy visuals
**Background:** Deep forest green
**Accent:** Leaf green, amber, terracotta

```css
:root {
  --bg: #0a0f0a;
  --bg-2: #0f1a0f;
  --surface: rgba(255,255,255,0.03);
  --border: rgba(255,255,255,0.06);
  --text: #E8F5E9;
  --text-muted: rgba(232,245,233,0.5);
  --accent: #4CAF50;
  --accent-2: #8BC34A;
  --warm: #FF9800;
  --earth: #795548;
}
```

### Three.js Colors
```javascript
const colors = {
  primary: new THREE.Color(0x4CAF50),    // Green
  secondary: new THREE.Color(0x8BC34A),  // Light green
  warm: new THREE.Color(0xFF9800),       // Orange
  dark: new THREE.Color(0x0a0f0a)        // Background
};
```

---

## 4. Retro CRT

**Best for:** Terminal effects, retro aesthetics, hacker themes
**Background:** Phosphor green monitor
**Text:** Terminal green

```css
:root {
  --bg: #0a1a0a;
  --bg-2: #0f2a0f;
  --text: #00ff41;
  --text-muted: rgba(0,255,65,0.5);
  --text-dim: rgba(0,255,65,0.2);
  --glow: rgba(0,255,65,0.4);
  --red: #ff2a2a;
  --amber: #ffaa00;
}
```

### Three.js Colors
```javascript
const colors = {
  primary: new THREE.Color(0x00ff41),    // Terminal green
  glow: new THREE.Color(0x00ff41),       // Glow
  dark: new THREE.Color(0x0a1a0a)        // Background
};
```

---

## 5. Minimal Light

**Best for:** Clean designs, editorial layouts, professional sites
**Background:** Pure white
**Text:** Rich black

```css
:root {
  --bg: #ffffff;
  --bg-alt: #f7f7f5;
  --border: #e8e8e8;
  --border-2: #d0d0d0;
  --text: #111111;
  --text-muted: #888888;
  --text-dim: #cccccc;
  --accent: #000000;
  --accent-blue: #0066ff;
}
```

### Three.js Colors
```javascript
const colors = {
  primary: new THREE.Color(0x111111),    // Black
  accent: new THREE.Color(0x0066ff),     // Blue
  light: new THREE.Color(0xffffff)       // White
};
```

---

## 6. Warm Cream

**Best for:** Organic brands, luxury goods, warm aesthetics
**Background:** Aged paper cream
**Text:** Warm near-black

```css
:root {
  --bg: #F4F1E9;
  --bg-2: #EDE9DF;
  --border: rgba(0,0,0,0.1);
  --text: #1a1a16;
  --text-muted: rgba(26,26,22,0.55);
  --accent: #2d4a1e;    /* Deep botanical green */
  --warm: #C05621;      /* Terracotta */
  --cream: #E8D5B7;
  --amber: #8B6914;
}
```

### Three.js Colors
```javascript
const colors = {
  primary: new THREE.Color(0x2d4a1e),    // Green
  warm: new THREE.Color(0xC05621),       // Terracotta
  light: new THREE.Color(0xF4F1E9)       // Cream
};
```

---

## 7. Neon Cyber

**Best for:** Cyberpunk effects, glitch art, digital themes
**Background:** Deep black
**Accent:** Neon pink, cyan, yellow

```css
:root {
  --bg: #000000;
  --bg-2: #050505;
  --surface: rgba(255,255,255,0.03);
  --border: rgba(255,255,255,0.06);
  --text: #ffffff;
  --text-muted: rgba(255,255,255,0.5);
  --neon-pink: #ff00ff;
  --neon-cyan: #00ffff;
  --neon-yellow: #ffff00;
  --neon-green: #00ff00;
}
```

### Three.js Colors
```javascript
const colors = {
  pink: new THREE.Color(0xff00ff),
  cyan: new THREE.Color(0x00ffff),
  yellow: new THREE.Color(0xffff00),
  dark: new THREE.Color(0x000000)
};
```

---

## 8. Ocean Deep

**Best for:** Water effects, deep sea themes, aquatic visuals
**Background:** Deep ocean blue
**Accent:** Aqua, teal, white

```css
:root {
  --bg: #0a1628;
  --bg-2: #0f2035;
  --surface: rgba(255,255,255,0.04);
  --border: rgba(255,255,255,0.08);
  --text: #E0F7FA;
  --text-muted: rgba(224,247,250,0.5);
  --accent: #00BCD4;
  --accent-2: #0097A7;
  --foam: #ffffff;
}
```

### Three.js Colors
```javascript
const colors = {
  primary: new THREE.Color(0x00BCD4),    // Cyan
  deep: new THREE.Color(0x0097A7),       // Teal
  dark: new THREE.Color(0x0a1628)        // Background
};
```

---

## 9. Sunset Gradient

**Best for:** Warm effects, gradient backgrounds, atmospheric scenes
**Background:** Deep purple
**Accent:** Orange, pink, yellow

```css
:root {
  --bg: #1a0a2e;
  --bg-2: #2d1b4e;
  --text: #FFE0B2;
  --text-muted: rgba(255,224,178,0.5);
  --orange: #FF6B35;
  --pink: #FF1493;
  --yellow: #FFD700;
  --purple: #9C27B0;
}
```

### Three.js Colors
```javascript
const colors = {
  orange: new THREE.Color(0xFF6B35),
  pink: new THREE.Color(0xFF1493),
  yellow: new THREE.Color(0xFFD700),
  dark: new THREE.Color(0x1a0a2e)
};
```

---

## 10. Monochrome

**Best for:** Minimal effects, typography-focused, brutalist design
**Background:** White or black
**Text:** Opposite of background

```css
/* Light version */
:root {
  --bg: #ffffff;
  --text: #000000;
  --gray-1: #333333;
  --gray-2: #666666;
  --gray-3: #999999;
  --gray-4: #cccccc;
  --gray-5: #eeeeee;
}

/* Dark version */
:root {
  --bg: #000000;
  --text: #ffffff;
  --gray-1: #cccccc;
  --gray-2: #999999;
  --gray-3: #666666;
  --gray-4: #333333;
  --gray-5: #111111;
}
```

### Three.js Colors
```javascript
const colors = {
  black: new THREE.Color(0x000000),
  white: new THREE.Color(0xffffff),
  gray: new THREE.Color(0x888888)
};
```

---

## Color Harmony Rules

### For Glowing Effects
- Use **AdditiveBlending** in Three.js
- Keep background dark (near black)
- Accent colors should be bright and saturated
- Use opacity < 1 for layered glow

### For Subtle Effects
- Use **NormalBlending** in Three.js
- Keep colors muted
- Use low opacity (0.1-0.3)
- Layer multiple semi-transparent elements

### For High Contrast
- Use complementary colors (opposite on color wheel)
- Example: Blue (#4A6CF7) + Gold (#C9A84C)
- Example: Purple (#7B61FF) + Yellow (#FFD700)

### For Monochromatic
- Use different shades of same hue
- Vary lightness/saturation only
- Example: Dark blue → Light blue → White
