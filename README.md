<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Salim Moustanir — Vibe Coder</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;700;800&family=DM+Mono:ital,wght@0,400;0,500;1,400&display=swap" rel="stylesheet"/>
<style>
  :root {
    --black: #050505;
    --yellow: #F7DF1E;
    --green: #00ff94;
    --pink: #ff2d7a;
    --blue: #00c2ff;
    --white: #f0ede6;
    --dim: #333;
    --font-head: 'Syne', sans-serif;
    --font-mono: 'DM Mono', monospace;
  }

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    background: var(--black);
    color: var(--white);
    font-family: var(--font-mono);
    min-height: 100vh;
    overflow-x: hidden;
    cursor: none;
  }

  /* Custom cursor */
  .cursor {
    width: 12px; height: 12px;
    background: var(--yellow);
    border-radius: 50%;
    position: fixed;
    pointer-events: none;
    z-index: 9999;
    transition: transform 0.15s ease;
    mix-blend-mode: difference;
  }
  .cursor-ring {
    width: 36px; height: 36px;
    border: 1px solid var(--yellow);
    border-radius: 50%;
    position: fixed;
    pointer-events: none;
    z-index: 9998;
    transition: all 0.25s ease;
    mix-blend-mode: difference;
    transform: translate(-12px, -12px);
  }

  /* Scanline overlay */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background: repeating-linear-gradient(
      0deg,
      transparent,
      transparent 2px,
      rgba(0,0,0,0.07) 2px,
      rgba(0,0,0,0.07) 4px
    );
    pointer-events: none;
    z-index: 100;
  }

  /* Noise texture */
  body::after {
    content: '';
    position: fixed;
    inset: 0;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='0.04'/%3E%3C/svg%3E");
    pointer-events: none;
    z-index: 99;
    opacity: 0.5;
  }

  /* === LAYOUT === */
  .page { max-width: 860px; margin: 0 auto; padding: 60px 32px; position: relative; z-index: 10; }

  /* === HERO === */
  .hero {
    border: 1px solid var(--dim);
    padding: 48px 40px;
    margin-bottom: 2px;
    position: relative;
    overflow: hidden;
    animation: fadeUp 0.8s ease both;
  }
  .hero::before {
    content: '';
    position: absolute;
    top: -80px; right: -80px;
    width: 300px; height: 300px;
    background: radial-gradient(circle, rgba(247,223,30,0.12) 0%, transparent 70%);
    pointer-events: none;
  }
  .hero-label {
    font-family: var(--font-mono);
    font-size: 11px;
    letter-spacing: 0.25em;
    color: var(--yellow);
    text-transform: uppercase;
    margin-bottom: 16px;
    opacity: 0.7;
  }
  .hero-name {
    font-family: var(--font-head);
    font-size: clamp(52px, 8vw, 88px);
    font-weight: 800;
    line-height: 0.92;
    letter-spacing: -0.03em;
    color: var(--white);
    margin-bottom: 20px;
    animation: fadeUp 0.8s 0.1s ease both;
  }
  .hero-name span {
    color: var(--yellow);
    position: relative;
    display: inline-block;
  }
  .hero-name span::after {
    content: '';
    position: absolute;
    bottom: 4px; left: 0;
    width: 100%; height: 3px;
    background: var(--yellow);
    transform: scaleX(0);
    transform-origin: left;
    animation: lineIn 0.6s 1s ease forwards;
  }
  .hero-tag {
    font-size: 13px;
    color: rgba(240,237,230,0.5);
    letter-spacing: 0.1em;
    animation: fadeUp 0.8s 0.2s ease both;
    margin-bottom: 32px;
  }
  .hero-tag em {
    color: var(--green);
    font-style: normal;
    font-weight: 500;
  }
  .hero-badge {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    border: 1px solid var(--yellow);
    padding: 8px 16px;
    font-size: 12px;
    letter-spacing: 0.12em;
    color: var(--yellow);
    text-transform: uppercase;
    animation: fadeUp 0.8s 0.3s ease both;
  }
  .hero-badge::before {
    content: '';
    width: 6px; height: 6px;
    background: var(--yellow);
    border-radius: 50%;
    animation: pulse 1.5s ease-in-out infinite;
  }
  .hero-flag {
    position: absolute;
    top: 40px; right: 40px;
    font-size: 40px;
    opacity: 0.15;
    filter: grayscale(0.3);
  }

  /* === CODE BLOCK === */
  .code-block {
    background: #0a0a0a;
    border: 1px solid var(--dim);
    border-top: none;
    padding: 32px 40px;
    margin-bottom: 2px;
    animation: fadeUp 0.8s 0.2s ease both;
    position: relative;
    overflow: hidden;
  }
  .code-block::before {
    content: '// about.ts';
    position: absolute;
    top: 14px; right: 24px;
    font-size: 10px;
    letter-spacing: 0.15em;
    color: var(--dim);
    text-transform: uppercase;
  }
  .code-line { display: block; line-height: 1.8; font-size: 13px; }
  .c-key { color: var(--blue); }
  .c-str { color: var(--yellow); }
  .c-val { color: var(--green); }
  .c-dim { color: #555; }
  .c-arr { color: var(--pink); }
  .c-type { color: #a78bfa; }

  /* === SECTION HEADER === */
  .section-header {
    display: flex;
    align-items: center;
    gap: 16px;
    border: 1px solid var(--dim);
    border-top: none;
    padding: 20px 40px;
    margin-bottom: 2px;
  }
  .section-number {
    font-family: var(--font-mono);
    font-size: 11px;
    color: var(--dim);
    letter-spacing: 0.2em;
    min-width: 32px;
  }
  .section-title {
    font-family: var(--font-head);
    font-size: 22px;
    font-weight: 700;
    letter-spacing: -0.02em;
    color: var(--white);
  }
  .section-line {
    flex: 1;
    height: 1px;
    background: linear-gradient(to right, var(--dim), transparent);
  }

  /* === SKILLS GRID === */
  .skills-panel {
    border: 1px solid var(--dim);
    border-top: none;
    padding: 32px 40px;
    margin-bottom: 2px;
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 28px;
    animation: fadeUp 0.8s 0.3s ease both;
  }
  .skill-group-label {
    font-size: 10px;
    letter-spacing: 0.3em;
    text-transform: uppercase;
    color: var(--pink);
    margin-bottom: 14px;
    display: flex;
    align-items: center;
    gap: 10px;
  }
  .skill-group-label::after {
    content: '';
    flex: 1;
    height: 1px;
    background: rgba(255,45,122,0.2);
  }
  .skill-tags { display: flex; flex-wrap: wrap; gap: 8px; }
  .tag {
    font-size: 11px;
    padding: 5px 12px;
    border: 1px solid #222;
    color: rgba(240,237,230,0.7);
    letter-spacing: 0.08em;
    transition: all 0.2s ease;
    position: relative;
    overflow: hidden;
  }
  .tag:hover {
    border-color: var(--yellow);
    color: var(--yellow);
    background: rgba(247,223,30,0.05);
  }
  .tag.hot {
    border-color: rgba(0,255,148,0.3);
    color: var(--green);
    background: rgba(0,255,148,0.04);
  }
  .tag.hot:hover { border-color: var(--green); background: rgba(0,255,148,0.1); }

  /* === STATS === */
  .stats-row {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    border: 1px solid var(--dim);
    border-top: none;
    margin-bottom: 2px;
    animation: fadeUp 0.8s 0.4s ease both;
  }
  .stat-cell {
    padding: 28px 32px;
    border-right: 1px solid var(--dim);
    text-align: center;
    position: relative;
    overflow: hidden;
    transition: background 0.3s ease;
  }
  .stat-cell:last-child { border-right: none; }
  .stat-cell:hover { background: rgba(247,223,30,0.03); }
  .stat-cell::before {
    content: '';
    position: absolute;
    bottom: 0; left: 0;
    width: 0; height: 1px;
    background: var(--yellow);
    transition: width 0.4s ease;
  }
  .stat-cell:hover::before { width: 100%; }
  .stat-num {
    font-family: var(--font-head);
    font-size: 42px;
    font-weight: 800;
    color: var(--yellow);
    line-height: 1;
    margin-bottom: 8px;
    letter-spacing: -0.04em;
  }
  .stat-label {
    font-size: 10px;
    letter-spacing: 0.25em;
    text-transform: uppercase;
    color: rgba(240,237,230,0.35);
  }

  /* === MANIFESTO === */
  .manifesto {
    border: 1px solid var(--dim);
    border-top: none;
    padding: 40px;
    margin-bottom: 2px;
    position: relative;
    animation: fadeUp 0.8s 0.5s ease both;
    overflow: hidden;
  }
  .manifesto::before {
    content: '"';
    position: absolute;
    top: -20px; left: 32px;
    font-family: var(--font-head);
    font-size: 180px;
    font-weight: 800;
    color: rgba(247,223,30,0.04);
    line-height: 1;
    pointer-events: none;
  }
  .manifesto-text {
    font-family: var(--font-head);
    font-size: clamp(18px, 3vw, 26px);
    font-weight: 700;
    line-height: 1.4;
    letter-spacing: -0.02em;
    color: var(--white);
    position: relative;
    z-index: 1;
  }
  .manifesto-text .accent { color: var(--yellow); }
  .manifesto-text .accent2 { color: var(--green); }

  /* === CONTACT === */
  .contact-row {
    border: 1px solid var(--dim);
    border-top: none;
    padding: 32px 40px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    flex-wrap: wrap;
    gap: 20px;
    animation: fadeUp 0.8s 0.6s ease both;
  }
  .contact-text {
    font-size: 12px;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    color: rgba(240,237,230,0.4);
  }
  .contact-links { display: flex; gap: 12px; flex-wrap: wrap; }
  .contact-link {
    font-size: 11px;
    letter-spacing: 0.15em;
    text-transform: uppercase;
    color: var(--white);
    text-decoration: none;
    padding: 9px 20px;
    border: 1px solid #222;
    transition: all 0.2s ease;
    position: relative;
    overflow: hidden;
  }
  .contact-link::before {
    content: '';
    position: absolute;
    inset: 0;
    background: var(--yellow);
    transform: translateX(-100%);
    transition: transform 0.25s ease;
    z-index: -1;
  }
  .contact-link:hover { color: var(--black); border-color: var(--yellow); }
  .contact-link:hover::before { transform: translateX(0); }

  /* === FOOTER === */
  .footer {
    padding: 24px 40px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    animation: fadeUp 0.8s 0.7s ease both;
  }
  .footer-left {
    font-size: 10px;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    color: var(--dim);
  }
  .footer-right {
    font-size: 10px;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    color: var(--dim);
  }
  .footer-right span { color: var(--yellow); }

  /* === ANIMATIONS === */
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(24px); }
    to   { opacity: 1; transform: translateY(0); }
  }
  @keyframes pulse {
    0%, 100% { opacity: 1; transform: scale(1); }
    50% { opacity: 0.4; transform: scale(0.7); }
  }
  @keyframes lineIn {
    to { transform: scaleX(1); }
  }
  @keyframes blink {
    0%, 100% { opacity: 1; }
    50% { opacity: 0; }
  }

  .blink { animation: blink 1s step-end infinite; }

  /* Glitch on name hover */
  .hero-name:hover {
    animation: glitch 0.3s linear;
  }
  @keyframes glitch {
    0%   { text-shadow: none; }
    20%  { text-shadow: -2px 0 var(--pink), 2px 0 var(--blue); }
    40%  { text-shadow: 2px 0 var(--pink), -2px 0 var(--blue); }
    60%  { text-shadow: -2px 0 var(--green), 2px 0 var(--pink); }
    80%  { text-shadow: 2px 0 var(--blue), -2px 0 var(--green); }
    100% { text-shadow: none; }
  }

  /* Responsive */
  @media (max-width: 600px) {
    .skills-panel { grid-template-columns: 1fr; }
    .stats-row { grid-template-columns: 1fr; }
    .stat-cell { border-right: none; border-bottom: 1px solid var(--dim); }
    .hero { padding: 32px 24px; }
    .code-block, .skills-panel, .manifesto, .contact-row { padding: 24px; }
    .hero-flag { display: none; }
  }
</style>
</head>
<body>

<div class="cursor" id="cursor"></div>
<div class="cursor-ring" id="cursorRing"></div>

<div class="page">

  <!-- HERO -->
  <div class="hero">
    <div class="hero-flag">🇲🇦</div>
    <div class="hero-label">// github.com/salimmous — profile readme</div>
    <h1 class="hero-name">SALIM<br/><span>MOUSTANIR</span></h1>
    <div class="hero-tag">Full-Stack Developer &nbsp;×&nbsp; UI/UX Artisan &nbsp;×&nbsp; <em>Vibe Coder</em></div>
    <div class="hero-badge">Available for collabs<span class="blink">_</span></div>
  </div>

  <!-- ABOUT CODE BLOCK -->
  <div class="code-block">
    <code>
      <span class="code-line"><span class="c-dim">const </span><span class="c-key">salim</span><span class="c-dim">: </span><span class="c-type">Developer</span><span class="c-dim"> = {</span></span>
      <span class="code-line">&nbsp;&nbsp;<span class="c-key">origin</span><span class="c-dim">:&nbsp;&nbsp;&nbsp;&nbsp; </span><span class="c-str">"Casablanca, Morocco 🇲🇦"</span><span class="c-dim">,</span></span>
      <span class="code-line">&nbsp;&nbsp;<span class="c-key">focus</span><span class="c-dim">:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span class="c-arr">["Mobile Apps", "Web Platforms", "Design Systems"]</span><span class="c-dim">,</span></span>
      <span class="code-line">&nbsp;&nbsp;<span class="c-key">currently</span><span class="c-dim">: </span><span class="c-str">"Shipping products that feel alive ⚡"</span><span class="c-dim">,</span></span>
      <span class="code-line">&nbsp;&nbsp;<span class="c-key">vibe</span><span class="c-dim">:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span class="c-val">true</span><span class="c-dim">,</span></span>
      <span class="code-line"><span class="c-dim">};</span></span>
    </code>
  </div>

  <!-- SKILLS HEADER -->
  <div class="section-header">
    <span class="section-number">01</span>
    <span class="section-title">Tech Stack</span>
    <span class="section-line"></span>
  </div>

  <!-- SKILLS -->
  <div class="skills-panel">
    <div>
      <div class="skill-group-label">Mobile</div>
      <div class="skill-tags">
        <span class="tag hot">Flutter</span>
        <span class="tag hot">React Native</span>
        <span class="tag">iOS / Swift</span>
        <span class="tag">Android</span>
      </div>
    </div>
    <div>
      <div class="skill-group-label">Web Frontend</div>
      <div class="skill-tags">
        <span class="tag hot">React</span>
        <span class="tag">Vue.js</span>
        <span class="tag">TypeScript</span>
        <span class="tag">JavaScript</span>
      </div>
    </div>
    <div>
      <div class="skill-group-label">Backend & Cloud</div>
      <div class="skill-tags">
        <span class="tag">Node.js</span>
        <span class="tag">Firebase</span>
        <span class="tag">AWS</span>
        <span class="tag">Azure</span>
        <span class="tag">PHP</span>
      </div>
    </div>
    <div>
      <div class="skill-group-label">Design & Tools</div>
      <div class="skill-tags">
        <span class="tag hot">Figma</span>
        <span class="tag">Adobe XD</span>
        <span class="tag">Illustrator</span>
        <span class="tag">Git</span>
        <span class="tag">Docker</span>
      </div>
    </div>
  </div>

  <!-- STATS HEADER -->
  <div class="section-header">
    <span class="section-number">02</span>
    <span class="section-title">By the Numbers</span>
    <span class="section-line"></span>
  </div>

  <!-- STATS ROW -->
  <div class="stats-row">
    <div class="stat-cell">
      <div class="stat-num" data-target="5">0</div>
      <div class="stat-label">Years Shipping</div>
    </div>
    <div class="stat-cell">
      <div class="stat-num" data-target="30">0</div>
      <div class="stat-label">Projects Delivered</div>
    </div>
    <div class="stat-cell">
      <div class="stat-num" data-target="∞" data-special>∞</div>
      <div class="stat-label">Vibes Generated</div>
    </div>
  </div>

  <!-- MANIFESTO HEADER -->
  <div class="section-header">
    <span class="section-number">03</span>
    <span class="section-title">The Philosophy</span>
    <span class="section-line"></span>
  </div>

  <!-- MANIFESTO -->
  <div class="manifesto">
    <div class="manifesto-text">
      Code isn't just <span class="accent">logic</span> —<br/>
      it's an <span class="accent2">energy</span> you send into the world.<br/>
      Every pixel, every function,<br/>
      every commit should <span class="accent">hit different.</span>
    </div>
  </div>

  <!-- CONTACT -->
  <div class="contact-row">
    <div class="contact-text">Let's build something → </div>
    <div class="contact-links">
      <a href="mailto:salim.moustanir@gmail.com" class="contact-link">Email</a>
      <a href="https://linkedin.com/in/salimmoustanir" class="contact-link">LinkedIn</a>
      <a href="https://twitter.com/moustanirsalim" class="contact-link">Twitter</a>
      <a href="https://instagram.com/salimmous1" class="contact-link">Instagram</a>
    </div>
  </div>

  <!-- FOOTER -->
  <div class="footer">
    <div class="footer-left">salimmous — 2025</div>
    <div class="footer-right">Made in <span>Morocco 🇲🇦</span></div>
  </div>

</div>

<script>
  // Custom cursor
  const cursor = document.getElementById('cursor');
  const ring = document.getElementById('cursorRing');
  let mx = 0, my = 0, rx = 0, ry = 0;
  document.addEventListener('mousemove', e => {
    mx = e.clientX; my = e.clientY;
    cursor.style.left = mx - 6 + 'px';
    cursor.style.top  = my - 6 + 'px';
  });
  function animRing() {
    rx += (mx - rx - 18) * 0.12;
    ry += (my - ry - 18) * 0.12;
    ring.style.left = rx + 'px';
    ring.style.top  = ry + 'px';
    requestAnimationFrame(animRing);
  }
  animRing();

  // Counter animation
  function animateCount(el, target, duration = 1800) {
    let start = null;
    const step = ts => {
      if (!start) start = ts;
      const progress = Math.min((ts - start) / duration, 1);
      const ease = 1 - Math.pow(1 - progress, 3);
      el.textContent = Math.floor(ease * target) + (progress >= 1 ? '+' : '');
      if (progress < 1) requestAnimationFrame(step);
    };
    requestAnimationFrame(step);
  }

  const observer = new IntersectionObserver(entries => {
    entries.forEach(e => {
      if (e.isIntersecting) {
        const nums = e.target.querySelectorAll('[data-target]');
        nums.forEach(n => {
          if (!n.dataset.special) animateCount(n, parseInt(n.dataset.target));
        });
        observer.unobserve(e.target);
      }
    });
  }, { threshold: 0.3 });

  document.querySelector('.stats-row') && observer.observe(document.querySelector('.stats-row'));
</script>
</body>
</html>
