<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>COSMOS — Dashboard Educativo</title>
<link href="https://fonts.googleapis.com/css2?family=Space+Mono:ital,wght@0,400;0,700;1,400&family=Syne:wght@400;600;700;800&family=Crimson+Pro:ital,wght@0,300;1,300&display=swap" rel="stylesheet">
<style>
  :root {
    --void: #020408;
    --deep: #060d1a;
    --nebula: #0a1628;
    --accent1: #4fc3f7;
    --accent2: #f06292;
    --accent3: #aed581;
    --accent4: #ffb74d;
    --gold: #ffd54f;
    --glow1: rgba(79,195,247,0.15);
    --glow2: rgba(240,98,146,0.15);
    --text: #e8f4fd;
    --muted: #7ba3c8;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    font-family: 'Syne', sans-serif;
    background: var(--void);
    color: var(--text);
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* STARFIELD */
  #starfield {
    position: fixed; inset: 0; z-index: 0;
    pointer-events: none;
  }

  /* HEADER */
  header {
    position: relative; z-index: 10;
    padding: 2rem 3rem 1rem;
    display: flex; align-items: center; justify-content: space-between;
    border-bottom: 1px solid rgba(79,195,247,0.15);
    backdrop-filter: blur(10px);
    background: linear-gradient(180deg, rgba(2,4,8,0.9) 0%, transparent 100%);
  }

  .logo {
    display: flex; align-items: baseline; gap: 0.4rem;
  }

  .logo-text {
    font-family: 'Space Mono', monospace;
    font-size: 2rem; font-weight: 700;
    letter-spacing: 0.3em;
    background: linear-gradient(135deg, var(--accent1), var(--accent2));
    -webkit-background-clip: text; -webkit-text-fill-color: transparent;
  }

  .logo-sub {
    font-family: 'Crimson Pro', serif;
    font-style: italic; font-size: 0.9rem;
    color: var(--muted); letter-spacing: 0.1em;
  }

  .header-time {
    font-family: 'Space Mono', monospace;
    font-size: 0.8rem; color: var(--muted);
    text-align: right; line-height: 1.8;
  }

  .live-dot {
    display: inline-block; width: 8px; height: 8px;
    background: var(--accent3); border-radius: 50%;
    animation: pulse 2s infinite;
    margin-right: 6px;
  }

  @keyframes pulse {
    0%,100%{opacity:1;transform:scale(1)}
    50%{opacity:0.5;transform:scale(1.3)}
  }

  /* NAV TABS */
  nav {
    position: relative; z-index: 10;
    display: flex; gap: 0; padding: 0 3rem;
    background: rgba(6,13,26,0.8);
    border-bottom: 1px solid rgba(255,255,255,0.05);
  }

  nav button {
    font-family: 'Syne', sans-serif;
    font-weight: 600; font-size: 0.8rem;
    letter-spacing: 0.15em; text-transform: uppercase;
    padding: 1rem 2rem; border: none; cursor: pointer;
    background: transparent; color: var(--muted);
    border-bottom: 3px solid transparent;
    transition: all 0.3s ease;
    position: relative;
  }

  nav button.active {
    color: var(--accent1);
    border-bottom-color: var(--accent1);
  }

  nav button:hover { color: var(--text); }

  nav button .tab-icon { margin-right: 8px; font-size: 1rem; }

  /* MAIN */
  main {
    position: relative; z-index: 10;
    padding: 2.5rem 3rem;
    max-width: 1400px; margin: 0 auto;
  }

  .panel { display: none; animation: fadeIn 0.5s ease; }
  .panel.active { display: block; }

  @keyframes fadeIn {
    from{opacity:0;transform:translateY(20px)}
    to{opacity:1;transform:translateY(0)}
  }

  /* GRID */
  .grid-3 { display: grid; grid-template-columns: repeat(3, 1fr); gap: 1.5rem; }
  .grid-2 { display: grid; grid-template-columns: repeat(2, 1fr); gap: 1.5rem; }
  .col-span-2 { grid-column: span 2; }
  .col-span-3 { grid-column: span 3; }

  /* CARDS */
  .card {
    background: linear-gradient(135deg, rgba(6,13,26,0.9) 0%, rgba(10,22,40,0.9) 100%);
    border: 1px solid rgba(79,195,247,0.1);
    border-radius: 16px;
    padding: 1.8rem;
    position: relative; overflow: hidden;
    transition: border-color 0.3s, transform 0.3s;
  }

  .card:hover {
    border-color: rgba(79,195,247,0.3);
    transform: translateY(-3px);
  }

  .card::before {
    content: '';
    position: absolute; top: 0; left: 0; right: 0; height: 1px;
    background: linear-gradient(90deg, transparent, var(--accent1), transparent);
    opacity: 0.5;
  }

  .card-label {
    font-family: 'Space Mono', monospace;
    font-size: 0.65rem; letter-spacing: 0.2em; text-transform: uppercase;
    color: var(--muted); margin-bottom: 1rem;
    display: flex; align-items: center; gap: 8px;
  }

  .card-label::after {
    content: ''; flex: 1; height: 1px;
    background: rgba(255,255,255,0.08);
  }

  .card-title {
    font-size: 1.3rem; font-weight: 700;
    margin-bottom: 0.5rem;
    background: linear-gradient(135deg, #fff, var(--muted));
    -webkit-background-clip: text; -webkit-text-fill-color: transparent;
  }

  .card-value {
    font-family: 'Space Mono', monospace;
    font-size: 2.8rem; font-weight: 700;
    line-height: 1;
  }

  .card-unit {
    font-size: 1rem; color: var(--muted); margin-left: 4px;
  }

  .card-desc {
    font-family: 'Crimson Pro', serif;
    font-size: 1rem; color: var(--muted);
    line-height: 1.6; margin-top: 0.8rem;
  }

  /* PLANET DISPLAY */
  .planet-scene {
    position: relative; height: 200px;
    display: flex; align-items: center; justify-content: center;
  }

  .planet {
    width: 120px; height: 120px;
    border-radius: 50%;
    position: relative;
    animation: float 6s ease-in-out infinite;
    cursor: pointer;
    transition: transform 0.3s;
  }

  .planet:hover { transform: scale(1.1); }

  @keyframes float {
    0%,100%{transform:translateY(0)}
    50%{transform:translateY(-12px)}
  }

  .planet-ring {
    position: absolute;
    width: 170px; height: 40px;
    border: 3px solid rgba(255,200,100,0.6);
    border-radius: 50%;
    top: 50%; left: 50%;
    transform: translate(-50%, -50%) rotateX(75deg);
    box-shadow: 0 0 20px rgba(255,200,100,0.3);
  }

  /* ORBIT ANIMATION */
  .orbit-system {
    position: relative; width: 300px; height: 300px;
    margin: 0 auto;
  }

  .sun {
    position: absolute; top: 50%; left: 50%;
    width: 50px; height: 50px;
    background: radial-gradient(circle, #fff7d6, #ffb300, #e65100);
    border-radius: 50%;
    transform: translate(-50%, -50%);
    box-shadow: 0 0 30px #ffb300, 0 0 60px rgba(255,179,0,0.4);
    animation: sunPulse 3s ease-in-out infinite;
  }

  @keyframes sunPulse {
    0%,100%{box-shadow:0 0 30px #ffb300,0 0 60px rgba(255,179,0,0.4)}
    50%{box-shadow:0 0 50px #ffb300,0 0 100px rgba(255,179,0,0.6)}
  }

  .orbit-path {
    position: absolute; top: 50%; left: 50%;
    border: 1px solid rgba(255,255,255,0.08);
    border-radius: 50%;
    transform: translate(-50%, -50%);
  }

  .orbiter {
    position: absolute; top: 50%; left: 50%;
    border-radius: 50%;
    transform-origin: 0 0;
  }

  /* WEATHER CARD */
  .weather-icon {
    font-size: 4rem;
    display: block; text-align: center;
    margin-bottom: 0.5rem;
    filter: drop-shadow(0 0 20px rgba(255,255,255,0.3));
  }

  /* PROGRESS BAR */
  .progress-bar {
    height: 6px; background: rgba(255,255,255,0.08);
    border-radius: 10px; overflow: hidden; margin-top: 0.8rem;
  }

  .progress-fill {
    height: 100%; border-radius: 10px;
    transition: width 1.5s ease;
  }

  /* FACT CARDS */
  .fact-card {
    background: linear-gradient(135deg, rgba(240,98,146,0.08), rgba(79,195,247,0.08));
    border: 1px solid rgba(240,98,146,0.2);
    border-radius: 12px; padding: 1.5rem;
    cursor: pointer; transition: all 0.3s;
  }

  .fact-card:hover {
    background: linear-gradient(135deg, rgba(240,98,146,0.15), rgba(79,195,247,0.15));
    transform: translateY(-4px);
    box-shadow: 0 20px 40px rgba(0,0,0,0.3);
  }

  .fact-number {
    font-family: 'Space Mono', monospace;
    font-size: 3rem; font-weight: 700;
    color: var(--accent2); line-height: 1;
  }

  .fact-text {
    font-family: 'Crimson Pro', serif;
    font-size: 1.1rem; color: var(--muted);
    margin-top: 0.5rem; line-height: 1.5;
  }

  /* QUIZ */
  .quiz-question {
    font-size: 1.4rem; font-weight: 700; margin-bottom: 1.5rem;
    line-height: 1.4;
  }

  .quiz-options {
    display: grid; grid-template-columns: 1fr 1fr; gap: 1rem;
  }

  .quiz-btn {
    font-family: 'Syne', sans-serif;
    font-size: 0.9rem; padding: 1rem;
    background: rgba(255,255,255,0.04);
    border: 1px solid rgba(255,255,255,0.1);
    border-radius: 10px; color: var(--text);
    cursor: pointer; transition: all 0.3s;
    text-align: left; line-height: 1.4;
  }

  .quiz-btn:hover {
    background: rgba(79,195,247,0.1);
    border-color: var(--accent1);
  }

  .quiz-btn.correct {
    background: rgba(174,213,129,0.2);
    border-color: var(--accent3);
    color: var(--accent3);
  }

  .quiz-btn.wrong {
    background: rgba(240,98,146,0.2);
    border-color: var(--accent2);
    color: var(--accent2);
  }

  .quiz-feedback {
    margin-top: 1.2rem; padding: 1rem;
    border-radius: 10px; font-family: 'Crimson Pro', serif;
    font-size: 1.05rem; line-height: 1.6; display: none;
  }

  .score-badge {
    font-family: 'Space Mono', monospace;
    font-size: 1.5rem; font-weight: 700;
    color: var(--gold);
    display: flex; align-items: center; gap: 8px;
  }

  /* TELESCOPE */
  .telescope-view {
    width: 100%; aspect-ratio: 1;
    max-width: 260px; margin: 0 auto;
    border-radius: 50%;
    background: radial-gradient(ellipse at 30% 30%, #1a2a4a, #020408);
    border: 4px solid rgba(79,195,247,0.3);
    position: relative; overflow: hidden;
    cursor: crosshair;
    box-shadow: 0 0 40px rgba(79,195,247,0.2), inset 0 0 60px rgba(0,0,0,0.5);
  }

  /* SECTION TITLE */
  .section-title {
    font-size: 1.8rem; font-weight: 800;
    margin-bottom: 1.5rem;
    display: flex; align-items: center; gap: 1rem;
  }

  .section-title span {
    background: linear-gradient(135deg, var(--accent1), var(--accent2));
    -webkit-background-clip: text; -webkit-text-fill-color: transparent;
  }

  .section-title::after {
    content: ''; flex: 1; height: 1px;
    background: linear-gradient(90deg, rgba(79,195,247,0.3), transparent);
  }

  /* BADGE */
  .badge {
    display: inline-block;
    font-family: 'Space Mono', monospace;
    font-size: 0.65rem; letter-spacing: 0.1em;
    padding: 3px 10px; border-radius: 20px;
    font-weight: 400;
  }

  .badge-blue { background: rgba(79,195,247,0.15); color: var(--accent1); border: 1px solid rgba(79,195,247,0.3); }
  .badge-pink { background: rgba(240,98,146,0.15); color: var(--accent2); border: 1px solid rgba(240,98,146,0.3); }
  .badge-green { background: rgba(174,213,129,0.15); color: var(--accent3); border: 1px solid rgba(174,213,129,0.3); }
  .badge-orange { background: rgba(255,183,77,0.15); color: var(--accent4); border: 1px solid rgba(255,183,77,0.3); }

  /* NEXT BTN */
  .btn-primary {
    font-family: 'Syne', sans-serif; font-weight: 700;
    font-size: 0.85rem; letter-spacing: 0.1em;
    padding: 0.8rem 2rem; border-radius: 8px;
    background: linear-gradient(135deg, var(--accent1), #0288d1);
    border: none; color: #000; cursor: pointer;
    transition: all 0.3s; margin-top: 1rem;
    display: inline-block;
  }

  .btn-primary:hover { transform: translateY(-2px); box-shadow: 0 8px 20px rgba(79,195,247,0.3); }

  /* PLANET SELECTOR */
  .planet-selector {
    display: flex; gap: 1rem; flex-wrap: wrap;
    margin-bottom: 1.5rem;
  }

  .planet-btn {
    font-family: 'Space Mono', monospace;
    font-size: 0.7rem; padding: 0.5rem 1rem;
    border-radius: 20px; border: 1px solid rgba(255,255,255,0.15);
    background: transparent; color: var(--muted); cursor: pointer;
    transition: all 0.3s;
  }

  .planet-btn.active, .planet-btn:hover {
    border-color: var(--accent1); color: var(--accent1);
    background: rgba(79,195,247,0.1);
  }

  /* CANVAS */
  canvas { display: block; }

  /* SCROLLBAR */
  ::-webkit-scrollbar { width: 6px; }
  ::-webkit-scrollbar-track { background: var(--void); }
  ::-webkit-scrollbar-thumb { background: rgba(79,195,247,0.3); border-radius: 3px; }

  /* RESPONSIVE */
  @media (max-width: 900px) {
    main { padding: 1.5rem; }
    header { padding: 1.5rem; }
    nav { padding: 0 1rem; }
    .grid-3, .grid-2 { grid-template-columns: 1fr; }
    .col-span-2, .col-span-3 { grid-column: span 1; }
  }

  .glow-text { text-shadow: 0 0 20px currentColor; }

  .data-row {
    display: flex; justify-content: space-between; align-items: center;
    padding: 0.7rem 0;
    border-bottom: 1px solid rgba(255,255,255,0.05);
    font-family: 'Space Mono', monospace; font-size: 0.8rem;
  }

  .data-row:last-child { border-bottom: none; }
  .data-key { color: var(--muted); }
  .data-val { color: var(--text); font-weight: 700; }
</style>
</head>
<body>

<canvas id="starfield"></canvas>

<header>
  <div class="logo">
    <div class="logo-text">COSMOS</div>
    <div class="logo-sub">Dashboard Educativo</div>
  </div>
  <div class="header-time">
    <div><span class="live-dot"></span>EN VIVO</div>
    <div id="clock">—</div>
    <div id="date-display" style="color:var(--muted);font-size:0.7rem;">—</div>
  </div>
</header>

<nav>
  <button class="active" onclick="switchTab('astronomia', this)">
    <span class="tab-icon">🪐</span>Astronomía
  </button>
  <button onclick="switchTab('clima', this)">
    <span class="tab-icon">🌦</span>Clima &amp; Tierra
  </button>
  <button onclick="switchTab('ciencia', this)">
    <span class="tab-icon">⚗️</span>Ciencia
  </button>
  <button onclick="switchTab('quiz', this)">
    <span class="tab-icon">🧠</span>Quiz
  </button>
</nav>

<main>

  <!-- ===== ASTRONOMÍA ===== -->
  <div class="panel active" id="panel-astronomia">
    <div class="section-title"><span>Sistema Solar</span></div>

    <div class="planet-selector" id="planet-selector">
      <button class="planet-btn active" onclick="selectPlanet(0,this)">☿ Mercurio</button>
      <button class="planet-btn" onclick="selectPlanet(1,this)">♀ Venus</button>
      <button class="planet-btn" onclick="selectPlanet(2,this)">🌍 Tierra</button>
      <button class="planet-btn" onclick="selectPlanet(3,this)">♂ Marte</button>
      <button class="planet-btn" onclick="selectPlanet(4,this)">♃ Júpiter</button>
      <button class="planet-btn" onclick="selectPlanet(5,this)">♄ Saturno</button>
      <button class="planet-btn" onclick="selectPlanet(6,this)">⛢ Urano</button>
      <button class="planet-btn" onclick="selectPlanet(7,this)">♆ Neptuno</button>
    </div>

    <div class="grid-3">
      <div class="card col-span-2" id="planet-card">
        <div class="card-label">Planeta Seleccionado</div>
        <div style="display:flex;gap:2rem;align-items:center;flex-wrap:wrap;">
          <div class="planet-scene" id="planet-display">
            <canvas id="planetCanvas" width="150" height="150"></canvas>
          </div>
          <div style="flex:1;min-width:200px;">
            <div class="card-title" id="planet-name">Mercurio</div>
            <div style="display:flex;gap:8px;flex-wrap:wrap;margin-bottom:1rem;" id="planet-badges"></div>
            <div class="card-desc" id="planet-desc">—</div>
            <div style="margin-top:1rem;" id="planet-data"></div>
          </div>
        </div>
      </div>

      <div class="card">
        <div class="card-label">Distancia al Sol</div>
        <div class="card-value" id="planet-dist" style="color:var(--accent4);font-size:1.8rem;">—</div>
        <div class="card-unit">millones km</div>
        <div class="progress-bar" style="margin-top:1rem;">
          <div class="progress-fill" id="dist-bar" style="background:linear-gradient(90deg,var(--accent4),var(--accent2));width:5%"></div>
        </div>
        <div class="card-desc" style="margin-top:0.8rem;font-size:0.85rem;" id="planet-orbit">Período orbital: —</div>
      </div>

      <div class="card col-span-3">
        <div class="card-label">Órbita en Tiempo Real</div>
        <canvas id="orbitCanvas" width="900" height="180" style="width:100%;height:180px;"></canvas>
      </div>
    </div>

    <div style="margin-top:1.5rem;" class="section-title"><span>Datos del Cosmos</span></div>
    <div class="grid-3">
      <div class="fact-card">
        <div class="fact-number" style="color:var(--accent1);">299,792</div>
        <div class="fact-text">km/s — La velocidad de la luz. En 1 segundo, la luz rodea la Tierra 7.5 veces.</div>
      </div>
      <div class="fact-card">
        <div class="fact-number" style="color:var(--accent4);">4.246</div>
        <div class="fact-text">años luz de distancia tiene Próxima Centauri, la estrella más cercana al Sol.</div>
      </div>
      <div class="fact-card">
        <div class="fact-number" style="color:var(--accent3);">2 × 10³⁰</div>
        <div class="fact-text">kg es la masa del Sol. Representa el 99.86% de toda la masa del Sistema Solar.</div>
      </div>
    </div>
  </div>

  <!-- ===== CLIMA ===== -->
  <div class="panel" id="panel-clima">
    <div class="section-title"><span>Clima &amp; Planeta Tierra</span></div>

    <div class="grid-3">
      <div class="card">
        <div class="card-label">Temperatura Global Promedio</div>
        <div class="card-value" style="color:var(--accent2);">15.1°</div>
        <div class="card-unit">Celsius</div>
        <div class="card-desc">+1.2°C sobre el promedio pre-industrial. El año 2023 fue el más cálido registrado.</div>
        <div class="progress-bar">
          <div class="progress-fill" style="background:linear-gradient(90deg,#42a5f5,#ef5350);width:72%"></div>
        </div>
      </div>

      <div class="card">
        <div class="card-label">Capas de la Atmósfera</div>
        <div id="atmo-layers" style="margin-top:0.5rem;"></div>
      </div>

      <div class="card">
        <div class="card-label">Fenómenos Climáticos</div>
        <div id="weather-phenomena"></div>
      </div>

      <div class="card col-span-2">
        <div class="card-label">Ciclo del Agua</div>
        <canvas id="waterCycleCanvas" width="600" height="220" style="width:100%;height:220px;"></canvas>
      </div>

      <div class="card">
        <div class="card-label">CO₂ Atmosférico</div>
        <div class="card-value" style="color:var(--accent2);font-size:2rem;">422 ppm</div>
        <div class="card-desc">Partes por millón de CO₂. El nivel más alto en 800,000 años.</div>
        <div class="data-row" style="margin-top:1rem;">
          <span class="data-key">1750 (pre-industrial)</span>
          <span class="data-val" style="color:var(--accent3);">280 ppm</span>
        </div>
        <div class="data-row">
          <span class="data-key">1950</span>
          <span class="data-val" style="color:var(--accent4);">311 ppm</span>
        </div>
        <div class="data-row">
          <span class="data-key">2000</span>
          <span class="data-val" style="color:var(--accent2);">370 ppm</span>
        </div>
        <div class="data-row">
          <span class="data-key">Hoy</span>
          <span class="data-val" style="color:var(--accent2);font-weight:700;">422 ppm</span>
        </div>
      </div>

      <div class="card col-span-3">
        <div class="card-label">Temperatura Global 1880–2024</div>
        <canvas id="tempChart" width="900" height="150" style="width:100%;height:150px;"></canvas>
      </div>
    </div>
  </div>

  <!-- ===== CIENCIA ===== -->
  <div class="panel" id="panel-ciencia">
    <div class="section-title"><span>Ciencia Fundamental</span></div>

    <div class="grid-3">
      <div class="card">
        <div class="card-label">Tabla Periódica — Elemento</div>
        <div id="element-display" style="text-align:center;padding:1rem 0;">
          <div style="font-size:4rem;font-family:'Space Mono',monospace;font-weight:700;color:var(--accent1);" id="el-symbol">H</div>
          <div style="font-size:1.2rem;font-weight:700;" id="el-name">Hidrógeno</div>
          <div style="font-size:0.8rem;color:var(--muted);font-family:'Space Mono',monospace;" id="el-num">Número atómico: 1</div>
          <div style="font-size:0.8rem;color:var(--muted);font-family:'Space Mono',monospace;" id="el-mass">Masa atómica: 1.008 u</div>
        </div>
        <div class="card-desc" id="el-desc">El elemento más abundante del universo. Forma el 75% de la masa elemental normal.</div>
        <button class="btn-primary" onclick="nextElement()" style="width:100%;text-align:center;">Siguiente Elemento →</button>
      </div>

      <div class="card">
        <div class="card-label">Átomo Visualizado</div>
        <canvas id="atomCanvas" width="260" height="200" style="width:100%;height:200px;"></canvas>
      </div>

      <div class="card">
        <div class="card-label">Constantes del Universo</div>
        <div class="data-row">
          <span class="data-key">Gravedad (g)</span>
          <span class="data-val" style="color:var(--accent1);">9.81 m/s²</span>
        </div>
        <div class="data-row">
          <span class="data-key">Planck (h)</span>
          <span class="data-val" style="color:var(--accent2);">6.63×10⁻³⁴ J·s</span>
        </div>
        <div class="data-row">
          <span class="data-key">Boltzmann (k)</span>
          <span class="data-val" style="color:var(--accent3);">1.38×10⁻²³ J/K</span>
        </div>
        <div class="data-row">
          <span class="data-key">Avogadro (N)</span>
          <span class="data-val" style="color:var(--accent4);">6.022×10²³</span>
        </div>
        <div class="data-row">
          <span class="data-key">Luz (c)</span>
          <span class="data-val" style="color:var(--accent1);">3×10⁸ m/s</span>
        </div>

        <div style="margin-top:1.2rem;padding:1rem;background:rgba(79,195,247,0.06);border-radius:10px;border:1px solid rgba(79,195,247,0.15);">
          <div style="font-family:'Space Mono',monospace;font-size:0.75rem;color:var(--muted);margin-bottom:4px;">E = mc²</div>
          <div style="font-family:'Crimson Pro',serif;font-size:0.95rem;line-height:1.5;color:var(--muted);">La ecuación más famosa de Einstein: la energía equivale a la masa por la velocidad de la luz al cuadrado.</div>
        </div>
      </div>

      <div class="card col-span-2">
        <div class="card-label">Escala del Universo</div>
        <canvas id="scaleCanvas" width="700" height="160" style="width:100%;height:160px;cursor:pointer;" onclick="animateScale()"></canvas>
        <div style="font-family:'Crimson Pro',serif;font-size:0.9rem;color:var(--muted);margin-top:0.8rem;text-align:center;">Haz clic para explorar la escala desde el átomo hasta el universo observable</div>
      </div>

      <div class="card">
        <div class="card-label">Tipos de Energía</div>
        <canvas id="energyCanvas" width="260" height="200" style="width:100%;height:200px;"></canvas>
      </div>
    </div>
  </div>

  <!-- ===== QUIZ ===== -->
  <div class="panel" id="panel-quiz">
    <div class="section-title"><span>Quiz Cósmico</span></div>

    <div class="grid-2">
      <div class="card col-span-2" style="max-width:700px;">
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:1.5rem;">
          <div class="badge badge-blue" id="quiz-category">Astronomía</div>
          <div class="score-badge">⭐ <span id="quiz-score">0</span> / <span id="quiz-total">0</span></div>
        </div>
        <div class="quiz-question" id="quiz-q">—</div>
        <div class="quiz-options" id="quiz-options"></div>
        <div class="quiz-feedback" id="quiz-feedback"></div>
        <button class="btn-primary" onclick="nextQuestion()" id="quiz-next" style="display:none;">Siguiente pregunta →</button>
        <div id="quiz-result" style="display:none;text-align:center;padding:2rem;">
          <div style="font-size:3rem;">🎉</div>
          <div style="font-size:1.5rem;font-weight:700;margin:1rem 0;">¡Completado!</div>
          <div class="score-badge" style="justify-content:center;font-size:2rem;" id="final-score"></div>
          <button class="btn-primary" onclick="restartQuiz()" style="margin-top:1.5rem;">Volver a intentar</button>
        </div>
      </div>
    </div>
  </div>

</main>

<script>
// ============ STARFIELD ============
const sf = document.getElementById('starfield');
sf.width = window.innerWidth;
sf.height = window.innerHeight;
const ctx = sf.getContext('2d');
const stars = Array.from({length:200},()=>({
  x:Math.random()*sf.width,
  y:Math.random()*sf.height,
  r:Math.random()*1.5+0.3,
  a:Math.random(),
  speed:Math.random()*0.005+0.002
}));

function drawStars(){
  ctx.clearRect(0,0,sf.width,sf.height);
  stars.forEach(s=>{
    s.a += s.speed;
    ctx.beginPath();
    ctx.arc(s.x,s.y,s.r,0,Math.PI*2);
    ctx.fillStyle=`rgba(255,255,255,${0.3+0.7*Math.abs(Math.sin(s.a))})`;
    ctx.fill();
  });
  requestAnimationFrame(drawStars);
}
drawStars();
window.addEventListener('resize',()=>{sf.width=window.innerWidth;sf.height=window.innerHeight;});

// ============ CLOCK ============
function updateClock(){
  const now = new Date();
  document.getElementById('clock').textContent = now.toLocaleTimeString('es-ES');
  document.getElementById('date-display').textContent = now.toLocaleDateString('es-ES',{weekday:'long',day:'numeric',month:'long'});
}
setInterval(updateClock,1000); updateClock();

// ============ TABS ============
function switchTab(id, btn){
  document.querySelectorAll('.panel').forEach(p=>p.classList.remove('active'));
  document.querySelectorAll('nav button').forEach(b=>b.classList.remove('active'));
  document.getElementById('panel-'+id).classList.add('active');
  btn.classList.add('active');
  if(id==='astronomia') { initOrbitCanvas(); setTimeout(renderPlanet,100); }
  if(id==='clima') { initWaterCycle(); initTempChart(); }
  if(id==='ciencia') { initAtomCanvas(); initScaleCanvas(); initEnergyCanvas(); }
  if(id==='quiz') initQuiz();
}

// ============ PLANETS ============
const planets = [
  {name:'Mercurio',color:'#9e9e9e',glow:'#9e9e9e',size:40,dist:57.9,period:'88 días',temp:'430°C día / -180°C noche',moons:0,desc:'El planeta más pequeño y rápido del Sistema Solar. No tiene atmósfera, por lo que sufre temperaturas extremas.',badges:['badge-blue','Rocoso'],ring:false},
  {name:'Venus',color:'#ffcc80',glow:'#ffcc80',size:55,dist:108.2,period:'225 días',temp:'465°C',moons:0,desc:'El planeta más caliente del Sistema Solar gracias a su efecto invernadero extremo. Gira en dirección opuesta a la mayoría.',badges:['badge-orange','Rocoso'],ring:false},
  {name:'Tierra',color:'#42a5f5',glow:'#42a5f5',size:55,dist:149.6,period:'365.25 días',temp:'15°C promedio',moons:1,desc:'Nuestro hogar: el único planeta conocido con vida. Tiene agua líquida en la superficie y una atmósfera perfecta.',badges:['badge-blue','Vida'],ring:false},
  {name:'Marte',color:'#ef5350',glow:'#ef5350',size:48,dist:227.9,period:'687 días',temp:'-60°C promedio',moons:2,desc:'El Planeta Rojo, con el volcán más grande del Sistema Solar: el Olympus Mons (21 km de altura).',badges:['badge-pink','Rocoso'],ring:false},
  {name:'Júpiter',color:'#ffb74d',glow:'#ffb74d',size:90,dist:778.5,period:'11.86 años',temp:'-110°C nubes',moons:95,desc:'El planeta más grande: tan enorme que todos los demás planetas cabrían dentro de él. Su Gran Mancha Roja es una tormenta de 300 años.',badges:['badge-orange','Gaseoso'],ring:false},
  {name:'Saturno',color:'#ffe082',glow:'#ffe082',size:80,dist:1432,period:'29.46 años',temp:'-140°C',moons:146,desc:'Famoso por sus espectaculares anillos de hielo y roca. Es el planeta menos denso: ¡flotaría en el agua!',badges:['badge-orange','Anillos'],ring:true},
  {name:'Urano',color:'#80deea',glow:'#80deea',size:65,dist:2867,period:'84 años',temp:'-195°C',moons:27,desc:'El gigante de hielo que gira de lado (inclinación axial de 98°). Es el planeta más frío del Sistema Solar.',badges:['badge-blue','Hielo'],ring:true},
  {name:'Neptuno',color:'#5c6bc0',glow:'#5c6bc0',size:62,dist:4515,period:'164.8 años',temp:'-200°C',moons:16,desc:'El planeta más lejano, con los vientos más rápidos del Sistema Solar, alcanzando 2,100 km/h.',badges:['badge-blue','Hielo'],ring:false},
];

let currentPlanet = 0;

function selectPlanet(i, btn){
  currentPlanet = i;
  document.querySelectorAll('.planet-btn').forEach(b=>b.classList.remove('active'));
  if(btn) btn.classList.add('active');
  renderPlanet();
}

function renderPlanet(){
  const p = planets[currentPlanet];
  document.getElementById('planet-name').textContent = p.name;
  document.getElementById('planet-desc').textContent = p.desc;
  document.getElementById('planet-dist').textContent = p.dist.toLocaleString('es-ES');
  document.getElementById('planet-orbit').textContent = `Período orbital: ${p.period}`;

  const distPct = Math.min(95, (p.dist / 4515) * 95 + 3);
  document.getElementById('dist-bar').style.width = distPct + '%';

  const badges = document.getElementById('planet-badges');
  badges.innerHTML = `
    <span class="badge ${p.badges[0]}">${p.badges[1]}</span>
    <span class="badge badge-pink">${p.moons} lunas</span>
    <span class="badge badge-green">Temp: ${p.temp}</span>
  `;

  const data = document.getElementById('planet-data');
  data.innerHTML = `
    <div class="data-row"><span class="data-key">Distancia al Sol</span><span class="data-val">${p.dist.toLocaleString()} M km</span></div>
    <div class="data-row"><span class="data-key">Temperatura</span><span class="data-val">${p.temp}</span></div>
    <div class="data-row"><span class="data-key">Lunas</span><span class="data-val">${p.moons}</span></div>
    <div class="data-row"><span class="data-key">Período orbital</span><span class="data-val">${p.period}</span></div>
  `;

  drawPlanetCanvas(p);
}

function drawPlanetCanvas(p){
  const canvas = document.getElementById('planetCanvas');
  const c = canvas.getContext('2d');
  c.clearRect(0,0,canvas.width,canvas.height);
  const cx = 75, cy = 75;
  const r = Math.min(p.size, 60);

  const grad = c.createRadialGradient(cx-r*0.3, cy-r*0.3, r*0.1, cx, cy, r);
  grad.addColorStop(0, '#fff');
  grad.addColorStop(0.3, p.color);
  grad.addColorStop(1, '#000');
  c.beginPath(); c.arc(cx, cy, r, 0, Math.PI*2);
  c.fillStyle = grad;
  c.shadowBlur = 30; c.shadowColor = p.glow;
  c.fill(); c.shadowBlur = 0;

  if(p.ring){
    c.save();
    c.translate(cx,cy);
    c.scale(1, 0.3);
    c.beginPath();
    c.arc(0, 0, r*1.6, 0, Math.PI*2);
    c.strokeStyle = p.color;
    c.lineWidth = 5; c.globalAlpha = 0.5;
    c.stroke(); c.restore();
  }

  // clouds for Earth
  if(p.name === 'Tierra'){
    c.globalAlpha = 0.3;
    c.fillStyle = '#fff';
    c.beginPath(); c.ellipse(cx-10,cy-15,18,8,0.3,0,Math.PI*2); c.fill();
    c.beginPath(); c.ellipse(cx+15,cy+10,12,5,0.5,0,Math.PI*2); c.fill();
    c.globalAlpha = 1;
  }
}

// ============ ORBIT CANVAS ============
function initOrbitCanvas(){
  const canvas = document.getElementById('orbitCanvas');
  const c = canvas.getContext('2d');
  const w = canvas.width, h = canvas.height;
  let t = 0;

  function draw(){
    c.clearRect(0,0,w,h);
    const sunX = 50, sunY = h/2, maxR = 35;
    const scales = [57.9,108.2,149.6,227.9,778.5,1432,2867,4515];
    const maxD = 4515;

    // Sun
    const sg = c.createRadialGradient(sunX,sunY,2,sunX,sunY,14);
    sg.addColorStop(0,'#fff');sg.addColorStop(0.5,'#ffb300');sg.addColorStop(1,'transparent');
    c.beginPath(); c.arc(sunX,sunY,14,0,Math.PI*2);
    c.fillStyle=sg; c.fill();

    planets.forEach((p,i)=>{
      const px = 80 + ((scales[i]/maxD)*(w-100));
      const speed = 0.3/(i+1);
      const dy = Math.sin(t*speed + i)*20*(1/(i*0.3+1));
      const oy = h/2 + dy;

      // orbit line
      c.beginPath(); c.moveTo(sunX,sunY); c.lineTo(px, oy);
      c.strokeStyle='rgba(255,255,255,0.03)'; c.lineWidth=1; c.stroke();

      const pr = Math.max(3, p.size/15);
      const grad = c.createRadialGradient(px-pr*0.3,oy-pr*0.3,1,px,oy,pr);
      grad.addColorStop(0,'#fff');
      grad.addColorStop(0.4,p.color);
      grad.addColorStop(1,'#000');

      c.beginPath(); c.arc(px,oy,pr,0,Math.PI*2);
      c.fillStyle=grad;
      c.shadowBlur=15; c.shadowColor=p.glow;
      c.fill(); c.shadowBlur=0;

      // label
      c.fillStyle='rgba(255,255,255,0.5)';
      c.font='9px Space Mono, monospace';
      c.fillText(p.name.substring(0,3),px-12,oy-pr-5);
    });

    t += 0.02;
    requestAnimationFrame(draw);
  }
  draw();
}

setTimeout(()=>{renderPlanet(); initOrbitCanvas();},100);

// ============ WEATHER ============
function initWaterCycle(){
  const canvas = document.getElementById('waterCycleCanvas');
  const c = canvas.getContext('2d');
  const w=canvas.width, h=canvas.height;
  let t=0;

  const labels = [
    {x:500,y:20,text:'Condensación',color:'#90caf9'},
    {x:100,y:20,text:'Precipitación',color:'#42a5f5'},
    {x:50,y:150,text:'Escorrentía',color:'#1e88e5'},
    {x:300,y:150,text:'Infiltración',color:'#5c6bc0'},
    {x:480,y:140,text:'Evaporación',color:'#80deea'},
    {x:200,y:30,text:'Transpiración',color:'#aed581'},
  ];

  function draw(){
    c.clearRect(0,0,w,h);

    // Sky gradient
    const sky = c.createLinearGradient(0,0,0,h);
    sky.addColorStop(0,'rgba(13,30,60,0.8)');
    sky.addColorStop(1,'rgba(30,60,100,0.3)');
    c.fillStyle=sky; c.fillRect(0,0,w,h);

    // Ground
    const ground = c.createLinearGradient(0,h*0.65,0,h);
    ground.addColorStop(0,'rgba(66,160,75,0.4)');
    ground.addColorStop(1,'rgba(30,80,30,0.2)');
    c.fillStyle=ground; c.fillRect(0,h*0.65,w,h*0.35);

    // Cloud
    const cx2 = 450+Math.sin(t*0.3)*15;
    c.fillStyle='rgba(255,255,255,0.85)';
    [cx2,cx2-28,cx2+28,cx2-10,cx2+15].forEach((x,i)=>{
      c.beginPath(); c.arc(x,30+[0,12,10,18,15][i],[22,14,16,12,14][i],0,Math.PI*2); c.fill();
    });

    // Rain drops
    for(let i=0;i<6;i++){
      const rx=(cx2-40)+i*15;
      const ry=55 + ((t*80+i*30)%100);
      c.strokeStyle='rgba(100,180,255,0.8)'; c.lineWidth=2;
      c.beginPath(); c.moveTo(rx,ry); c.lineTo(rx-2,ry+10); c.stroke();
    }

    // Water body (lake)
    const wg=c.createLinearGradient(0,h*0.67,0,h);
    wg.addColorStop(0,'rgba(30,136,229,0.7)');
    wg.addColorStop(1,'rgba(13,71,161,0.7)');
    c.fillStyle=wg;
    c.beginPath(); c.ellipse(200,h*0.75,120,30,0,0,Math.PI*2); c.fill();

    // Wave on lake
    c.strokeStyle='rgba(255,255,255,0.3)'; c.lineWidth=2;
    c.beginPath();
    for(let x=80;x<320;x+=2){
      const yw = h*0.73+Math.sin(x*0.1+t)*3;
      x===80?c.moveTo(x,yw):c.lineTo(x,yw);
    }
    c.stroke();

    // Evaporation arrows
    for(let i=0;i<4;i++){
      const ex=150+i*25, ey=h*0.65-((t*40+i*30)%80);
      c.strokeStyle=`rgba(128,222,234,${1-((t*0.5+i*0.25)%1)})`;
      c.lineWidth=2;
      c.beginPath(); c.moveTo(ex,h*0.65); c.lineTo(ex,ey); c.stroke();
      c.fillStyle=c.strokeStyle;
      c.beginPath(); c.moveTo(ex-4,ey); c.lineTo(ex,ey-8); c.lineTo(ex+4,ey); c.fill();
    }

    // Tree
    c.fillStyle='rgba(60,120,40,0.8)';
    c.beginPath(); c.moveTo(370,h*0.65); c.lineTo(380,h*0.35); c.lineTo(390,h*0.65); c.fill();
    c.fillStyle='rgba(80,40,20,0.8)';
    c.fillRect(377,h*0.65,6,30);

    // Labels
    labels.forEach(l=>{
      c.fillStyle=l.color; c.font='bold 11px Syne, sans-serif';
      c.fillText(l.text,l.x,l.y);
    });

    t+=0.016;
    requestAnimationFrame(draw);
  }
  draw();

  // Atmosphere layers
  const layers = [
    {name:'Exosfera',height:'700+ km',color:'rgba(79,195,247,0.1)'},
    {name:'Termosfera',height:'80–700 km',color:'rgba(79,195,247,0.15)'},
    {name:'Mesosfera',height:'50–80 km',color:'rgba(79,195,247,0.2)'},
    {name:'Estratosfera',height:'12–50 km',color:'rgba(79,195,247,0.25)'},
    {name:'Troposfera',height:'0–12 km',color:'rgba(66,165,245,0.3)'},
  ];
  document.getElementById('atmo-layers').innerHTML = layers.map(l=>`
    <div style="padding:0.5rem;margin-bottom:4px;border-radius:6px;background:${l.color};border-left:3px solid rgba(79,195,247,0.4);">
      <div style="font-weight:700;font-size:0.85rem;">${l.name}</div>
      <div style="font-size:0.75rem;color:var(--muted);font-family:'Space Mono',monospace;">${l.height}</div>
    </div>
  `).join('');

  const phenomena = [
    {icon:'🌪',name:'Tornado',desc:'Vientos giratorios hasta 500 km/h'},
    {icon:'⛈',name:'Tormenta eléctrica',desc:'3 millones ocurren diariamente'},
    {icon:'🌊',name:'Tsunami',desc:'Olas que viajan a 800 km/h'},
    {icon:'🌈',name:'Aurora boreal',desc:'Colisión de partículas solares'},
  ];
  document.getElementById('weather-phenomena').innerHTML = phenomena.map(p=>`
    <div style="display:flex;align-items:center;gap:0.8rem;padding:0.5rem 0;border-bottom:1px solid rgba(255,255,255,0.05);">
      <span style="font-size:1.5rem;">${p.icon}</span>
      <div>
        <div style="font-weight:700;font-size:0.85rem;">${p.name}</div>
        <div style="font-size:0.75rem;color:var(--muted);">${p.desc}</div>
      </div>
    </div>
  `).join('');
}

function initTempChart(){
  const canvas = document.getElementById('tempChart');
  const c = canvas.getContext('2d');
  const w=canvas.width, h=canvas.height;

  const data = [-0.16,-0.09,-0.19,-0.29,-0.35,-0.14,-0.15,0.02,-0.05,-0.15,
                -0.1,-0.01,-0.1,-0.19,-0.29,-0.12,0.04,0.1,0.08,-0.02,
                0.08,0.2,0.3,0.35,0.14,0.25,0.18,0.27,0.4,0.37,
                0.45,0.55,0.6,0.65,0.58,0.7,0.75,0.8,0.9,0.85,
                0.98,1.02,1.1,1.15,1.02];

  const pad=40, chartW=w-2*pad, chartH=h-2*pad;
  const min=-0.5, max=1.3;

  c.fillStyle='rgba(0,0,0,0)'; c.fillRect(0,0,w,h);

  // Grid
  c.strokeStyle='rgba(255,255,255,0.05)'; c.lineWidth=1;
  for(let i=0;i<=4;i++){
    const y=pad+chartH*(1-i/4);
    c.beginPath(); c.moveTo(pad,y); c.lineTo(pad+chartW,y); c.stroke();
    c.fillStyle='rgba(255,255,255,0.3)'; c.font='10px Space Mono';
    c.fillText(((min+(max-min)*i/4)).toFixed(1)+'°',2,y+4);
  }

  // Zero line
  const zeroY = pad + chartH*(1-(-min)/(max-min));
  c.strokeStyle='rgba(255,255,255,0.2)'; c.setLineDash([4,4]);
  c.beginPath(); c.moveTo(pad,zeroY); c.lineTo(pad+chartW,zeroY); c.stroke();
  c.setLineDash([]);

  // Area fill
  const gradient = c.createLinearGradient(0,pad,0,pad+chartH);
  gradient.addColorStop(0,'rgba(239,83,80,0.6)');
  gradient.addColorStop(0.5,'rgba(79,195,247,0.1)');
  gradient.addColorStop(1,'rgba(79,195,247,0)');

  c.beginPath();
  data.forEach((v,i)=>{
    const x=pad+i*chartW/(data.length-1);
    const y=pad+chartH*(1-(v-min)/(max-min));
    i===0?c.moveTo(x,y):c.lineTo(x,y);
  });
  const lastX=pad+chartW, lastY=pad+chartH*(1-(-0.5-min)/(max-min));
  c.lineTo(lastX,zeroY); c.lineTo(pad,zeroY);
  c.fillStyle=gradient; c.fill();

  // Line
  const lg=c.createLinearGradient(pad,0,pad+chartW,0);
  lg.addColorStop(0,'#42a5f5'); lg.addColorStop(0.6,'#ffb74d'); lg.addColorStop(1,'#ef5350');
  c.beginPath();
  data.forEach((v,i)=>{
    const x=pad+i*chartW/(data.length-1);
    const y=pad+chartH*(1-(v-min)/(max-min));
    i===0?c.moveTo(x,y):c.lineTo(x,y);
  });
  c.strokeStyle=lg; c.lineWidth=2.5; c.stroke();

  // X labels
  c.fillStyle='rgba(255,255,255,0.4)'; c.font='10px Space Mono';
  ['1880','1900','1920','1940','1960','1980','2000','2024'].forEach((yr,i)=>{
    c.fillText(yr, pad + (i/7)*chartW - 10, h-4);
  });
}

// ============ SCIENCE ============
const elements = [
  {s:'H',name:'Hidrógeno',n:1,m:'1.008',desc:'El elemento más abundante del universo. Forma el 75% de la masa elemental normal.',e:1},
  {s:'He',name:'Helio',n:2,m:'4.003',desc:'Gas noble usado en globos y cohetes. Es el segundo elemento más abundante del cosmos.',e:2},
  {s:'C',name:'Carbono',n:6,m:'12.011',desc:'La base de toda vida. Puede formar millones de compuestos diferentes: el elemento más versátil.',e:6},
  {s:'O',name:'Oxígeno',n:8,m:'15.999',desc:'El tercer elemento más abundante del universo. Esencial para la respiración y la combustión.',e:8},
  {s:'Fe',name:'Hierro',n:26,m:'55.845',desc:'El elemento más abundante en el núcleo de la Tierra. Las estrellas muertas colapsan al producirlo.',e:26},
  {s:'Au',name:'Oro',n:79,m:'196.97',desc:'Metal precioso formado en colisiones de estrellas de neutrones. Toda la Tierra tiene muy poco.',e:79},
  {s:'U',name:'Uranio',n:92,m:'238.03',desc:'El elemento natural más pesado. Usado en energía nuclear, tiene una vida media de 4.5 mil millones de años.',e:92},
];
let currentEl = 0;

function nextElement(){
  currentEl = (currentEl+1) % elements.length;
  const el = elements[currentEl];
  document.getElementById('el-symbol').textContent = el.s;
  document.getElementById('el-name').textContent = el.name;
  document.getElementById('el-num').textContent = `Número atómico: ${el.n}`;
  document.getElementById('el-mass').textContent = `Masa atómica: ${el.m} u`;
  document.getElementById('el-desc').textContent = el.desc;
  drawAtom(el.e);
}

function initAtomCanvas(){
  drawAtom(1);
}

function drawAtom(electrons){
  const canvas = document.getElementById('atomCanvas');
  const c = canvas.getContext('2d');
  const w=canvas.width, h=canvas.height;
  const cx=w/2, cy=h/2;
  let t = 0;

  function frame(){
    c.clearRect(0,0,w,h);

    // Nucleus
    const ng=c.createRadialGradient(cx,cy,2,cx,cy,12);
    ng.addColorStop(0,'#fff');ng.addColorStop(0.5,'#ef5350');ng.addColorStop(1,'#b71c1c');
    c.beginPath(); c.arc(cx,cy,12,0,Math.PI*2);
    c.fillStyle=ng; c.shadowBlur=20; c.shadowColor='#ef5350'; c.fill(); c.shadowBlur=0;

    // Shells
    const shells = electrons<=2?[Math.min(electrons,2)]
      :electrons<=10?[2,electrons-2]
      :electrons<=18?[2,8,electrons-10]
      :[2,8,18,electrons-28];

    shells.forEach((count,si)=>{
      const r = 30 + si*30;
      c.beginPath(); c.arc(cx,cy,r,0,Math.PI*2);
      c.strokeStyle='rgba(79,195,247,0.2)'; c.lineWidth=1; c.stroke();

      for(let ei=0;ei<count;ei++){
        const angle = t*(1/(si+1)) + (ei/count)*Math.PI*2;
        const ex=cx+r*Math.cos(angle), ey=cy+r*Math.sin(angle);
        const eg=c.createRadialGradient(ex,ey,1,ex,ey,5);
        eg.addColorStop(0,'#fff'); eg.addColorStop(1,'#4fc3f7');
        c.beginPath(); c.arc(ex,ey,5,0,Math.PI*2);
        c.fillStyle=eg; c.shadowBlur=10; c.shadowColor='#4fc3f7'; c.fill(); c.shadowBlur=0;
      }
    });

    t+=0.03;
    requestAnimationFrame(frame);
  }
  frame();
}

let scaleIdx=0;
const scaleItems=[
  {label:'Átomo',size:'0.1 nm',val:0.05},
  {label:'Bacteria',size:'1 µm',val:0.1},
  {label:'Mosca',size:'5 mm',val:0.18},
  {label:'Humano',size:'1.8 m',val:0.28},
  {label:'Monte Everest',size:'8.8 km',val:0.4},
  {label:'Tierra',size:'12,742 km',val:0.55},
  {label:'Sol',size:'1.4 M km',val:0.7},
  {label:'Sistema Solar',size:'30 UA',val:0.82},
  {label:'Vía Láctea',size:'100,000 al',val:0.92},
  {label:'Universo Observable',size:'93 mil M al',val:1.0},
];

function initScaleCanvas(){ drawScale(); }
function animateScale(){
  scaleIdx=(scaleIdx+1)%scaleItems.length;
  drawScale();
}

function drawScale(){
  const canvas=document.getElementById('scaleCanvas');
  const c=canvas.getContext('2d');
  const w=canvas.width, h=canvas.height;
  c.clearRect(0,0,w,h);

  // Background
  const bg=c.createLinearGradient(0,0,w,0);
  bg.addColorStop(0,'rgba(6,13,26,0)');bg.addColorStop(0.5,'rgba(6,13,26,0.5)');bg.addColorStop(1,'rgba(6,13,26,0)');
  c.fillStyle=bg;c.fillRect(0,0,w,h);

  const trackY=h/2+20, trackH=8;
  const trackBg=c.createLinearGradient(0,0,w,0);
  trackBg.addColorStop(0,'#4fc3f7');trackBg.addColorStop(0.5,'#f06292');trackBg.addColorStop(1,'#aed581');
  c.fillStyle='rgba(255,255,255,0.06)';
  c.beginPath();c.roundRect(30,trackY,w-60,trackH,4);c.fill();

  const filled=(scaleItems[scaleIdx].val*(w-60));
  c.fillStyle=trackBg;
  c.beginPath();c.roundRect(30,trackY,filled,trackH,4);c.fill();

  // Notches
  scaleItems.forEach((it,i)=>{
    const x=30+it.val*(w-60);
    c.fillStyle= i<=scaleIdx ?'rgba(255,255,255,0.8)':'rgba(255,255,255,0.2)';
    c.fillRect(x-1,trackY-4,2,trackH+8);
  });

  // Thumb
  const thumbX=30+scaleItems[scaleIdx].val*(w-60);
  const tg=c.createRadialGradient(thumbX,trackY+4,2,thumbX,trackY+4,12);
  tg.addColorStop(0,'#fff');tg.addColorStop(1,'#4fc3f7');
  c.beginPath();c.arc(thumbX,trackY+4,12,0,Math.PI*2);
  c.fillStyle=tg;c.shadowBlur=20;c.shadowColor='#4fc3f7';c.fill();c.shadowBlur=0;

  const item=scaleItems[scaleIdx];
  c.fillStyle='#fff';c.font='bold 20px Syne, sans-serif';
  c.fillText(item.label,thumbX-50,trackY-30>20?trackY-30:30);
  c.fillStyle='rgba(255,255,255,0.5)';c.font='12px Space Mono';
  c.fillText(item.size,thumbX-30,trackY+35);
}

function initEnergyCanvas(){
  const canvas=document.getElementById('energyCanvas');
  const c=canvas.getContext('2d');
  const w=canvas.width,h=canvas.height;
  const types=[
    {name:'Cinética',pct:30,color:'#4fc3f7'},
    {name:'Potencial',pct:25,color:'#f06292'},
    {name:'Térmica',pct:20,color:'#ffb74d'},
    {name:'Química',pct:15,color:'#aed581'},
    {name:'Nuclear',pct:10,color:'#ce93d8'},
  ];
  let start=-Math.PI/2;
  const cx=w/2, cy=h/2-10, r=70;
  types.forEach(t=>{
    const angle=(t.pct/100)*Math.PI*2;
    c.beginPath();c.moveTo(cx,cy);
    c.arc(cx,cy,r,start,start+angle);
    c.fillStyle=t.color;c.shadowBlur=10;c.shadowColor=t.color;
    c.fill();c.shadowBlur=0;
    c.strokeStyle='rgba(0,0,0,0.5)';c.lineWidth=2;c.stroke();
    // label
    const lx=cx+(r+25)*Math.cos(start+angle/2), ly=cy+(r+25)*Math.sin(start+angle/2);
    c.fillStyle=t.color;c.font='bold 10px Syne';
    c.fillText(t.name,lx-25,ly+4);
    start+=angle;
  });
  // Center
  c.beginPath();c.arc(cx,cy,30,0,Math.PI*2);
  c.fillStyle='rgba(6,13,26,0.9)';c.fill();
  c.fillStyle='rgba(255,255,255,0.7)';c.font='10px Space Mono';
  c.fillText('ENERGÍA',cx-24,cy+4);
}

// ============ QUIZ ============
const questions=[
  {q:'¿Cuál es el planeta más grande del Sistema Solar?',opts:['Saturno','Júpiter','Neptuno','Urano'],ans:1,exp:'Júpiter es tan grande que todos los planetas del Sistema Solar cabrían dentro de él. Su masa es 318 veces la de la Tierra.',cat:'Astronomía'},
  {q:'¿A qué velocidad viaja la luz en el vacío?',opts:['150,000 km/s','299,792 km/s','500,000 km/s','1,000 km/s'],ans:1,exp:'La velocidad de la luz (c) es exactamente 299,792.458 km/s. En 1 segundo, recorre 7.5 vueltas alrededor de la Tierra.',cat:'Física'},
  {q:'¿Cuál es el elemento más abundante en el universo?',opts:['Helio','Oxígeno','Carbono','Hidrógeno'],ans:3,exp:'El Hidrógeno forma aproximadamente el 75% de la masa barionica del universo. Es el combustible de las estrellas.',cat:'Ciencia'},
  {q:'¿Cuántas capas tiene la atmósfera terrestre?',opts:['3','4','5','6'],ans:2,exp:'La atmósfera tiene 5 capas: troposfera, estratosfera, mesosfera, termosfera y exosfera. La troposfera es donde vivimos.',cat:'Clima'},
  {q:'¿Qué planeta tiene los anillos más espectaculares?',opts:['Júpiter','Urano','Saturno','Neptuno'],ans:2,exp:'Saturno tiene el sistema de anillos más extenso y visible. Están compuestos principalmente de hielo y roca.',cat:'Astronomía'},
  {q:'¿Qué es el CO₂?',opts:['Monóxido de carbono','Dióxido de carbono','Cloruro de calcio','Carbonato de calcio'],ans:1,exp:'El dióxido de carbono (CO₂) es el principal gas de efecto invernadero. Su concentración actual es de 422 ppm, la más alta en 800,000 años.',cat:'Clima'},
  {q:'¿Cuál es la estrella más cercana al Sol?',opts:['Sirio','Vega','Próxima Centauri','Betelgeuse'],ans:2,exp:'Próxima Centauri está a 4.246 años luz del Sol. Aunque cerca en términos cósmicos, un viaje en cohete tardaría 70,000 años.',cat:'Astronomía'},
  {q:'¿Cuántos planetas tiene el Sistema Solar?',opts:['7','8','9','10'],ans:1,exp:'El Sistema Solar tiene 8 planetas: Mercurio, Venus, Tierra, Marte, Júpiter, Saturno, Urano y Neptuno. Plutón fue reclasificado como planeta enano en 2006.',cat:'Astronomía'},
];

let qIdx=0,score=0,answered=false;

function initQuiz(){
  qIdx=0;score=0;
  document.getElementById('quiz-result').style.display='none';
  document.getElementById('quiz-next').style.display='none';
  showQuestion();
}

function showQuestion(){
  const q=questions[qIdx];
  document.getElementById('quiz-category').textContent=q.cat;
  document.getElementById('quiz-q').textContent=q.q;
  document.getElementById('quiz-score').textContent=score;
  document.getElementById('quiz-total').textContent=qIdx;
  document.getElementById('quiz-feedback').style.display='none';
  document.getElementById('quiz-next').style.display='none';
  answered=false;

  const opts=document.getElementById('quiz-options');
  opts.innerHTML=q.opts.map((o,i)=>`
    <button class="quiz-btn" onclick="checkAnswer(${i})">${o}</button>
  `).join('');
}

function checkAnswer(i){
  if(answered)return;
  answered=true;
  const q=questions[qIdx];
  const btns=document.querySelectorAll('.quiz-btn');
  btns[q.ans].classList.add('correct');
  if(i!==q.ans) btns[i].classList.add('wrong');
  else score++;

  const fb=document.getElementById('quiz-feedback');
  fb.style.display='block';
  fb.style.background=i===q.ans?'rgba(174,213,129,0.1)':'rgba(240,98,146,0.1)';
  fb.style.border=`1px solid ${i===q.ans?'rgba(174,213,129,0.3)':'rgba(240,98,146,0.3)'}`;
  fb.innerHTML=`<strong>${i===q.ans?'✅ ¡Correcto!':'❌ Incorrecto'}</strong><br>${q.exp}`;

  document.getElementById('quiz-next').style.display=qIdx<questions.length-1?'inline-block':'none';
  if(qIdx===questions.length-1){
    setTimeout(()=>{
      document.getElementById('quiz-result').style.display='block';
      document.getElementById('final-score').textContent=`${score} / ${questions.length}`;
    },1500);
  }
}

function nextQuestion(){
  qIdx++;
  showQuestion();
}

function restartQuiz(){ initQuiz(); }

// Init
setTimeout(()=>{
  renderPlanet();
  initOrbitCanvas();
},200);
</script>
</body>
</html>
