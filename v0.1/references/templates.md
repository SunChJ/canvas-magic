# HTML Templates — Ready-to-Use Structures

These templates provide the complete HTML structure for different types of visual effects. Copy and customize as needed.

---

## Template 1: Three.js Full-Screen Effect

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[Effect Name]</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    
    body {
      background: #050505;
      overflow: hidden;
      font-family: 'Space Grotesk', -apple-system, sans-serif;
    }
    
    canvas {
      display: block;
      position: fixed;
      top: 0;
      left: 0;
      width: 100vw;
      height: 100vh;
    }
    
    .overlay {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      pointer-events: none;
      z-index: 10;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      padding: 40px;
    }
    
    .header {
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    
    .logo {
      font-size: 14px;
      font-weight: 600;
      letter-spacing: 0.1em;
      text-transform: uppercase;
      color: rgba(255,255,255,0.7);
    }
    
    .status {
      font-family: 'JetBrains Mono', monospace;
      font-size: 11px;
      color: rgba(255,255,255,0.4);
      letter-spacing: 0.05em;
    }
    
    .footer {
      display: flex;
      justify-content: space-between;
      align-items: flex-end;
    }
    
    .coordinates {
      font-family: 'JetBrains Mono', monospace;
      font-size: 11px;
      color: rgba(255,255,255,0.3);
    }
    
    .instructions {
      font-size: 12px;
      color: rgba(255,255,255,0.5);
      text-align: right;
    }
  </style>
</head>
<body>
  <div class="overlay">
    <div class="header">
      <div class="logo">[Brand]</div>
      <div class="status">[Status Text]</div>
    </div>
    <div class="footer">
      <div class="coordinates" id="coords">0.000, 0.000</div>
      <div class="instructions">Move mouse to interact</div>
    </div>
  </div>
  
  <script type="importmap">
  {
    "imports": {
      "three": "https://unpkg.com/three@0.160.0/build/three.module.js",
      "three/addons/": "https://unpkg.com/three@0.160.0/examples/jsm/"
    }
  }
  </script>
  
  <script type="module">
    import * as THREE from 'three';
    
    // Scene setup
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
    camera.position.z = 5;
    
    const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
    renderer.setSize(window.innerWidth, window.innerHeight);
    renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
    document.body.appendChild(renderer.domElement);
    
    // Mouse tracking
    const mouse = new THREE.Vector2(0, 0);
    const mouseTarget = new THREE.Vector2(0, 0);
    
    window.addEventListener('mousemove', e => {
      mouseTarget.x = (e.clientX / window.innerWidth - 0.5) * 2;
      mouseTarget.y = -(e.clientY / window.innerHeight - 0.5) * 2;
      document.getElementById('coords').textContent = 
        `${mouseTarget.x.toFixed(3)}, ${mouseTarget.y.toFixed(3)}`;
    });
    
    // Clock
    const clock = new THREE.Clock();
    
    // Create your objects here
    // const geometry = new THREE.SphereGeometry(1.5, 128, 128);
    // const material = new THREE.ShaderMaterial({...});
    // const mesh = new THREE.Mesh(geometry, material);
    // scene.add(mesh);
    
    // Animation loop
    function animate() {
      requestAnimationFrame(animate);
      const t = clock.getElapsedTime();
      
      // Smooth mouse
      mouse.x += (mouseTarget.x - mouse.x) * 0.05;
      mouse.y += (mouseTarget.y - mouse.y) * 0.05;
      
      // Update your objects here
      // material.uniforms.uTime.value = t;
      // material.uniforms.uMouse.value.copy(mouse);
      
      renderer.render(scene, camera);
    }
    animate();
    
    // Resize handler
    window.addEventListener('resize', () => {
      camera.aspect = window.innerWidth / window.innerHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(window.innerWidth, window.innerHeight);
    });
  </script>
</body>
</html>
```

---

## Template 2: Canvas 2D Effect

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[Effect Name]</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    
    body {
      background: #050505;
      overflow: hidden;
    }
    
    canvas {
      display: block;
    }
  </style>
</head>
<body>
  <canvas id="canvas"></canvas>
  
  <script>
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
    const mouseTarget = { x: W/2, y: H/2 };
    
    window.addEventListener('mousemove', e => {
      mouseTarget.x = e.clientX;
      mouseTarget.y = e.clientY;
    });
    
    // Your particle/effect classes here
    // class Particle { ... }
    
    // const particles = Array.from({ length: 100 }, () => new Particle());
    
    function animate() {
      requestAnimationFrame(animate);
      
      // Smooth mouse
      mouse.x += (mouseTarget.x - mouse.x) * 0.05;
      mouse.y += (mouseTarget.y - mouse.y) * 0.05;
      
      // Clear or trail effect
      ctx.fillStyle = 'rgba(5, 5, 5, 0.1)'; // Trail effect
      ctx.fillRect(0, 0, W, H);
      // OR
      // ctx.clearRect(0, 0, W, H); // No trail
      
      // Update and draw here
      // particles.forEach(p => { p.update(); p.draw(); });
    }
    animate();
  </script>
</body>
</html>
```

---

## Template 3: Hybrid (Three.js + HTML Overlay)

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[Page Title]</title>
  <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@300;400;500;600;700&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg: #050505;
      --text: #EEEAF4;
      --text-muted: rgba(238,234,244,0.5);
      --accent: #4A6CF7;
      --gold: #C9A84C;
    }
    
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    
    body {
      background: var(--bg);
      color: var(--text);
      font-family: 'Space Grotesk', sans-serif;
      overflow-x: hidden;
    }
    
    #canvas-container {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: 0;
    }
    
    .content {
      position: relative;
      z-index: 10;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
    }
    
    nav {
      position: sticky;
      top: 0;
      padding: 20px 40px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      backdrop-filter: blur(20px);
      background: rgba(5,5,5,0.8);
      border-bottom: 1px solid rgba(255,255,255,0.07);
    }
    
    .nav-logo {
      font-weight: 700;
      font-size: 18px;
      letter-spacing: -0.02em;
    }
    
    .nav-links {
      display: flex;
      gap: 32px;
    }
    
    .nav-links a {
      color: var(--text-muted);
      text-decoration: none;
      font-size: 13px;
      letter-spacing: 0.05em;
      text-transform: uppercase;
      transition: color 0.3s;
    }
    
    .nav-links a:hover {
      color: var(--text);
    }
    
    .hero {
      flex: 1;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      text-align: center;
      padding: 120px 40px;
      min-height: 100vh;
    }
    
    .hero-tag {
      font-family: 'JetBrains Mono', monospace;
      font-size: 11px;
      letter-spacing: 0.2em;
      text-transform: uppercase;
      color: var(--gold);
      margin-bottom: 24px;
    }
    
    h1 {
      font-size: clamp(3rem, 8vw, 7rem);
      font-weight: 700;
      line-height: 0.9;
      letter-spacing: -0.04em;
      margin-bottom: 24px;
      background: linear-gradient(135deg, #ffffff 0%, rgba(255,255,255,0.7) 100%);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }
    
    .hero-sub {
      font-size: 18px;
      color: var(--text-muted);
      max-width: 500px;
      line-height: 1.6;
    }
    
    .corner-frame {
      position: fixed;
      top: 20px;
      left: 20px;
      right: 20px;
      bottom: 20px;
      pointer-events: none;
      z-index: 5;
    }
    
    .corner {
      position: absolute;
      width: 20px;
      height: 20px;
      border-color: rgba(255,255,255,0.15);
      border-style: solid;
    }
    
    .corner.tl { top: 0; left: 0; border-width: 1px 0 0 1px; }
    .corner.tr { top: 0; right: 0; border-width: 1px 1px 0 0; }
    .corner.bl { bottom: 0; left: 0; border-width: 0 0 1px 1px; }
    .corner.br { bottom: 0; right: 0; border-width: 0 1px 1px 0; }
  </style>
</head>
<body>
  <div id="canvas-container"></div>
  
  <div class="corner-frame">
    <div class="corner tl"></div>
    <div class="corner tr"></div>
    <div class="corner bl"></div>
    <div class="corner br"></div>
  </div>
  
  <div class="content">
    <nav>
      <div class="nav-logo">Brand</div>
      <div class="nav-links">
        <a href="#">Work</a>
        <a href="#">About</a>
        <a href="#">Contact</a>
      </div>
    </nav>
    
    <section class="hero">
      <div class="hero-tag">// Section Tag</div>
      <h1>Your Headline<br>Here</h1>
      <p class="hero-sub">A brief description or tagline that explains what this is about.</p>
    </section>
  </div>
  
  <script type="importmap">
  {
    "imports": {
      "three": "https://unpkg.com/three@0.160.0/build/three.module.js",
      "three/addons/": "https://unpkg.com/three@0.160.0/examples/jsm/"
    }
  }
  </script>
  
  <script type="module">
    import * as THREE from 'three';
    
    const container = document.getElementById('canvas-container');
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
    camera.position.z = 5;
    
    const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
    renderer.setSize(window.innerWidth, window.innerHeight);
    renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
    container.appendChild(renderer.domElement);
    
    const mouse = new THREE.Vector2(0, 0);
    const mouseTarget = new THREE.Vector2(0, 0);
    
    window.addEventListener('mousemove', e => {
      mouseTarget.x = (e.clientX / window.innerWidth - 0.5) * 2;
      mouseTarget.y = -(e.clientY / window.innerHeight - 0.5) * 2;
    });
    
    const clock = new THREE.Clock();
    
    // Create your Three.js objects here
    
    function animate() {
      requestAnimationFrame(animate);
      const t = clock.getElapsedTime();
      
      mouse.x += (mouseTarget.x - mouse.x) * 0.05;
      mouse.y += (mouseTarget.y - mouse.y) * 0.05;
      
      // Update objects
      
      renderer.render(scene, camera);
    }
    animate();
    
    window.addEventListener('resize', () => {
      camera.aspect = window.innerWidth / window.innerHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(window.innerWidth, window.innerHeight);
    });
  </script>
</body>
</html>
```

---

## Template 4: CRT / Retro Terminal

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Terminal</title>
  <link href="https://fonts.googleapis.com/css2?family=VT323&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg: #0a1a0a;
      --text: #00ff41;
      --glow: rgba(0, 255, 65, 0.4);
    }
    
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    
    body {
      background: var(--bg);
      color: var(--text);
      font-family: 'VT323', monospace;
      font-size: 18px;
      line-height: 1.4;
      min-height: 100vh;
      padding: 40px;
      overflow: hidden;
    }
    
    /* CRT Effects */
    body::before {
      content: '';
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: radial-gradient(ellipse at center, transparent 60%, rgba(0,0,0,0.6) 100%);
      pointer-events: none;
      z-index: 999;
    }
    
    body::after {
      content: '';
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: repeating-linear-gradient(
        0deg,
        rgba(0,0,0,0.04) 0px,
        rgba(0,0,0,0.04) 1px,
        transparent 1px,
        transparent 2px
      );
      pointer-events: none;
      z-index: 998;
    }
    
    .terminal {
      max-width: 800px;
      margin: 0 auto;
    }
    
    .line {
      margin-bottom: 8px;
    }
    
    .prompt {
      color: #00ff41;
    }
    
    .command {
      color: #ffffff;
    }
    
    .output {
      color: rgba(0, 255, 65, 0.7);
    }
    
    .cursor {
      display: inline-block;
      width: 10px;
      height: 18px;
      background: var(--text);
      animation: blink 1s infinite;
      vertical-align: middle;
    }
    
    @keyframes blink {
      0%, 49% { opacity: 1; }
      50%, 100% { opacity: 0; }
    }
    
    .glow {
      text-shadow: 0 0 10px var(--glow), 0 0 20px var(--glow);
    }
  </style>
</head>
<body>
  <div class="terminal">
    <div class="line">
      <span class="prompt">$</span>
      <span class="command"> whoami</span>
    </div>
    <div class="line output">user@system</div>
    <div class="line">
      <span class="prompt">$</span>
      <span class="command"> </span>
      <span class="cursor"></span>
    </div>
  </div>
  
  <script>
    // Typing effect
    const lines = document.querySelectorAll('.line');
    let lineIndex = 0;
    
    function typeLine() {
      if (lineIndex >= lines.length) return;
      
      const line = lines[lineIndex];
      const text = line.textContent;
      line.textContent = '';
      line.style.visibility = 'visible';
      
      let charIndex = 0;
      const interval = setInterval(() => {
        line.textContent += text[charIndex];
        charIndex++;
        if (charIndex >= text.length) {
          clearInterval(interval);
          lineIndex++;
          setTimeout(typeLine, 500);
        }
      }, 50);
    }
    
    // Glitch effect
    function glitch(element) {
      const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789@#$%&';
      const original = element.textContent;
      let iter = 0;
      
      const interval = setInterval(() => {
        element.textContent = original.split('').map((c, i) =>
          i < iter ? c : chars[Math.floor(Math.random() * chars.length)]
        ).join('');
        
        if (iter >= original.length) clearInterval(interval);
        iter += 0.35;
      }, 30);
    }
    
    // Start
    typeLine();
  </script>
</body>
</html>
```

---

## Template 5: Minimal Light Theme

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Minimal</title>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg: #ffffff;
      --bg-alt: #f7f7f5;
      --text: #111111;
      --text-muted: #888888;
      --border: #e8e8e8;
      --accent: #000000;
    }
    
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    
    body {
      background: var(--bg);
      color: var(--text);
      font-family: 'Inter', -apple-system, sans-serif;
      line-height: 1.6;
    }
    
    .container {
      max-width: 1200px;
      margin: 0 auto;
      padding: 0 40px;
    }
    
    header {
      padding: 24px 0;
      border-bottom: 1px solid var(--border);
    }
    
    .header-inner {
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    
    .logo {
      font-weight: 700;
      font-size: 20px;
      letter-spacing: -0.02em;
    }
    
    nav a {
      color: var(--text-muted);
      text-decoration: none;
      font-size: 14px;
      margin-left: 32px;
      transition: color 0.2s;
    }
    
    nav a:hover {
      color: var(--text);
    }
    
    .hero {
      padding: 120px 0;
      text-align: center;
    }
    
    h1 {
      font-size: clamp(2.5rem, 5vw, 4rem);
      font-weight: 700;
      line-height: 1.1;
      letter-spacing: -0.03em;
      margin-bottom: 24px;
    }
    
    .subtitle {
      font-size: 18px;
      color: var(--text-muted);
      max-width: 600px;
      margin: 0 auto 40px;
    }
    
    .btn {
      display: inline-block;
      padding: 14px 32px;
      background: var(--accent);
      color: white;
      text-decoration: none;
      font-weight: 500;
      font-size: 14px;
      border-radius: 8px;
      transition: transform 0.2s, box-shadow 0.2s;
    }
    
    .btn:hover {
      transform: translateY(-2px);
      box-shadow: 0 8px 24px rgba(0,0,0,0.15);
    }
    
    #canvas-bg {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: 0;
      opacity: 0.3;
    }
    
    .content {
      position: relative;
      z-index: 10;
    }
  </style>
</head>
<body>
  <canvas id="canvas-bg"></canvas>
  
  <div class="content">
    <header>
      <div class="container header-inner">
        <div class="logo">Brand</div>
        <nav>
          <a href="#">Work</a>
          <a href="#">About</a>
          <a href="#">Contact</a>
        </nav>
      </div>
    </header>
    
    <section class="hero">
      <div class="container">
        <h1>Your Headline<br>Here</h1>
        <p class="subtitle">A brief description that explains your product or service.</p>
        <a href="#" class="btn">Get Started</a>
      </div>
    </section>
  </div>
  
  <script>
    const canvas = document.getElementById('canvas-bg');
    const ctx = canvas.getContext('2d');
    let W, H;
    
    function resize() {
      W = canvas.width = window.innerWidth;
      H = canvas.height = window.innerHeight;
    }
    window.addEventListener('resize', resize);
    resize();
    
    const mouse = { x: W/2, y: H/2 };
    window.addEventListener('mousemove', e => {
      mouse.x = e.clientX;
      mouse.y = e.clientY;
    });
    
    // Subtle dot grid
    function animate() {
      requestAnimationFrame(animate);
      ctx.clearRect(0, 0, W, H);
      
      const t = Date.now() * 0.001;
      const spacing = 40;
      const cols = Math.ceil(W / spacing);
      const rows = Math.ceil(H / spacing);
      
      for (let i = 0; i < cols; i++) {
        for (let j = 0; j < rows; j++) {
          const x = i * spacing + spacing/2;
          const y = j * spacing + spacing/2;
          
          const dx = x - mouse.x;
          const dy = y - mouse.y;
          const dist = Math.sqrt(dx*dx + dy*dy);
          const influence = Math.max(0, 1 - dist / 200);
          
          const r = 1 + influence * 3;
          const alpha = 0.1 + influence * 0.3;
          
          ctx.beginPath();
          ctx.arc(x, y, r, 0, Math.PI * 2);
          ctx.fillStyle = `rgba(0, 0, 0, ${alpha})`;
          ctx.fill();
        }
      }
    }
    animate();
  </script>
</body>
</html>
```
