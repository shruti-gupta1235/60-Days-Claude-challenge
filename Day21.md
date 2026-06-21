<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Digital Privacy Dashboard</title>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@tabler/icons-webfont@2.44.0/tabler-icons.min.css">
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js"></script>
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --bg: #0a0b0f;
    --bg2: #111318;
    --bg3: #181c24;
    --bg4: #1e2330;
    --border: rgba(255,255,255,0.07);
    --border2: rgba(255,255,255,0.13);
    --text: #f0f2f7;
    --text2: #8a90a0;
    --text3: #555c70;
    --accent: #4f6ef7;
    --accent2: #7c3aed;
    --red: #ef4444;
    --orange: #f97316;
    --yellow: #eab308;
    --green: #22c55e;
    --teal: #14b8a6;
    --purple: #a855f7;
    --pink: #ec4899;
  }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', system-ui, sans-serif;
    font-size: 14px;
    line-height: 1.6;
    min-height: 100vh;
    padding: 0;
  }

  /* Header */
  .header {
    background: var(--bg2);
    border-bottom: 1px solid var(--border);
    padding: 0 2rem;
    display: flex;
    align-items: center;
    justify-content: space-between;
    height: 56px;
    position: sticky;
    top: 0;
    z-index: 100;
  }
  .header-brand {
    display: flex;
    align-items: center;
    gap: 10px;
    font-size: 15px;
    font-weight: 600;
    letter-spacing: -0.01em;
  }
  .header-brand .logo {
    width: 28px; height: 28px;
    background: linear-gradient(135deg, #4f6ef7, #7c3aed);
    border-radius: 8px;
    display: flex; align-items: center; justify-content: center;
    font-size: 14px; color: #fff;
  }
  .header-meta {
    display: flex;
    gap: 1rem;
    align-items: center;
    font-size: 12px;
    color: var(--text2);
  }
  .badge-fact {
    background: rgba(20,184,166,0.15);
    color: var(--teal);
    border: 1px solid rgba(20,184,166,0.3);
    border-radius: 20px;
    padding: 2px 10px;
    font-size: 11px;
    font-weight: 600;
    letter-spacing: 0.04em;
  }
  .badge-est {
    background: rgba(234,179,8,0.15);
    color: var(--yellow);
    border: 1px solid rgba(234,179,8,0.3);
    border-radius: 20px;
    padding: 2px 10px;
    font-size: 11px;
    font-weight: 600;
    letter-spacing: 0.04em;
  }

  /* Layout */
  .dashboard {
    max-width: 1280px;
    margin: 0 auto;
    padding: 2rem;
    display: flex;
    flex-direction: column;
    gap: 1.5rem;
  }

  /* Alert banner */
  .alert-banner {
    background: rgba(239,68,68,0.08);
    border: 1px solid rgba(239,68,68,0.25);
    border-radius: 12px;
    padding: 1rem 1.25rem;
    display: flex;
    align-items: center;
    gap: 12px;
    font-size: 13px;
    color: #fca5a5;
  }
  .alert-banner i { font-size: 18px; color: var(--red); flex-shrink: 0; }

  /* Card base */
  .card {
    background: var(--bg2);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 1.5rem;
  }
  .card-sm { padding: 1.25rem; border-radius: 12px; }
  .card-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 1.25rem;
  }
  .card-title {
    font-size: 13px;
    font-weight: 600;
    color: var(--text2);
    letter-spacing: 0.05em;
    text-transform: uppercase;
  }
  .card-badge {
    font-size: 11px;
    color: var(--text3);
    background: var(--bg3);
    border: 1px solid var(--border);
    border-radius: 6px;
    padding: 2px 8px;
  }

  /* Grid layouts */
  .grid-5 { display: grid; grid-template-columns: repeat(5, 1fr); gap: 1rem; }
  .grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 1.5rem; }
  .grid-3 { display: grid; grid-template-columns: repeat(3, 1fr); gap: 1.5rem; }
  .grid-2-1 { display: grid; grid-template-columns: 2fr 1fr; gap: 1.5rem; }
  .grid-1-2 { display: grid; grid-template-columns: 1fr 2fr; gap: 1.5rem; }

  /* Score hero */
  .score-hero {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 0.5rem;
  }
  .score-ring-wrap { position: relative; width: 140px; height: 140px; }
  .score-ring-wrap svg { width: 140px; height: 140px; transform: rotate(-90deg); }
  .score-num {
    position: absolute;
    inset: 0;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 2px;
  }
  .score-num span:first-child { font-size: 36px; font-weight: 700; letter-spacing: -0.03em; }
  .score-num span:last-child { font-size: 11px; color: var(--text2); font-weight: 500; letter-spacing: 0.03em; text-transform: uppercase; }
  .score-label { font-size: 13px; font-weight: 600; margin-top: 4px; }
  .score-desc { font-size: 12px; color: var(--text2); text-align: center; max-width: 160px; }

  /* Stat tiles */
  .stat-tile {
    background: var(--bg3);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 1rem 1.1rem;
  }
  .stat-tile .label { font-size: 11px; color: var(--text3); font-weight: 600; letter-spacing: 0.04em; text-transform: uppercase; margin-bottom: 6px; }
  .stat-tile .value { font-size: 26px; font-weight: 700; letter-spacing: -0.03em; line-height: 1; }
  .stat-tile .sub { font-size: 11px; color: var(--text2); margin-top: 4px; }

  /* Company table */
  .company-row {
    display: flex;
    align-items: center;
    gap: 12px;
    padding: 10px 0;
    border-bottom: 1px solid var(--border);
  }
  .company-row:last-child { border-bottom: none; }
  .company-icon {
    width: 36px; height: 36px;
    border-radius: 10px;
    display: flex; align-items: center; justify-content: center;
    font-size: 16px; flex-shrink: 0;
    font-weight: 700;
  }
  .company-info { flex: 1; min-width: 0; }
  .company-name { font-size: 13px; font-weight: 600; }
  .company-apps { font-size: 11px; color: var(--text2); }
  .exposure-bar-wrap { width: 120px; }
  .exposure-bar-bg {
    height: 6px;
    background: var(--bg4);
    border-radius: 99px;
    overflow: hidden;
  }
  .exposure-bar-fill { height: 100%; border-radius: 99px; }
  .exposure-pct { font-size: 12px; font-weight: 600; margin-top: 3px; text-align: right; }
  .risk-pill {
    font-size: 11px;
    font-weight: 700;
    border-radius: 6px;
    padding: 3px 8px;
    letter-spacing: 0.03em;
    white-space: nowrap;
  }

  /* Heatmap */
  .heatmap-grid {
    display: grid;
    grid-template-columns: repeat(6, 1fr);
    gap: 4px;
  }
  .heatmap-cell {
    border-radius: 8px;
    padding: 10px 8px;
    display: flex;
    flex-direction: column;
    gap: 4px;
    cursor: default;
    transition: transform 0.1s;
  }
  .heatmap-cell:hover { transform: scale(1.04); }
  .heatmap-cell .app-name { font-size: 11px; font-weight: 600; }
  .heatmap-cell .app-co { font-size: 10px; opacity: 0.7; }
  .heatmap-cell .app-score { font-size: 16px; font-weight: 700; }

  /* Matrix */
  .matrix-table { width: 100%; border-collapse: collapse; }
  .matrix-table th {
    font-size: 10px;
    font-weight: 600;
    color: var(--text3);
    text-transform: uppercase;
    letter-spacing: 0.05em;
    padding: 8px 10px;
    text-align: center;
    border-bottom: 1px solid var(--border);
  }
  .matrix-table th:first-child { text-align: left; }
  .matrix-table td {
    padding: 8px 10px;
    text-align: center;
    border-bottom: 1px solid var(--border);
    font-size: 12px;
  }
  .matrix-table td:first-child { text-align: left; font-weight: 500; font-size: 12px; }
  .matrix-table tr:last-child td { border-bottom: none; }
  .dot {
    width: 18px; height: 18px;
    border-radius: 50%;
    display: inline-flex; align-items: center; justify-content: center;
    font-size: 9px; font-weight: 700;
    margin: auto;
  }
  .dot-yes { background: rgba(239,68,68,0.2); color: var(--red); border: 1px solid rgba(239,68,68,0.4); }
  .dot-maybe { background: rgba(234,179,8,0.15); color: var(--yellow); border: 1px solid rgba(234,179,8,0.35); }
  .dot-no { background: rgba(34,197,94,0.12); color: var(--green); border: 1px solid rgba(34,197,94,0.3); }

  /* Radar */
  .radar-wrap { position: relative; width: 100%; }
  .radar-legend { display: flex; flex-wrap: wrap; gap: 8px; margin-top: 1rem; }
  .radar-legend-item { display: flex; align-items: center; gap: 6px; font-size: 12px; color: var(--text2); }
  .radar-dot { width: 8px; height: 8px; border-radius: 50%; }

  /* Digital twin */
  .twin-section { margin-bottom: 1.25rem; }
  .twin-label { font-size: 11px; color: var(--text3); font-weight: 600; text-transform: uppercase; letter-spacing: 0.05em; margin-bottom: 8px; }
  .twin-tags { display: flex; flex-wrap: wrap; gap: 6px; }
  .twin-tag {
    font-size: 12px;
    border-radius: 8px;
    padding: 4px 10px;
    font-weight: 500;
  }
  .tag-fact { background: rgba(20,184,166,0.1); color: #5eead4; border: 1px solid rgba(20,184,166,0.25); }
  .tag-est { background: rgba(234,179,8,0.1); color: #fde68a; border: 1px solid rgba(234,179,8,0.2); }

  /* Asset cards */
  .asset-card {
    background: var(--bg3);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 1rem;
    display: flex;
    flex-direction: column;
    gap: 8px;
  }
  .asset-header { display: flex; align-items: center; gap: 8px; }
  .asset-icon { font-size: 20px; width: 36px; height: 36px; display: flex; align-items: center; justify-content: center; border-radius: 10px; }
  .asset-name { font-size: 13px; font-weight: 600; }
  .asset-value-label { font-size: 11px; color: var(--text3); }
  .asset-value { font-size: 22px; font-weight: 700; letter-spacing: -0.02em; }
  .asset-desc { font-size: 11px; color: var(--text2); line-height: 1.5; }

  /* Simulator */
  .sim-row { display: flex; align-items: center; gap: 12px; padding: 10px 0; border-bottom: 1px solid var(--border); }
  .sim-row:last-child { border-bottom: none; }
  .sim-action { flex: 1; font-size: 13px; }
  .sim-action .action-title { font-weight: 500; }
  .sim-action .action-sub { font-size: 11px; color: var(--text2); margin-top: 2px; }
  .sim-gain { font-size: 13px; font-weight: 700; color: var(--green); width: 70px; text-align: right; flex-shrink: 0; }
  .sim-diff { font-size: 11px; color: var(--text2); }
  .toggle-btn {
    width: 40px; height: 22px;
    background: var(--bg4);
    border-radius: 11px;
    border: 1px solid var(--border2);
    cursor: pointer;
    position: relative;
    transition: background 0.2s;
    flex-shrink: 0;
  }
  .toggle-btn.on { background: rgba(79,110,247,0.8); }
  .toggle-knob {
    position: absolute;
    width: 16px; height: 16px;
    background: #fff;
    border-radius: 50%;
    top: 2px; left: 2px;
    transition: left 0.2s;
    pointer-events: none;
  }
  .toggle-btn.on .toggle-knob { left: 20px; }
  .sim-score-display {
    text-align: center;
    padding: 1.5rem;
    background: var(--bg3);
    border-radius: 12px;
    margin-top: 1rem;
  }
  .sim-score-num { font-size: 48px; font-weight: 800; letter-spacing: -0.04em; }
  .sim-score-label { font-size: 12px; color: var(--text2); margin-top: 4px; }

  /* Verdict */
  .verdict-header {
    display: flex;
    align-items: center;
    gap: 1rem;
    margin-bottom: 1.5rem;
  }
  .verdict-grade {
    width: 72px; height: 72px;
    border-radius: 16px;
    display: flex; align-items: center; justify-content: center;
    font-size: 36px; font-weight: 800;
    flex-shrink: 0;
    background: rgba(239,68,68,0.12);
    border: 2px solid rgba(239,68,68,0.4);
    color: var(--red);
  }
  .verdict-title { font-size: 22px; font-weight: 700; letter-spacing: -0.02em; }
  .verdict-sub { font-size: 13px; color: var(--text2); margin-top: 4px; }
  .verdict-pills { display: flex; flex-wrap: wrap; gap: 8px; margin-top: 1rem; }
  .vpill {
    font-size: 12px;
    border-radius: 8px;
    padding: 5px 12px;
    font-weight: 500;
  }
  .vpill-red { background: rgba(239,68,68,0.12); color: #fca5a5; border: 1px solid rgba(239,68,68,0.25); }
  .vpill-yellow { background: rgba(234,179,8,0.1); color: #fde68a; border: 1px solid rgba(234,179,8,0.2); }
  .vpill-green { background: rgba(34,197,94,0.1); color: #86efac; border: 1px solid rgba(34,197,94,0.2); }

  .section-divider {
    font-size: 11px;
    font-weight: 700;
    color: var(--text3);
    letter-spacing: 0.08em;
    text-transform: uppercase;
    display: flex;
    align-items: center;
    gap: 12px;
  }
  .section-divider::after { content: ''; flex: 1; height: 1px; background: var(--border); }

  /* WOW insight card */
  .wow-card {
    background: var(--bg3);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 1rem 1.25rem;
    display: flex;
    gap: 12px;
    align-items: flex-start;
  }
  .wow-icon { font-size: 20px; flex-shrink: 0; margin-top: 2px; }
  .wow-title { font-size: 13px; font-weight: 600; margin-bottom: 3px; }
  .wow-desc { font-size: 12px; color: var(--text2); line-height: 1.5; }

  .score-tier { display: flex; gap: 4px; margin-top: 12px; }
  .tier-seg {
    height: 4px; flex: 1; border-radius: 99px;
    opacity: 0.3;
  }
  .tier-seg.active { opacity: 1; }

  @media (max-width: 900px) {
    .grid-5 { grid-template-columns: repeat(3, 1fr); }
    .grid-2, .grid-3, .grid-2-1, .grid-1-2 { grid-template-columns: 1fr; }
    .heatmap-grid { grid-template-columns: repeat(3, 1fr); }
  }
</style>
</head>
<body>

<header class="header">
  <div class="header-brand">
    <div class="logo"><i class="ti ti-shield-lock" style="font-size:14px"></i></div>
    PrivacyScope — Digital Footprint Report
  </div>
  <div class="header-meta">
    <span class="badge-fact"><i class="ti ti-check" style="font-size:10px; margin-right:3px"></i>FACT</span>
    <span class="badge-est"><i class="ti ti-sparkles" style="font-size:10px; margin-right:3px"></i>ESTIMATE</span>
    <span style="font-size:11px; color:var(--text3)">Based on 15 reported services</span>
  </div>
</header>

<div class="dashboard">

  <!-- Disclaimer -->
  <div class="alert-banner">
    <i class="ti ti-info-circle"></i>
    <div>
      <strong>Transparency notice:</strong> This dashboard is built entirely from your self-reported list of 15 services. Facts are labeled clearly. All inferences about behaviour, habits, or demographics are <strong>Estimates</strong> based on publicly known data practices of these platforms — never from private databases. No certainty is claimed.
    </div>
  </div>

  <!-- SECTION 1: Scores -->
  <div class="section-divider">Overview scores</div>
  <div class="grid-2-1">
    <!-- Scores side by side -->
    <div class="card" style="display:flex; align-items:center; gap:2rem; flex-wrap:wrap;">
      <div class="score-hero" id="footprint-hero">
        <div class="score-ring-wrap">
          <svg viewBox="0 0 140 140">
            <circle cx="70" cy="70" r="58" fill="none" stroke="rgba(255,255,255,0.06)" stroke-width="12"/>
            <circle cx="70" cy="70" r="58" fill="none" stroke="#f97316" stroke-width="12"
              stroke-dasharray="364" stroke-dashoffset="72" stroke-linecap="round"/>
          </svg>
          <div class="score-num">
            <span style="color:#f97316">80</span>
            <span>Footprint</span>
          </div>
        </div>
        <div class="score-label" style="color:#f97316">🟠 Significant</div>
        <div class="score-desc">You leave a wide trail across apps, platforms, and ecosystems.</div>
        <div class="score-tier">
          <div class="tier-seg" style="background:var(--green)"></div>
          <div class="tier-seg" style="background:var(--yellow)"></div>
          <div class="tier-seg active" style="background:var(--orange)"></div>
          <div class="tier-seg" style="background:var(--red)"></div>
        </div>
      </div>

      <div style="width:1px; height:140px; background:var(--border); flex-shrink:0;"></div>

      <div class="score-hero">
        <div class="score-ring-wrap">
          <svg viewBox="0 0 140 140">
            <circle cx="70" cy="70" r="58" fill="none" stroke="rgba(255,255,255,0.06)" stroke-width="12"/>
            <circle cx="70" cy="70" r="58" fill="none" stroke="#f97316" stroke-width="12"
              stroke-dasharray="364" stroke-dashoffset="255" stroke-linecap="round"/>
          </svg>
          <div class="score-num">
            <span style="color:#f97316">30</span>
            <span>Privacy</span>
          </div>
        </div>
        <div class="score-label" style="color:#f97316">🟠 Fair</div>
        <div class="score-desc">Significant exposure to data collection with limited controls.</div>
        <div class="score-tier">
          <div class="tier-seg active" style="background:var(--red)"></div>
          <div class="tier-seg active" style="background:var(--orange)"></div>
          <div class="tier-seg" style="background:var(--yellow)"></div>
          <div class="tier-seg" style="background:var(--green)"></div>
        </div>
      </div>

      <div style="flex:1; min-width:200px;">
        <div class="twin-label" style="margin-bottom:12px">Score rationale</div>
        <div style="display:flex;flex-direction:column;gap:8px;">
          <div class="wow-card">
            <div class="wow-icon">📊</div>
            <div>
              <div class="wow-title">High footprint drivers</div>
              <div class="wow-desc">15 apps across 4 ecosystems. 3 social video platforms. Dual payment systems. Cross-device presence across Android & iOS.</div>
            </div>
          </div>
          <div class="wow-card">
            <div class="wow-icon">🔒</div>
            <div>
              <div class="wow-title">Privacy score basis</div>
              <div class="wow-desc">No VPN, ad-blockers, or privacy browsers reported. All major platforms have ad-targeting enabled by default.</div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Quick stats -->
    <div style="display:flex;flex-direction:column;gap:1rem;">
      <div class="stat-tile">
        <div class="label">Total services</div>
        <div class="value" style="color:var(--accent)">15</div>
        <div class="sub badge-fact" style="display:inline-block;margin-top:6px">Reported fact</div>
      </div>
      <div class="stat-tile">
        <div class="label">Parent companies</div>
        <div class="value" style="color:var(--purple)">6</div>
        <div class="sub">Meta · Google · Apple · ByteDance · Microsoft · Amazon</div>
      </div>
      <div class="stat-tile">
        <div class="label">Ecosystem concentration</div>
        <div class="value" style="color:var(--orange)">74%</div>
        <div class="sub">Google + Meta control most of your digital life</div>
      </div>
      <div class="stat-tile">
        <div class="label">Est. tracking surface</div>
        <div class="value" style="color:var(--red)">High</div>
        <div class="sub badge-est" style="display:inline-block;margin-top:6px">Estimated</div>
      </div>
    </div>
  </div>

  <!-- SECTION 2: Exposure Heatmap -->
  <div class="section-divider">Exposure heatmap</div>
  <div class="card">
    <div class="card-header">
      <div class="card-title">App exposure intensity</div>
      <div style="display:flex;gap:8px;align-items:center;font-size:11px;color:var(--text2)">
        <span style="display:flex;align-items:center;gap:4px"><span style="width:10px;height:10px;border-radius:3px;background:#1e3a1e;display:inline-block"></span>Low</span>
        <span style="display:flex;align-items:center;gap:4px"><span style="width:10px;height:10px;border-radius:3px;background:#854d0e;display:inline-block"></span>Moderate</span>
        <span style="display:flex;align-items:center;gap:4px"><span style="width:10px;height:10px;border-radius:3px;background:#7f1d1d;display:inline-block"></span>High</span>
        <span class="badge-est">Estimated</span>
      </div>
    </div>
    <div class="heatmap-grid" id="heatmap"></div>
  </div>

  <!-- SECTION 3: Company Exposure Ranking -->
  <div class="section-divider">Company exposure ranking</div>
  <div class="grid-2">
    <div class="card">
      <div class="card-header">
        <div class="card-title">Data controller exposure</div>
        <div class="card-badge">By platform count</div>
      </div>
      <div id="company-list"></div>
    </div>
    <div class="card">
      <div class="card-header">
        <div class="card-title">Ecosystem concentration</div>
        <div class="card-badge">% of services</div>
      </div>
      <div style="position:relative;height:280px;">
        <canvas id="pieChart" role="img" aria-label="Pie chart showing Google at 40%, Meta at 20%, Apple at 13%, ByteDance at 13%, Amazon at 7%, Activision/Krafton at 7%">Google 40%, Meta 20%, Apple 13%, ByteDance 13%, Amazon 7%, others 7%.</canvas>
      </div>
      <div class="radar-legend" style="margin-top:12px" id="pie-legend"></div>
    </div>
  </div>

  <!-- SECTION 4: Data Collection Matrix -->
  <div class="section-divider">Data collection matrix</div>
  <div class="card">
    <div class="card-header">
      <div class="card-title">What each company likely collects</div>
      <div style="display:flex;gap:8px;align-items:center">
        <span style="font-size:11px;display:flex;align-items:center;gap:4px;color:var(--text2)"><span class="dot dot-yes" style="width:14px;height:14px">●</span> Likely</span>
        <span style="font-size:11px;display:flex;align-items:center;gap:4px;color:var(--text2)"><span class="dot dot-maybe" style="width:14px;height:14px">?</span> Possible</span>
        <span style="font-size:11px;display:flex;align-items:center;gap:4px;color:var(--text2)"><span class="dot dot-no" style="width:14px;height:14px">–</span> Unlikely</span>
        <span class="badge-est">All estimates</span>
      </div>
    </div>
    <div style="overflow-x:auto;">
      <table class="matrix-table" id="matrix-table"></table>
    </div>
  </div>

  <!-- SECTION 5: Risk Radar -->
  <div class="section-divider">Risk radar</div>
  <div class="grid-2">
    <div class="card">
      <div class="card-header">
        <div class="card-title">Multi-domain risk assessment</div>
        <div class="card-badge badge-est">Estimated</div>
      </div>
      <div class="radar-wrap" style="height:300px; position:relative;">
        <canvas id="radarChart" role="img" aria-label="Radar chart showing risk across identity, financial, social, behavioral, location, and communication axes">Risk levels: Identity 78, Financial 65, Social 80, Behavioral 85, Location 60, Communication 70.</canvas>
      </div>
    </div>
    <div class="card">
      <div class="card-header">
        <div class="card-title">Risk breakdown by category</div>
      </div>
      <div id="risk-bars"></div>
    </div>
  </div>

  <!-- SECTION 6: Digital Twin Profile -->
  <div class="section-divider">Digital twin profile</div>
  <div class="grid-2">
    <div class="card">
      <div class="card-header">
        <div class="card-title">What algorithms likely know about you</div>
        <div class="card-badge badge-est">All estimates</div>
      </div>

      <div class="twin-section">
        <div class="twin-label">Demographic inferences</div>
        <div class="twin-tags">
          <span class="twin-tag tag-est">Age: likely 13–25</span>
          <span class="twin-tag tag-est">Gender: inferred from app mix</span>
          <span class="twin-tag tag-est">Language: Hindi / English</span>
          <span class="twin-tag tag-est">Country: India (likely Bihar region)</span>
          <span class="twin-tag tag-est">Device: Smartphone primary</span>
        </div>
      </div>

      <div class="twin-section">
        <div class="twin-label">Behavioural inferences</div>
        <div class="twin-tags">
          <span class="twin-tag tag-est">Short-form video consumer</span>
          <span class="twin-tag tag-est">Online gaming enthusiast</span>
          <span class="twin-tag tag-est">Music streaming daily</span>
          <span class="twin-tag tag-est">Social media active</span>
          <span class="twin-tag tag-est">Online shopper</span>
          <span class="twin-tag tag-est">UPI payment user</span>
          <span class="twin-tag tag-est">Digital photo archiver</span>
        </div>
      </div>

      <div class="twin-section">
        <div class="twin-label">Confirmed facts</div>
        <div class="twin-tags">
          <span class="twin-tag tag-fact">Uses Instagram</span>
          <span class="twin-tag tag-fact">Uses TikTok</span>
          <span class="twin-tag tag-fact">Uses YouTube</span>
          <span class="twin-tag tag-fact">Uses Discord</span>
          <span class="twin-tag tag-fact">Uses WhatsApp</span>
          <span class="twin-tag tag-fact">Uses Google Pay</span>
          <span class="twin-tag tag-fact">Uses iMessage</span>
          <span class="twin-tag tag-fact">Uses Spotify</span>
          <span class="twin-tag tag-fact">Uses Roblox</span>
          <span class="twin-tag tag-fact">Uses PUBG Mobile</span>
          <span class="twin-tag tag-fact">Uses Meesho</span>
          <span class="twin-tag tag-fact">Uses Amazon</span>
          <span class="twin-tag tag-fact">Uses Snapchat</span>
          <span class="twin-tag tag-fact">Uses Google Search</span>
          <span class="twin-tag tag-fact">Uses Google Photos</span>
        </div>
      </div>

      <div class="twin-section" style="margin-bottom:0">
        <div class="twin-label">Not enough information</div>
        <div class="twin-tags">
          <span class="twin-tag" style="background:var(--bg4);color:var(--text2);border:1px solid var(--border)">Exact age</span>
          <span class="twin-tag" style="background:var(--bg4);color:var(--text2);border:1px solid var(--border)">Income level</span>
          <span class="twin-tag" style="background:var(--bg4);color:var(--text2);border:1px solid var(--border)">Real name</span>
          <span class="twin-tag" style="background:var(--bg4);color:var(--text2);border:1px solid var(--border)">Precise location</span>
        </div>
      </div>
    </div>

    <!-- WOW Insights -->
    <div style="display:flex;flex-direction:column;gap:1rem;">
      <div class="card" style="background:rgba(79,110,247,0.05);border-color:rgba(79,110,247,0.2)">
        <div class="card-title" style="margin-bottom:1rem;color:var(--accent)">⚡ Wow insights</div>
        <div style="display:flex;flex-direction:column;gap:10px;">
          <div class="wow-card" style="background:var(--bg4)">
            <div class="wow-icon">🔭</div>
            <div>
              <div class="wow-title">3 platforms can likely recognise your face</div>
              <div class="wow-desc">Instagram, Snapchat & Google Photos all use facial recognition or AR face mapping. <span class="badge-est" style="display:inline;font-size:10px">Estimate</span></div>
            </div>
          </div>
          <div class="wow-card" style="background:var(--bg4)">
            <div class="wow-icon">🎙️</div>
            <div>
              <div class="wow-title">Multiple apps have microphone access</div>
              <div class="wow-desc">TikTok, Instagram, Snapchat, Discord & WhatsApp may request mic access. Ad targeting based on voice is disputed but plausible. <span class="badge-est" style="display:inline;font-size:10px">Estimate</span></div>
            </div>
          </div>
          <div class="wow-card" style="background:var(--bg4)">
            <div class="wow-icon">💳</div>
            <div>
              <div class="wow-title">Google knows your spending habits</div>
              <div class="wow-desc">Google Pay + Google Search creates a powerful link between what you search for and what you buy. <span class="badge-est" style="display:inline;font-size:10px">Estimate</span></div>
            </div>
          </div>
          <div class="wow-card" style="background:var(--bg4)">
            <div class="wow-icon">🌐</div>
            <div>
              <div class="wow-title">ByteDance operates outside GDPR jurisdiction</div>
              <div class="wow-desc">TikTok data may be subject to Chinese data laws, which differ significantly from Indian or European privacy standards. <span class="badge-est" style="display:inline;font-size:10px">Estimate</span></div>
            </div>
          </div>
          <div class="wow-card" style="background:var(--bg4)">
            <div class="wow-icon">📸</div>
            <div>
              <div class="wow-title">Your photos may train AI models</div>
              <div class="wow-desc">Google Photos content may be used for improving Google AI, per their terms of service. <span class="badge-est" style="display:inline;font-size:10px">Estimate</span></div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- SECTION 7: Most Valuable Data Assets -->
  <div class="section-divider">Most valuable data assets</div>
  <div class="card">
    <div class="card-header">
      <div class="card-title">Your data as a commodity — estimated ad-market value signals</div>
      <div class="card-badge badge-est">Estimated</div>
    </div>
    <div class="grid-3" id="assets-grid" style="gap:1rem;"></div>
    <div style="margin-top:1rem;padding:1rem;background:var(--bg3);border-radius:10px;font-size:12px;color:var(--text2)">
      <i class="ti ti-info-circle" style="vertical-align:-2px;margin-right:6px;font-size:14px;color:var(--text3)"></i>
      These are illustrative signals, not actual prices. Ad-tech industry research suggests behavioural + financial intent data is among the most valuable user data for targeted advertising. Exact values are impossible to determine without access to platform revenue data.
    </div>
  </div>

  <!-- SECTION 8: Privacy Improvement Simulator -->
  <div class="section-divider">Privacy improvement simulator</div>
  <div class="grid-2">
    <div class="card">
      <div class="card-header">
        <div class="card-title">Toggle actions to see privacy score impact</div>
        <div class="card-badge">Simulated</div>
      </div>
      <div id="sim-list"></div>
    </div>
    <div class="card" style="display:flex;flex-direction:column;align-items:center;justify-content:center;">
      <div class="sim-score-display" style="width:100%">
        <div class="twin-label" style="margin-bottom:8px">Simulated privacy score</div>
        <div class="sim-score-num" id="sim-score" style="color:var(--orange)">30</div>
        <div class="sim-score-label" id="sim-label">Fair — Current baseline</div>
        <div style="margin-top:1rem;height:6px;background:var(--bg4);border-radius:99px;overflow:hidden;">
          <div id="sim-bar" style="height:100%;width:30%;background:var(--orange);border-radius:99px;transition:width 0.4s,background 0.4s;"></div>
        </div>
        <div style="display:flex;justify-content:space-between;font-size:10px;color:var(--text3);margin-top:4px">
          <span>0 — Weak</span><span>100 — Strong</span>
        </div>
      </div>
      <div style="margin-top:1.5rem;width:100%">
        <div class="twin-label" style="margin-bottom:8px">Improvement breakdown</div>
        <div id="sim-breakdown" style="font-size:12px;color:var(--text2);display:flex;flex-direction:column;gap:6px;"></div>
      </div>
    </div>
  </div>

  <!-- SECTION 9: Final Verdict -->
  <div class="section-divider">Final verdict</div>
  <div class="card">
    <div class="verdict-header">
      <div class="verdict-grade">D+</div>
      <div>
        <div class="verdict-title">Significant exposure, limited control</div>
        <div class="verdict-sub">You use powerful, feature-rich apps — but at a substantial privacy cost. 6 data controllers, 3 likely facial recognition systems, and at least 2 cross-ecosystem payment trackers profile you daily.</div>
        <div class="verdict-pills">
          <span class="vpill vpill-red">High behavioral exposure</span>
          <span class="vpill vpill-red">ByteDance jurisdiction risk</span>
          <span class="vpill vpill-red">Multi-platform face mapping</span>
          <span class="vpill vpill-yellow">Dual payment ecosystem</span>
          <span class="vpill vpill-yellow">Cross-app ad targeting</span>
          <span class="vpill vpill-green">Apple iMessage encryption</span>
          <span class="vpill vpill-green">WhatsApp E2E encryption</span>
        </div>
      </div>
    </div>
    <div style="border-top:1px solid var(--border);padding-top:1.25rem;margin-top:0.5rem;">
      <div class="twin-label" style="margin-bottom:1rem">Top 5 recommended actions</div>
      <div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(200px,1fr));gap:10px;">
        <div class="wow-card">
          <div class="wow-icon">🗑️</div>
          <div><div class="wow-title">Review TikTok data permissions</div><div class="wow-desc">Check what data TikTok has collected via Settings → Privacy → Personalization.</div></div>
        </div>
        <div class="wow-card">
          <div class="wow-icon">🔐</div>
          <div><div class="wow-title">Enable 2FA everywhere</div><div class="wow-desc">Protect all accounts with two-factor authentication, especially Google, Instagram, and Discord.</div></div>
        </div>
        <div class="wow-card">
          <div class="wow-icon">📵</div>
          <div><div class="wow-title">Audit app permissions</div><div class="wow-desc">Review mic, camera, and location permissions for all 15 apps in your phone settings.</div></div>
        </div>
        <div class="wow-card">
          <div class="wow-icon">🧹</div>
          <div><div class="wow-title">Request data deletion</div><div class="wow-desc">Use Google Takeout and Meta's Privacy Center to download and review your stored data.</div></div>
        </div>
        <div class="wow-card">
          <div class="wow-icon">🌐</div>
          <div><div class="wow-title">Consider a privacy browser</div><div class="wow-desc">Replace Google Chrome with Brave or Firefox to reduce tracking outside apps.</div></div>
        </div>
      </div>
    </div>
    <div style="margin-top:1.25rem;padding:1rem;background:rgba(79,110,247,0.06);border:1px solid rgba(79,110,247,0.15);border-radius:10px;font-size:12px;color:var(--text2);line-height:1.6">
      <strong style="color:var(--accent)">Disclaimer:</strong> This analysis is based solely on your self-reported list of apps. It does not involve access to any private accounts, databases, or APIs. All risk assessments are estimates derived from publicly documented data practices. Never assume certainty from this report.
    </div>
  </div>

</div><!-- /dashboard -->

<script>
// ── Data ─────────────────────────────────────────────────
const apps = [
  { name:'Instagram',    co:'Meta',       score:85, cat:'Social',    color:'#e1306c' },
  { name:'Snapchat',     co:'Snap',       score:72, cat:'Social',    color:'#fffc00' },
  { name:'TikTok',       co:'ByteDance',  score:90, cat:'Video',     color:'#010101' },
  { name:'YouTube',      co:'Google',     score:78, cat:'Video',     color:'#ff0000' },
  { name:'Discord',      co:'Discord',    score:60, cat:'Comms',     color:'#5865f2' },
  { name:'WhatsApp',     co:'Meta',       score:55, cat:'Comms',     color:'#25d366' },
  { name:'iMessage',     co:'Apple',      score:30, cat:'Comms',     color:'#34c759' },
  { name:'Spotify',      co:'Spotify',    score:65, cat:'Music',     color:'#1db954' },
  { name:'Roblox',       co:'Roblox',     score:70, cat:'Gaming',    color:'#e2231a' },
  { name:'PUBG Mobile',  co:'Krafton',    score:68, cat:'Gaming',    color:'#f5a623' },
  { name:'Amazon',       co:'Amazon',     score:80, cat:'Shopping',  color:'#ff9900' },
  { name:'Meesho',       co:'Meesho',     score:70, cat:'Shopping',  color:'#9c2bff' },
  { name:'Google Search',co:'Google',     score:92, cat:'Search',    color:'#4285f4' },
  { name:'Google Pay',   co:'Google',     score:82, cat:'Finance',   color:'#34a853' },
  { name:'Google Photos',co:'Google',     score:75, cat:'Storage',   color:'#fbbc04' },
];

function heatColor(score) {
  if (score >= 85) return { bg:'rgba(239,68,68,0.18)', border:'rgba(239,68,68,0.35)', text:'#fca5a5' };
  if (score >= 70) return { bg:'rgba(249,115,22,0.15)', border:'rgba(249,115,22,0.3)', text:'#fdba74' };
  if (score >= 55) return { bg:'rgba(234,179,8,0.12)', border:'rgba(234,179,8,0.28)', text:'#fde68a' };
  return { bg:'rgba(34,197,94,0.1)', border:'rgba(34,197,94,0.25)', text:'#86efac' };
}

// Heatmap
const hmap = document.getElementById('heatmap');
apps.forEach(a => {
  const c = heatColor(a.score);
  const cell = document.createElement('div');
  cell.className = 'heatmap-cell';
  cell.style.cssText = `background:${c.bg};border:1px solid ${c.border}`;
  cell.innerHTML = `
    <div class="app-score" style="color:${c.text}">${a.score}</div>
    <div class="app-name" style="color:${c.text}">${a.name}</div>
    <div class="app-co" style="color:${c.text}">${a.co}</div>
  `;
  hmap.appendChild(cell);
});

// Company exposure
const companies = [
  { name:'Google', apps:'Search, Pay, Photos, YouTube', count:4, pct:92, color:'#4285f4', risk:'Critical', riskClass:'vpill-red' },
  { name:'Meta', apps:'Instagram, WhatsApp', count:2, pct:75, color:'#1877f2', risk:'High', riskClass:'vpill-red' },
  { name:'ByteDance', apps:'TikTok', count:1, pct:90, color:'#010101', risk:'High', riskClass:'vpill-red' },
  { name:'Amazon', apps:'Amazon', count:1, pct:65, color:'#ff9900', risk:'Moderate', riskClass:'vpill-yellow' },
  { name:'Apple', apps:'iMessage', count:1, pct:22, color:'#555', risk:'Low', riskClass:'vpill-green' },
  { name:'Others', apps:'Discord, Snap, Spotify, Roblox, Krafton, Meesho', count:6, pct:55, color:'#8b5cf6', risk:'Moderate', riskClass:'vpill-yellow' },
];

const coList = document.getElementById('company-list');
companies.forEach(c => {
  const row = document.createElement('div');
  row.className = 'company-row';
  row.innerHTML = `
    <div class="company-icon" style="background:${c.color}22;color:${c.color};font-size:11px;font-weight:800">${c.name.slice(0,2).toUpperCase()}</div>
    <div class="company-info">
      <div class="company-name">${c.name} <span style="font-weight:400;color:var(--text3);font-size:11px">${c.count} app${c.count>1?'s':''}</span></div>
      <div class="company-apps">${c.apps}</div>
    </div>
    <div class="exposure-bar-wrap">
      <div class="exposure-bar-bg"><div class="exposure-bar-fill" style="width:${c.pct}%;background:${c.color}"></div></div>
      <div class="exposure-pct" style="color:${c.color}">${c.pct}%</div>
    </div>
    <div class="risk-pill ${c.riskClass}">${c.risk}</div>
  `;
  coList.appendChild(row);
});

// Pie chart
const pieLegend = document.getElementById('pie-legend');
const pieData = [
  { label:'Google (4 apps)', val:27, color:'#4285f4' },
  { label:'Meta (2 apps)', val:13, color:'#1877f2' },
  { label:'ByteDance', val:7, color:'#555' },
  { label:'Apple', val:7, color:'#888' },
  { label:'Amazon', val:7, color:'#ff9900' },
  { label:'Others (6 apps)', val:39, color:'#8b5cf6' },
];
pieData.forEach(d => {
  const s = document.createElement('span');
  s.className = 'radar-legend-item';
  s.innerHTML = `<span class="radar-dot" style="background:${d.color}"></span>${d.label} — ${d.val}%`;
  pieLegend.appendChild(s);
});

new Chart(document.getElementById('pieChart'), {
  type: 'doughnut',
  data: {
    labels: pieData.map(d=>d.label),
    datasets:[{ data: pieData.map(d=>d.val), backgroundColor: pieData.map(d=>d.color), borderWidth:2, borderColor:'#111318' }]
  },
  options: {
    responsive: true, maintainAspectRatio: false,
    cutout: '60%',
    plugins: { legend: { display:false }, tooltip: { callbacks: { label: ctx => ` ${ctx.parsed}%` } } }
  }
});

// Matrix table
const matrix = {
  headers: ['Company','Location','Biometrics','Contacts','Purchases','Browsing','Audio','Content'],
  rows: [
    ['Google',      'yes','maybe','yes','yes','yes','no','yes'],
    ['Meta',        'yes','yes','yes','yes','yes','maybe','yes'],
    ['ByteDance',   'yes','yes','no','maybe','yes','maybe','yes'],
    ['Apple',       'maybe','maybe','no','no','no','no','maybe'],
    ['Amazon',      'yes','no','no','yes','yes','no','yes'],
    ['Snap',        'yes','yes','yes','no','yes','maybe','yes'],
    ['Discord',     'no','no','yes','no','no','maybe','yes'],
    ['Spotify',     'yes','no','no','no','no','no','yes'],
  ]
};
const t = document.getElementById('matrix-table');
const thead = t.createTHead();
const hr = thead.insertRow();
matrix.headers.forEach(h => {
  const th = document.createElement('th');
  th.textContent = h;
  hr.appendChild(th);
});
const tbody = t.createTBody();
matrix.rows.forEach(row => {
  const tr = tbody.insertRow();
  row.forEach((cell, i) => {
    const td = tr.insertCell();
    if (i === 0) { td.textContent = cell; return; }
    const cls = cell === 'yes' ? 'dot-yes' : cell === 'maybe' ? 'dot-maybe' : 'dot-no';
    const sym = cell === 'yes' ? '●' : cell === 'maybe' ? '?' : '–';
    td.innerHTML = `<span class="dot ${cls}">${sym}</span>`;
  });
});

// Radar chart
new Chart(document.getElementById('radarChart'), {
  type: 'radar',
  data: {
    labels: ['Identity','Financial','Social','Behavioral','Location','Communication'],
    datasets: [{
      label: 'Your Risk Level',
      data: [78, 65, 80, 85, 60, 70],
      backgroundColor: 'rgba(239,68,68,0.15)',
      borderColor: '#ef4444',
      borderWidth: 2,
      pointBackgroundColor: '#ef4444',
      pointRadius: 4,
    }]
  },
  options: {
    responsive: true, maintainAspectRatio: false,
    scales: {
      r: {
        min: 0, max: 100,
        grid: { color: 'rgba(255,255,255,0.06)' },
        angleLines: { color: 'rgba(255,255,255,0.06)' },
        pointLabels: { color: '#8a90a0', font: { size: 11 } },
        ticks: { display: false, stepSize: 25 }
      }
    },
    plugins: { legend: { display: false } }
  }
});

// Risk bars
const risks = [
  { label:'Behavioral profiling', val:85, color:'#ef4444' },
  { label:'Social graph exposure', val:80, color:'#ef4444' },
  { label:'Identity tracking', val:78, color:'#f97316' },
  { label:'Financial data', val:65, color:'#f97316' },
  { label:'Communication intercept', val:70, color:'#eab308' },
  { label:'Location inference', val:60, color:'#eab308' },
];
const rb = document.getElementById('risk-bars');
risks.forEach(r => {
  const d = document.createElement('div');
  d.style.cssText = 'margin-bottom:14px';
  d.innerHTML = `
    <div style="display:flex;justify-content:space-between;font-size:12px;margin-bottom:5px">
      <span style="color:var(--text)">${r.label}</span>
      <span style="color:${r.color};font-weight:600">${r.val}/100</span>
    </div>
    <div style="height:6px;background:var(--bg4);border-radius:99px;overflow:hidden">
      <div style="height:100%;width:${r.val}%;background:${r.color};border-radius:99px;transition:width 1s"></div>
    </div>
    <div style="font-size:10px;color:var(--text3);margin-top:3px">${r.val>=80?'Critical':r.val>=65?'High':r.val>=50?'Moderate':'Low'} risk — Estimated</div>
  `;
  rb.appendChild(d);
});

// Assets
const assets = [
  { icon:'🎯', name:'Behavioral profile', iconBg:'rgba(239,68,68,0.12)', value:'High', color:'#ef4444', desc:'Your browsing, viewing, and gaming patterns form a rich behavioral graph used for targeted advertising.' },
  { icon:'💳', name:'Financial intent signals', iconBg:'rgba(249,115,22,0.12)', value:'High', color:'#f97316', desc:'Google Pay + Amazon + Meesho create purchasing intent signals. This data segment is among the most valuable in ad-tech.' },
  { icon:'👤', name:'Social identity graph', iconBg:'rgba(168,85,247,0.12)', value:'High', color:'#a855f7', desc:'Your connections on Instagram, WhatsApp, and Discord form a social graph — valuable for targeting and analysis.' },
  { icon:'🎵', name:'Taste & preference data', iconBg:'rgba(29,185,84,0.12)', value:'Medium', color:'#1db954', desc:'Spotify listening history, YouTube watch history, and TikTok engagement reveal detailed lifestyle preferences.' },
  { icon:'📸', name:'Visual / biometric data', iconBg:'rgba(20,184,166,0.12)', value:'Medium', color:'#14b8a6', desc:'Photos in Google Photos, Instagram selfies, and Snapchat AR face data may contain biometric identifiers. Estimated.' },
  { icon:'📍', name:'Mobility patterns', iconBg:'rgba(234,179,8,0.12)', value:'Medium', color:'#eab308', desc:'Multiple apps request location. Inferred movement patterns can reveal home, work, and social locations. Estimated.' },
];
const ag = document.getElementById('assets-grid');
assets.forEach(a => {
  const card = document.createElement('div');
  card.className = 'asset-card';
  card.innerHTML = `
    <div class="asset-header">
      <div class="asset-icon" style="background:${a.iconBg};font-size:18px">${a.icon}</div>
      <div>
        <div class="asset-name">${a.name}</div>
        <div class="asset-value-label">Sensitivity</div>
      </div>
    </div>
    <div class="asset-value" style="color:${a.color}">${a.value}</div>
    <div class="asset-desc">${a.desc}</div>
    <div class="badge-est" style="display:inline-block;width:fit-content">Estimated</div>
  `;
  ag.appendChild(card);
});

// Simulator
const simActions = [
  { label:'Delete TikTok', sub:'Remove ByteDance data collection', gain:8, key:'tiktok' },
  { label:'Limit ad tracking (iOS)', sub:'Apple privacy settings', gain:10, key:'adtrack' },
  { label:'Use Signal instead of WhatsApp', sub:'Better encryption & no ads', gain:6, key:'signal' },
  { label:'Enable Google Safe Browsing', sub:'Turn off personalisation in My Activity', gain:7, key:'google' },
  { label:'Audit app permissions', sub:'Revoke location/mic on all apps', gain:9, key:'perms' },
  { label:'Use privacy-focused DNS', sub:'1.1.1.1 or NextDNS', gain:5, key:'dns' },
  { label:'Enable 2FA everywhere', sub:'Reduces account hijack risk', gain:5, key:'twofa' },
];
let simState = {};
simActions.forEach(a => { simState[a.key] = false; });

function updateSim() {
  const gained = simActions.filter(a => simState[a.key]).reduce((s,a)=>s+a.gain,0);
  const score = Math.min(100, 30 + gained);
  const scoreEl = document.getElementById('sim-score');
  const labelEl = document.getElementById('sim-label');
  const barEl = document.getElementById('sim-bar');
  scoreEl.textContent = score;
  let col = '#f97316', lbl = 'Fair';
  if (score >= 80) { col = '#22c55e'; lbl = 'Strong'; }
  else if (score >= 60) { col = '#eab308'; lbl = 'Good'; }
  else if (score >= 40) { col = '#f97316'; lbl = 'Fair'; }
  else { col = '#ef4444'; lbl = 'Weak'; }
  scoreEl.style.color = col;
  barEl.style.width = score + '%';
  barEl.style.background = col;
  labelEl.textContent = `${lbl} — +${gained} points from ${simActions.filter(a=>simState[a.key]).length} actions`;
  const bd = document.getElementById('sim-breakdown');
  bd.innerHTML = '';
  const active = simActions.filter(a => simState[a.key]);
  if (active.length === 0) {
    bd.innerHTML = '<span style="color:var(--text3)">Toggle actions on the left to simulate improvement.</span>';
  } else {
    active.forEach(a => {
      const line = document.createElement('div');
      line.style.cssText = 'display:flex;justify-content:space-between;align-items:center;padding:4px 0;border-bottom:1px solid var(--border)';
      line.innerHTML = `<span>${a.label}</span><span style="color:var(--green);font-weight:600">+${a.gain} pts</span>`;
      bd.appendChild(line);
    });
  }
}

const simList = document.getElementById('sim-list');
simActions.forEach(a => {
  const row = document.createElement('div');
  row.className = 'sim-row';
  row.innerHTML = `
    <div class="sim-action">
      <div class="action-title">${a.label}</div>
      <div class="action-sub">${a.sub}</div>
    </div>
    <div class="sim-gain">+${a.gain} pts</div>
    <button class="toggle-btn" id="tog-${a.key}" onclick="toggleSim('${a.key}')"><div class="toggle-knob"></div></button>
  `;
  simList.appendChild(row);
});

function toggleSim(key) {
  simState[key] = !simState[key];
  const btn = document.getElementById('tog-' + key);
  btn.classList.toggle('on', simState[key]);
  updateSim();
}

updateSim();
</script>
</body>
</html>
