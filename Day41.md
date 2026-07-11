<!DOCTYPE html>
<html lang="en" data-theme="dark">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Interactive Learning Studio — Building a Complete Responsive Page</title>
<style>
/* ============================================================
   TOKEN SYSTEM — "Drafting Table" theme
   A page about building pages, styled like a technical blueprint:
   sharp corners, dimension lines, title blocks, inspection stamps.
   ============================================================ */
:root{
  --bg:#0b2740;
  --bg-alt:#0f3252;
  --surface:#123a5e;
  --surface-2:#173f66;
  --ink:#e7edf3;
  --ink-dim:#9fb8cf;
  --line:rgba(138,187,224,0.22);
  --line-strong:rgba(138,187,224,0.45);
  --accent:#e8823c;
  --accent-ink:#2a1206;
  --accent-2:#5fb4d9;
  --success:#5fbf8f;
  --success-ink:#08281a;
  --danger:#e2685a;
  --danger-ink:#2c0c08;
  --radius:3px;
  --font-display:ui-monospace,"JetBrains Mono","SFMono-Regular",Menlo,Consolas,"Liberation Mono",monospace;
  --font-body:-apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,Helvetica,Arial,sans-serif;
  --grid-size:28px;
  --shadow:0 8px 24px rgba(0,0,0,0.35);
}
[data-theme="light"]{
  --bg:#eef2f5;
  --bg-alt:#e5eaef;
  --surface:#ffffff;
  --surface-2:#f4f7f9;
  --ink:#122236;
  --ink-dim:#51687d;
  --line:rgba(18,34,54,0.14);
  --line-strong:rgba(18,34,54,0.3);
  --accent:#c8672b;
  --accent-ink:#fff5ec;
  --accent-2:#0f7ba8;
  --success:#2f8a5f;
  --success-ink:#eafff3;
  --danger:#c94a3c;
  --danger-ink:#fff0ee;
  --shadow:0 8px 24px rgba(20,40,60,0.12);
}
*{box-sizing:border-box;}
html{scroll-behavior:smooth;}
body{
  margin:0;
  background:
    linear-gradient(var(--line) 1px, transparent 1px) 0 0/var(--grid-size) var(--grid-size),
    linear-gradient(90deg, var(--line) 1px, transparent 1px) 0 0/var(--grid-size) var(--grid-size),
    var(--bg);
  color:var(--ink);
  font-family:var(--font-body);
  line-height:1.6;
  transition:background-color .3s ease, color .3s ease;
  padding-bottom:80px;
}
h1,h2,h3,h4{font-family:var(--font-display); font-weight:700; letter-spacing:-0.01em; margin:0 0 .5em;}
p{margin:0 0 1em;}
code, .mono{font-family:var(--font-display);}
a{color:var(--accent-2);}
button{font-family:var(--font-body); cursor:pointer;}
::selection{background:var(--accent); color:var(--accent-ink);}

/* corner-bracket "drafting sheet" frame, used on cards */
.sheet{
  position:relative;
  background:var(--surface);
  border:1px solid var(--line-strong);
  border-radius:var(--radius);
  box-shadow:var(--shadow);
}
.sheet::before,.sheet::after,
.sheet .brk-br, .sheet .brk-bl{
  content:"";
  position:absolute;
  width:14px;height:14px;
  pointer-events:none;
}
.sheet::before{ top:-1px; left:-1px; border-top:2px solid var(--accent-2); border-left:2px solid var(--accent-2);}
.sheet::after{ top:-1px; right:-1px; border-top:2px solid var(--accent-2); border-right:2px solid var(--accent-2);}

.wrap{max-width:980px; margin:0 auto; padding:0 20px;}

/* ---------- top bar ---------- */
.topbar{
  position:sticky; top:0; z-index:50;
  backdrop-filter:blur(10px);
  background:color-mix(in srgb, var(--bg) 85%, transparent);
  border-bottom:1px solid var(--line);
}
.topbar-inner{max-width:980px; margin:0 auto; padding:12px 20px; display:flex; align-items:center; gap:16px;}
.brand{display:flex; align-items:center; gap:10px; font-family:var(--font-display); font-weight:700; font-size:14px; letter-spacing:.04em; text-transform:uppercase; color:var(--ink);}
.brand .dot{width:9px;height:9px;background:var(--accent); display:inline-block;}
.progress-track{flex:1; height:8px; background:var(--surface-2); border:1px solid var(--line); position:relative; overflow:hidden;}
.progress-fill{height:100%; width:0%; background:linear-gradient(90deg,var(--accent-2),var(--accent)); transition:width .5s ease;}
.progress-label{font-family:var(--font-display); font-size:12px; color:var(--ink-dim); min-width:64px; text-align:right;}
.icon-btn{
  background:transparent; border:1px solid var(--line-strong); color:var(--ink);
  border-radius:var(--radius); padding:6px 10px; font-size:13px; display:flex; align-items:center; gap:6px;
}
.icon-btn:hover{border-color:var(--accent-2); color:var(--accent-2);}

/* ---------- hero / title block ---------- */
.hero{padding:56px 0 24px;}
.eyebrow{font-family:var(--font-display); text-transform:uppercase; letter-spacing:.12em; font-size:12px; color:var(--accent-2); margin-bottom:10px;}
.hero h1{font-size:clamp(28px,4vw,44px); line-height:1.1; max-width:16ch;}
.hero .lede{color:var(--ink-dim); max-width:60ch; font-size:16px;}

.title-block{
  margin-top:32px;
  display:grid;
  grid-template-columns:repeat(4,1fr);
  border:1px solid var(--line-strong);
  border-radius:var(--radius);
  overflow:hidden;
  background:var(--surface);
}
.title-block .cell{padding:16px 18px; border-right:1px solid var(--line);}
.title-block .cell:last-child{border-right:none;}
.title-block .cell .k{font-family:var(--font-display); font-size:11px; text-transform:uppercase; letter-spacing:.08em; color:var(--ink-dim); margin-bottom:6px;}
.title-block .cell .v{font-size:14px; font-weight:600;}
@media (max-width:720px){.title-block{grid-template-columns:1fr 1fr;} .title-block .cell:nth-child(2){border-right:none;}}

.intro-grid{display:grid; grid-template-columns:1.4fr 1fr; gap:20px; margin-top:24px;}
@media (max-width:800px){.intro-grid{grid-template-columns:1fr;}}
.card{padding:22px 24px;}
.list-check{list-style:none; margin:0; padding:0;}
.list-check li{position:relative; padding-left:26px; margin-bottom:10px; font-size:14.5px;}
.list-check li::before{content:"▸"; position:absolute; left:0; color:var(--accent); font-family:var(--font-display);}

.reward-box{padding:22px 24px;}
.reward-row{display:flex; align-items:center; justify-content:space-between; padding:10px 0; border-bottom:1px dashed var(--line);}
.reward-row:last-child{border-bottom:none;}
.reward-points{font-family:var(--font-display); font-size:22px; font-weight:700; color:var(--accent);}

.cta-row{margin-top:26px; display:flex; gap:12px; flex-wrap:wrap;}
.btn{
  font-family:var(--font-display); font-size:13px; text-transform:uppercase; letter-spacing:.06em;
  padding:12px 20px; border-radius:var(--radius); border:1px solid var(--line-strong);
  background:transparent; color:var(--ink); transition:all .15s ease;
}
.btn.primary{background:var(--accent); color:var(--accent-ink); border-color:var(--accent);}
.btn.primary:hover{filter:brightness(1.08);}
.btn:hover{border-color:var(--accent-2); color:var(--accent-2);}
.btn:disabled{opacity:.4; cursor:not-allowed;}
.btn:disabled:hover{border-color:var(--line-strong); color:var(--ink);}

/* ---------- module ---------- */
.module{margin-top:64px; scroll-margin-top:90px;}
.module.locked{opacity:.45; pointer-events:none; filter:grayscale(.6);}
.module-head{display:flex; align-items:baseline; gap:16px; margin-bottom:6px;}
.module-num{font-family:var(--font-display); font-size:13px; color:var(--accent); border:1px solid var(--accent); padding:2px 8px; border-radius:var(--radius);}
.module-sub{color:var(--ink-dim); font-size:14px; margin-bottom:28px;}

.section-label{
  font-family:var(--font-display); text-transform:uppercase; letter-spacing:.08em; font-size:12px;
  color:var(--accent-2); margin:36px 0 12px; display:flex; align-items:center; gap:10px;
}
.section-label::after{content:""; flex:1; height:1px; background:var(--line);}

.two-col{display:grid; grid-template-columns:1fr 1fr; gap:20px;}
@media (max-width:760px){.two-col{grid-template-columns:1fr;}}

.example-box{padding:18px 20px; margin:14px 0;}
.example-box pre{margin:0; overflow-x:auto; font-size:13px; font-family:var(--font-display); line-height:1.7;}
.example-box .cap{font-size:12px; color:var(--ink-dim); margin-top:10px; padding-top:10px; border-top:1px dashed var(--line);}

.analogy{
  padding:18px 20px; border-left:3px solid var(--accent);
  background:color-mix(in srgb, var(--accent) 8%, var(--surface));
  border-radius:0 var(--radius) var(--radius) 0;
  margin:16px 0;
}
.analogy .lbl{font-family:var(--font-display); font-size:11px; text-transform:uppercase; letter-spacing:.08em; color:var(--accent); margin-bottom:6px;}

.misconception{
  padding:16px 20px; border-left:3px solid var(--danger);
  background:color-mix(in srgb, var(--danger) 8%, var(--surface));
  border-radius:0 var(--radius) var(--radius) 0;
  margin:12px 0;
}
.misconception .lbl{font-family:var(--font-display); font-size:11px; text-transform:uppercase; letter-spacing:.08em; color:var(--danger); margin-bottom:6px;}

.takeaways{padding:20px; margin-top:20px;}
.takeaways h4{font-size:13px; text-transform:uppercase; letter-spacing:.08em; color:var(--accent-2);}

table.cmp{width:100%; border-collapse:collapse; font-size:13.5px; margin:14px 0;}
table.cmp th, table.cmp td{border:1px solid var(--line); padding:9px 12px; text-align:left;}
table.cmp th{background:var(--surface-2); font-family:var(--font-display); font-size:12px; text-transform:uppercase; letter-spacing:.04em;}

/* dimension line ruler used in diagrams */
.dim{display:flex; align-items:center; gap:6px; font-family:var(--font-display); font-size:11px; color:var(--ink-dim);}
.dim .tick{width:1px; height:8px; background:var(--ink-dim);}
.dim .line{flex:1; height:1px; background:var(--ink-dim); position:relative;}

/* box model diagram */
.boxmodel-wrap{padding:24px; margin:18px 0;}
.bm-controls{display:grid; grid-template-columns:repeat(3,1fr); gap:16px; margin-bottom:22px;}
.bm-controls label{display:block; font-family:var(--font-display); font-size:11px; text-transform:uppercase; color:var(--ink-dim); margin-bottom:6px;}
.bm-controls input[type=range]{width:100%; accent-color:var(--accent);}
.bm-controls .val{font-family:var(--font-display); color:var(--accent); font-size:12px;}
.bm-visual{display:flex; justify-content:center; padding:20px 0;}
.bm-margin{background:repeating-linear-gradient(45deg, color-mix(in srgb, #d9a441 22%, transparent), color-mix(in srgb, #d9a441 22%, transparent) 6px, transparent 6px, transparent 12px); border:1px dashed #d9a441; position:relative; transition:all .15s ease;}
.bm-border{background:color-mix(in srgb, var(--accent-2) 30%, transparent); border:2px solid var(--accent-2); position:relative; transition:all .15s ease;}
.bm-padding{background:color-mix(in srgb, var(--success) 25%, transparent); position:relative; transition:all .15s ease;}
.bm-content{background:var(--surface-2); border:1px solid var(--line-strong); display:flex; align-items:center; justify-content:center; font-family:var(--font-display); font-size:12px; color:var(--ink); min-width:70px; min-height:44px; transition:all .15s ease;}
.bm-tag{position:absolute; top:-9px; left:6px; font-family:var(--font-display); font-size:9px; text-transform:uppercase; padding:1px 5px; border-radius:2px; background:var(--bg); color:var(--ink-dim);}

/* semantic skeleton diagram */
.skeleton{display:flex; flex-direction:column; gap:6px; padding:18px; margin:16px 0;}
.sk-tag{
  border:1px dashed var(--line-strong); border-radius:var(--radius); padding:10px 14px;
  display:flex; align-items:center; justify-content:space-between; gap:10px;
  cursor:pointer; transition:all .15s ease; background:var(--surface);
}
.sk-tag:hover{border-color:var(--accent-2); background:var(--surface-2);}
.sk-tag .name{font-family:var(--font-display); font-size:13px; color:var(--accent-2);}
.sk-tag .desc{font-size:12.5px; color:var(--ink-dim); display:none; margin-top:8px;}
.sk-tag.open .desc{display:block;}
.sk-nested{margin-left:24px; border-left:2px solid var(--line); padding-left:14px; display:flex; flex-direction:column; gap:6px;}

/* grid playground */
.grid-play{padding:20px; margin:18px 0;}
.gp-controls{display:flex; flex-wrap:wrap; gap:22px; margin-bottom:18px; align-items:center;}
.gp-field{display:flex; align-items:center; gap:8px;}
.gp-field label{font-family:var(--font-display); font-size:11px; text-transform:uppercase; color:var(--ink-dim);}
.gp-field select, .gp-field input{
  background:var(--surface-2); border:1px solid var(--line-strong); color:var(--ink);
  padding:5px 8px; border-radius:var(--radius); font-family:var(--font-display); font-size:12px;
}
.gp-grid{display:grid; gap:8px; min-height:180px; padding:10px; border:1px dashed var(--line-strong);}
.gp-item{background:var(--surface-2); border:1px solid var(--accent-2); border-radius:var(--radius); display:flex; align-items:center; justify-content:center; font-family:var(--font-display); font-size:13px; color:var(--accent-2); min-height:52px;}
.gp-item:nth-child(3n+1){border-color:var(--accent);color:var(--accent);}
.gp-code{margin-top:14px; font-family:var(--font-display); font-size:12.5px; background:var(--bg-alt); padding:12px 14px; border-radius:var(--radius); border:1px solid var(--line);}

/* responsive viewport demo */
.vp-demo{padding:22px; margin:18px 0;}
.vp-frame-outer{border:1px dashed var(--line-strong); padding:16px; overflow-x:auto;}
.vp-frame{
  container-type: inline-size;
  resize:horizontal; overflow:auto;
  border:2px solid var(--accent-2); border-radius:var(--radius);
  width:100%; max-width:100%; min-width:220px;
  padding:16px; background:var(--surface-2);
}
.vp-content{display:grid; grid-template-columns:1fr; gap:10px;}
.vp-card{background:var(--surface); border:1px solid var(--line); border-radius:var(--radius); padding:10px 12px; font-size:13px;}
.vp-badge{font-family:var(--font-display); font-size:11px; color:var(--ink-dim); margin-bottom:14px;}
@container (min-width: 480px){
  .vp-content{grid-template-columns:1fr 1fr;}
  .vp-badge::after{content:" — breakpoint: ≥480px (2 columns)";}
}
@container (min-width: 720px){
  .vp-content{grid-template-columns:repeat(3,1fr);}
  .vp-badge::after{content:" — breakpoint: ≥720px (3 columns)";}
}
.vp-badge::after{content:" — breakpoint: <480px (1 column, mobile-first base)";}
.vp-hint{font-size:12.5px; color:var(--ink-dim); margin-top:10px;}

/* exercises */
.exercise{padding:20px; margin:18px 0; border-style:dashed !important;}
.exercise .lbl{font-family:var(--font-display); font-size:11px; text-transform:uppercase; letter-spacing:.08em; color:var(--success); margin-bottom:10px;}
.reveal-btn{margin-top:12px;}
.reveal-content{display:none; margin-top:14px; padding-top:14px; border-top:1px dashed var(--line);}
.reveal-content.open{display:block;}

/* quiz */
.quiz{margin-top:32px; padding:26px; border-color:var(--accent-2) !important;}
.quiz h3{font-size:16px;}
.q-item{margin-bottom:26px; padding-bottom:22px; border-bottom:1px solid var(--line);}
.q-item:last-of-type{border-bottom:none;}
.q-text{font-weight:600; margin-bottom:12px; font-size:14.5px;}
.q-opts{display:flex; flex-direction:column; gap:8px;}
.q-opt{
  display:flex; align-items:flex-start; gap:10px; padding:10px 12px; border:1px solid var(--line-strong);
  border-radius:var(--radius); cursor:pointer; font-size:13.5px; transition:all .12s ease;
}
.q-opt:hover{border-color:var(--accent-2);}
.q-opt.selected{border-color:var(--accent-2); background:color-mix(in srgb, var(--accent-2) 12%, var(--surface));}
.q-opt.correct{border-color:var(--success); background:color-mix(in srgb, var(--success) 16%, var(--surface));}
.q-opt.incorrect{border-color:var(--danger); background:color-mix(in srgb, var(--danger) 14%, var(--surface));}
.q-opt input{margin-top:3px;}
.q-explain{display:none; margin-top:10px; padding:10px 12px; font-size:13px; border-radius:var(--radius); background:var(--surface-2); border:1px solid var(--line);}
.q-explain.open{display:block;}
.quiz-summary{display:none; margin-top:10px; padding:20px; border-radius:var(--radius); text-align:center; background:var(--surface-2); border:1px solid var(--line-strong);}
.quiz-summary.open{display:block;}
.quiz-summary .score{font-family:var(--font-display); font-size:32px; font-weight:700; color:var(--accent);}

/* stamp */
.stamp{
  display:inline-flex; align-items:center; gap:8px; font-family:var(--font-display); font-size:12px;
  text-transform:uppercase; letter-spacing:.08em; border:2px solid var(--success); color:var(--success);
  padding:6px 12px; border-radius:4px; transform:rotate(-3deg); opacity:0; transition:opacity .3s ease;
}
.stamp.show{opacity:1;}

/* badges strip */
.badge-strip{display:flex; gap:10px; flex-wrap:wrap; margin-top:14px;}
.badge{
  font-family:var(--font-display); font-size:11px; text-transform:uppercase; padding:6px 10px;
  border:1px solid var(--line-strong); border-radius:var(--radius); color:var(--ink-dim);
}
.badge.earned{border-color:var(--accent); color:var(--accent);}

/* final challenge */
.challenge{padding:26px; margin-top:20px;}
.editor-grid{display:grid; grid-template-columns:1fr 1fr; gap:14px; margin-top:16px;}
@media (max-width:760px){.editor-grid{grid-template-columns:1fr;}}
.editor-grid textarea{
  width:100%; height:220px; background:var(--bg-alt); color:var(--ink); border:1px solid var(--line-strong);
  border-radius:var(--radius); font-family:var(--font-display); font-size:12.5px; padding:12px; resize:vertical;
}
.editor-grid iframe{width:100%; height:220px; border:1px solid var(--line-strong); border-radius:var(--radius); background:#fff;}
.checklist{margin-top:16px;}
.checklist label{display:flex; gap:10px; align-items:center; padding:8px 0; font-size:13.5px; border-bottom:1px dashed var(--line); cursor:pointer;}
.checklist input{accent-color:var(--success);}

/* cheat sheet */
.cheat-grid{display:grid; grid-template-columns:1fr 1fr; gap:16px; margin-top:16px;}
@media (max-width:760px){.cheat-grid{grid-template-columns:1fr;}}
.cheat-card{padding:16px 18px;}
.cheat-card h4{font-size:13px; color:var(--accent-2); text-transform:uppercase; letter-spacing:.06em;}
.cheat-card pre{font-size:12px; margin:0; white-space:pre-wrap; font-family:var(--font-display); line-height:1.8;}

/* resources */
.res-grid{display:grid; grid-template-columns:repeat(3,1fr); gap:16px; margin-top:16px;}
@media (max-width:800px){.res-grid{grid-template-columns:1fr 1fr;}}
@media (max-width:520px){.res-grid{grid-template-columns:1fr;}}
.res-card{padding:16px 18px;}
.res-card h4{font-size:12.5px; text-transform:uppercase; letter-spacing:.06em; color:var(--accent); margin-bottom:10px;}
.res-card ul{margin:0; padding-left:18px; font-size:13px; color:var(--ink-dim);}
.res-card li{margin-bottom:6px;}
.kw-chip-row{display:flex; flex-wrap:wrap; gap:8px;}
.kw-chip{font-family:var(--font-display); font-size:11.5px; border:1px solid var(--line-strong); border-radius:20px; padding:5px 12px; color:var(--ink-dim);}

footer{text-align:center; margin-top:70px; padding:30px 0; color:var(--ink-dim); font-size:12.5px; border-top:1px solid var(--line);}

/* print */
@media print{
  .topbar, .cta-row, .icon-btn, .reveal-btn, video, .btn{display:none !important;}
  body{background:#fff; color:#000;}
  .sheet{box-shadow:none; border-color:#999;}
  .module.locked{display:none;}
}

@media (prefers-reduced-motion: reduce){
  *{animation:none !important; transition:none !important;}
}
</style>
</head>
<body>

<div class="topbar">
  <div class="topbar-inner">
    <div class="brand"><span class="dot"></span>Interactive Learning Studio</div>
    <div class="progress-track"><div class="progress-fill" id="progressFill"></div></div>
    <div class="progress-label" id="progressLabel">0% built</div>
    <button class="icon-btn" id="themeToggle" title="Toggle light/dark">◐ Theme</button>
    <button class="icon-btn" id="printBtn" title="Print notes">⎙ Print</button>
  </div>
</div>

<div class="wrap">

  <!-- ============ HERO / INTRO ============ -->
  <section class="hero">
    <div class="eyebrow">Sheet 01 — Project Brief</div>
    <h1>Building a Complete Responsive Page</h1>
    <p class="lede">A hands-on course in structuring a web page with semantic HTML and shaping it with CSS — the box model, Grid, and mobile-first responsive design — until it holds together at any screen size.</p>

    <div class="title-block sheet">
      <div class="cell"><div class="k">Est. Duration</div><div class="v">≈ 2.5 hours</div></div>
      <div class="cell"><div class="k">Prerequisites</div><div class="v">Basic HTML tags &amp; CSS selectors</div></div>
      <div class="cell"><div class="k">Modules</div><div class="v">4, progressive</div></div>
      <div class="cell"><div class="k">Format</div><div class="v">Read · Try · Quiz</div></div>
    </div>

    <div class="intro-grid">
      <div class="card sheet">
        <div class="section-label" style="margin-top:0;">Learning Objectives</div>
        <ul class="list-check">
          <li>Structure a page using semantic HTML5 landmarks instead of generic <code>&lt;div&gt;</code> soup</li>
          <li>Explain and control the CSS box model — content, padding, border, margin</li>
          <li>Build multi-region layouts confidently with CSS Grid</li>
          <li>Write mobile-first media queries that adapt a layout across breakpoints</li>
          <li>Assemble all four skills into one complete, responsive page</li>
        </ul>
        <div class="section-label">Expected Outcomes</div>
        <p style="font-size:14.5px; color:var(--ink-dim);">By the end, you'll read any web page and identify its structural skeleton, predict how box-model properties affect layout before testing them, and confidently reach for Grid and media queries instead of guessing with trial-and-error CSS.</p>
      </div>
      <div class="reward-box card sheet">
        <div class="section-label" style="margin-top:0;">Reward System</div>
        <div class="reward-row"><span>Total points available</span><span class="reward-points" id="totalPointsLabel">0 / 400</span></div>
        <div class="reward-row"><span>Modules completed</span><span id="modulesDoneLabel">0 / 4</span></div>
        <div class="reward-row"><span>Quiz average</span><span id="quizAvgLabel">—</span></div>
        <div class="badge-strip" id="badgeStrip"></div>
        <div style="margin-top:16px;"><span class="stamp" id="finalStamp">✓ Sheet Approved</span></div>
      </div>
    </div>

    <div class="cta-row">
      <button class="btn primary" id="startBtn">Begin Module 01 ↓</button>
      <button class="btn" id="cheatJump">Jump to Cheat Sheet</button>
    </div>
  </section>

  <!-- ============ MODULE 1 — SEMANTIC HTML ============ -->
  <section class="module" id="module-1">
    <div class="module-head"><span class="module-num">01 / 04</span><h2>Semantic HTML Structure &amp; Page Skeleton</h2></div>
    <p class="module-sub">Foundational — establishing the bones every page is built on.</p>

    <p>Every web page is, underneath its styling, a tree of nested boxes. HTML's job is to name what each box <em>means</em>, not just where it sits. When you write <code>&lt;div&gt;</code> for everything, the browser, screen readers, and search engines see only anonymous containers. Semantic tags — <code>&lt;header&gt;</code>, <code>&lt;nav&gt;</code>, <code>&lt;main&gt;</code>, <code>&lt;section&gt;</code>, <code>&lt;article&gt;</code>, <code>&lt;aside&gt;</code>, <code>&lt;footer&gt;</code> — tell everyone reading the markup what role each region plays.</p>

    <div class="analogy">
      <div class="lbl">Analogy</div>
      Think of semantic HTML as an architect's blueprint versus a pile of identical cardboard boxes. A blueprint labels "kitchen," "bedroom," "entrance" — anyone can find their way around without ever seeing the finished building. A pile of unlabeled boxes might contain the same rooms, but nobody can tell what's where until they open every single one.
    </div>

    <div class="section-label">Interactive Diagram — Click Each Region</div>
    <div class="skeleton sheet" id="skeletonDiagram">
      <div class="sk-tag" data-desc="Runs once at the top of the page: logo, site title, sometimes the primary navigation. There should typically be one per page (or one per sectioning root).">
        <span class="name">&lt;header&gt;</span><span style="color:var(--ink-dim); font-size:12px;">click to reveal</span>
      </div>
      <div class="sk-tag" data-desc="Wraps the primary navigation links. A page can have more than one <nav> (e.g. main nav + footer nav), each should be distinguishable, often via aria-label.">
        <span class="name">&lt;nav&gt;</span><span style="color:var(--ink-dim); font-size:12px;">click to reveal</span>
      </div>
      <div class="sk-tag" data-desc="The single dominant content of the document — exactly one per page, not nested inside <article>, <aside>, <nav>, or <header>. Skip-links usually jump straight here.">
        <span class="name">&lt;main&gt;</span><span style="color:var(--ink-dim); font-size:12px;">click to reveal</span>
      </div>
      <div class="sk-nested">
        <div class="sk-tag" data-desc="A self-contained, independently distributable piece of content — a blog post, a product card, a forum comment. Ask: 'would this make sense in an RSS feed on its own?' If yes, it's an article.">
          <span class="name">&lt;article&gt;</span><span style="color:var(--ink-dim); font-size:12px;">click to reveal</span>
        </div>
        <div class="sk-tag" data-desc="A thematic grouping of content, usually with its own heading. Use it when a chunk of the page deserves its own outline entry, not as a generic styling hook (that's still a plain <div>).">
          <span class="name">&lt;section&gt;</span><span style="color:var(--ink-dim); font-size:12px;">click to reveal</span>
        </div>
        <div class="sk-tag" data-desc="Content related to the surrounding content but separable from it — a pull-quote, a sidebar of related links, an ad unit. Removing it shouldn't hurt the main flow's meaning.">
          <span class="name">&lt;aside&gt;</span><span style="color:var(--ink-dim); font-size:12px;">click to reveal</span>
        </div>
      </div>
      <div class="sk-tag" data-desc="Closing information for its nearest sectioning ancestor: copyright, author info, related links. A page can have one at the document level and more inside individual <article> elements.">
        <span class="name">&lt;footer&gt;</span><span style="color:var(--ink-dim); font-size:12px;">click to reveal</span>
      </div>
    </div>

    <div class="section-label">Example</div>
    <div class="example-box sheet">
<pre>&lt;body&gt;
  &lt;header&gt;
    &lt;h1&gt;Studio Ridge Coffee&lt;/h1&gt;
    &lt;nav&gt;&lt;a href="#menu"&gt;Menu&lt;/a&gt; &lt;a href="#visit"&gt;Visit&lt;/a&gt;&lt;/nav&gt;
  &lt;/header&gt;
  &lt;main&gt;
    &lt;section id="menu"&gt;
      &lt;h2&gt;Menu&lt;/h2&gt;
      &lt;article&gt;&lt;h3&gt;Pour Over&lt;/h3&gt;&lt;p&gt;Single origin, brewed to order.&lt;/p&gt;&lt;/article&gt;
    &lt;/section&gt;
    &lt;aside&gt;&lt;p&gt;Ask about our roasting classes.&lt;/p&gt;&lt;/aside&gt;
  &lt;/main&gt;
  &lt;footer&gt;&lt;p&gt;© Studio Ridge Coffee&lt;/p&gt;&lt;/footer&gt;
&lt;/body&gt;</pre>
      <div class="cap">Every tag here answers "what is this region for," not "how should it look."</div>
    </div>

    <div class="section-label">Div Soup vs. Semantic Markup</div>
    <table class="cmp">
      <tr><th>Signal</th><th>Div Soup</th><th>Semantic HTML</th></tr>
      <tr><td>Screen readers</td><td>Announce "group" repeatedly, no landmarks to jump between</td><td>Announce "navigation," "main," "footer" — users can jump straight to content</td></tr>
      <tr><td>SEO</td><td>Search engines must guess importance from position alone</td><td>&lt;article&gt;/&lt;h1-h6&gt; give explicit structural hints</td></tr>
      <tr><td>Team readability</td><td>Every region looks identical in the markup; intent lives only in class names</td><td>Tag names document intent directly, independent of styling</td></tr>
      <tr><td>Styling</td><td>Same amount of CSS either way</td><td>Same amount of CSS either way</td></tr>
    </table>

    <div class="misconception">
      <div class="lbl">Common Misconception</div>
      "Semantic tags automatically make my page accessible." They help, but they're a foundation, not a finish line — you still need meaningful alt text, correct heading order, sufficient color contrast, and keyboard-operable interactive elements.
    </div>
    <div class="misconception">
      <div class="lbl">Common Misconception</div>
      "&lt;section&gt; is just a styling wrapper, like a div with a nicer name." A &lt;section&gt; should represent a distinct thematic chunk with its own heading. If you're reaching for a tag purely to hang a CSS class on it with no thematic meaning, a plain &lt;div&gt; is the semantically correct choice.
    </div>

    <div class="exercise sheet">
      <div class="lbl">Practical Exercise</div>
      <p>Which semantic tag fits each scenario? Pick one, then reveal the answer.</p>
      <p><strong>1.</strong> A blog's "you might also like" list of three unrelated post links, placed after the main article.
      <select id="ex1sel" style="font-family:var(--font-display); background:var(--surface-2); color:var(--ink); border:1px solid var(--line-strong); padding:6px; border-radius:var(--radius); margin-left:8px;">
        <option value="">Choose…</option><option>&lt;aside&gt;</option><option>&lt;section&gt;</option><option>&lt;article&gt;</option><option>&lt;div&gt;</option>
      </select></p>
      <button class="btn reveal-btn" data-reveal="ex1reveal">Check Answer</button>
      <div class="reveal-content" id="ex1reveal"><strong>&lt;aside&gt;</strong> — it's related to the article but separable from its core meaning; removing it wouldn't damage the post itself.</div>
    </div>

    <div class="takeaways sheet">
      <h4>Key Takeaways</h4>
      <ul class="list-check">
        <li>Semantic tags describe <em>meaning</em>, not appearance — styling is CSS's job either way</li>
        <li>One &lt;main&gt; per page; &lt;article&gt; for standalone content; &lt;section&gt; for a themed chunk with its own heading</li>
        <li>Semantics benefit screen readers, search engines, and future-you reading the markup</li>
      </ul>
    </div>

    <!-- QUIZ 1 -->
    <div class="quiz sheet" id="quiz-1"></div>
  </section>

  <!-- ============ MODULE 2 — BOX MODEL ============ -->
  <section class="module locked" id="module-2">
    <div class="module-head"><span class="module-num">02 / 04</span><h2>CSS Box Model, Positioning &amp; Display Types</h2></div>
    <p class="module-sub">Every element is a box — this module controls its size, spacing, and place on the page.</p>

    <p>Every rendered element is a rectangular box made of four layers, from the inside out: <strong>content</strong>, <strong>padding</strong>, <strong>border</strong>, and <strong>margin</strong>. Padding pushes content away from the border on the inside; margin pushes other elements away from the border on the outside. Confusing the two is the single most common layout bug in CSS.</p>

    <div class="analogy">
      <div class="lbl">Analogy</div>
      Picture a framed photograph. The photo itself is the <strong>content</strong>. The mat board around it is <strong>padding</strong> — space that's still inside the frame. The wooden frame itself is the <strong>border</strong>. The gap you leave on the wall between this frame and the next one is the <strong>margin</strong> — it belongs to neither frame, it's the space between them.
    </div>

    <div class="section-label">Interactive Diagram — Drag the Sliders</div>
    <div class="boxmodel-wrap sheet">
      <div class="bm-controls">
        <div><label>Margin: <span class="val" id="mVal">20px</span></label><input type="range" id="mRange" min="0" max="50" value="20"></div>
        <div><label>Border: <span class="val" id="bVal">4px</span></label><input type="range" id="bRange" min="0" max="20" value="4"></div>
        <div><label>Padding: <span class="val" id="pVal">18px</span></label><input type="range" id="pRange" min="0" max="50" value="18"></div>
      </div>
      <div class="bm-visual">
        <div class="bm-margin" id="bmMargin" style="padding:20px;">
          <span class="bm-tag">margin</span>
          <div class="bm-border" id="bmBorder" style="border-width:4px;">
            <span class="bm-tag">border</span>
            <div class="bm-padding" id="bmPadding" style="padding:18px;">
              <span class="bm-tag">padding</span>
              <div class="bm-content">content</div>
            </div>
          </div>
        </div>
      </div>
      <p style="text-align:center; font-size:12.5px; color:var(--ink-dim);">box-sizing: content-box — width/height apply to <em>content only</em>; padding &amp; border are added on top.</p>
    </div>

    <div class="section-label">Positioning — Live Demo</div>
    <div class="boxmodel-wrap sheet">
      <div class="gp-controls">
        <div class="gp-field"><label>position</label>
          <select id="posSelect">
            <option value="static">static</option>
            <option value="relative">relative</option>
            <option value="absolute">absolute</option>
            <option value="sticky">sticky</option>
          </select>
        </div>
      </div>
      <div style="position:relative; height:150px; border:1px dashed var(--line-strong); overflow:auto; padding:10px;">
        <div style="background:var(--surface-2); padding:8px; margin-bottom:6px; font-size:12px; font-family:var(--font-display);">sibling box A</div>
        <div id="posDemo" style="background:var(--accent); color:var(--accent-ink); padding:8px; font-family:var(--font-display); font-size:12px; top:10px; left:10px;">moving box (id=posDemo)</div>
        <div style="background:var(--surface-2); padding:8px; margin-top:6px; font-size:12px; font-family:var(--font-display);">sibling box B</div>
        <div style="height:220px;"></div>
      </div>
      <p class="vp-hint" id="posExplain">static: box follows normal document flow; top/left/right/bottom are ignored.</p>
    </div>

    <table class="cmp">
      <tr><th>Value</th><th>Removed from flow?</th><th>Positioned relative to</th></tr>
      <tr><td><code>static</code></td><td>No</td><td>Normal document flow (default)</td></tr>
      <tr><td><code>relative</code></td><td>No — space is preserved</td><td>Its own original position</td></tr>
      <tr><td><code>absolute</code></td><td>Yes</td><td>Nearest positioned ancestor (or viewport)</td></tr>
      <tr><td><code>fixed</code></td><td>Yes</td><td>The browser viewport, ignores scroll</td></tr>
      <tr><td><code>sticky</code></td><td>No, until threshold</td><td>Flow, then locks at a scroll threshold</td></tr>
    </table>

    <div class="section-label">Display Types</div>
    <div class="two-col">
      <div class="example-box sheet"><pre>display: block;
/* full width, starts on new line
   e.g. div, p, section, h1 */</pre></div>
      <div class="example-box sheet"><pre>display: inline;
/* only as wide as content,
   no new line, ignores width/height
   e.g. span, a, strong */</pre></div>
      <div class="example-box sheet"><pre>display: inline-block;
/* flows inline BUT
   respects width/height/padding
   e.g. buttons, nav links */</pre></div>
      <div class="example-box sheet"><pre>display: none;
/* removed entirely — takes
   up zero space, unlike
   visibility: hidden */</pre></div>
    </div>

    <div class="misconception">
      <div class="lbl">Common Misconception</div>
      "Margin and padding are basically interchangeable — pick whichever fixes the gap." They behave differently on collision: adjacent vertical margins can <em>collapse</em> into a single margin, padding never does. If a gap looks smaller than the sum of two margins, that's collapsing, not a bug.
    </div>
    <div class="misconception">
      <div class="lbl">Common Misconception</div>
      "<code>position: absolute</code> always positions relative to the whole page." It positions relative to the nearest ancestor that has a position other than <code>static</code> — forgetting to set that ancestor to <code>relative</code> is the most common reason an absolutely positioned element ends up in the wrong place.
    </div>

    <div class="exercise sheet">
      <div class="lbl">Practical Exercise</div>
      <p>A button needs a fixed width and height <em>and</em> to sit inline next to text in a sentence. Which display value achieves both?</p>
      <select id="ex2sel" style="font-family:var(--font-display); background:var(--surface-2); color:var(--ink); border:1px solid var(--line-strong); padding:6px; border-radius:var(--radius);">
        <option value="">Choose…</option><option>block</option><option>inline</option><option>inline-block</option><option>none</option>
      </select>
      <button class="btn reveal-btn" data-reveal="ex2reveal">Check Answer</button>
      <div class="reveal-content" id="ex2reveal"><strong>inline-block</strong> — it flows with surrounding text like <code>inline</code>, but still respects an explicit width and height like <code>block</code>.</div>
    </div>

    <div class="takeaways sheet">
      <h4>Key Takeaways</h4>
      <ul class="list-check">
        <li>Padding = inside the border, margin = outside the border; they solve different problems</li>
        <li><code>absolute</code> needs a positioned ancestor to behave predictably</li>
        <li><code>inline-block</code> is the middle ground between <code>block</code> and <code>inline</code></li>
      </ul>
    </div>

    <div class="quiz sheet" id="quiz-2"></div>
  </section>

  <!-- ============ MODULE 3 — GRID ============ -->
  <section class="module locked" id="module-3">
    <div class="module-head"><span class="module-num">03 / 04</span><h2>CSS Grid Layout System</h2></div>
    <p class="module-sub">Applying structure and box control to arrange whole regions of a page at once.</p>

    <p>CSS Grid turns a container into a two-dimensional coordinate system: rows <em>and</em> columns at the same time. You define the structure once on the parent with <code>grid-template-columns</code> / <code>grid-template-rows</code>, and every child simply falls into a cell — no floats, no manual math.</p>

    <div class="analogy">
      <div class="lbl">Analogy</div>
      Grid is graph paper. You draw the column and row lines first — how wide, how tall — then place items into specific squares. Flexbox, by contrast, is closer to a row of people lining up shoulder-to-shoulder: it's one-dimensional, arranging items along a single line that wraps.
    </div>

    <div class="section-label">Interactive Diagram — Build a Grid</div>
    <div class="grid-play sheet">
      <div class="gp-controls">
        <div class="gp-field"><label>Columns</label>
          <select id="gCols"><option value="1fr 1fr">2 equal</option><option value="1fr 1fr 1fr" selected>3 equal</option><option value="2fr 1fr 1fr">1 wide + 2</option><option value="repeat(auto-fit,minmax(120px,1fr))">auto-fit</option></select>
        </div>
        <div class="gp-field"><label>Gap</label>
          <input type="range" id="gGap" min="0" max="30" value="10">
          <span class="val mono" id="gGapVal">10px</span>
        </div>
        <div class="gp-field"><label>Items</label>
          <input type="range" id="gItems" min="3" max="9" value="6">
        </div>
      </div>
      <div class="gp-grid" id="gpGrid"></div>
      <div class="gp-code" id="gpCode"></div>
    </div>

    <table class="cmp">
      <tr><th></th><th>CSS Grid</th><th>Flexbox</th></tr>
      <tr><td>Dimensions</td><td>Two: rows and columns together</td><td>One: a single row or column</td></tr>
      <tr><td>Best for</td><td>Overall page layout, dashboards, image galleries</td><td>Navigation bars, toolbars, aligning items along one axis</td></tr>
      <tr><td>Item placement</td><td>Can be placed explicitly by line/area</td><td>Flows in source order along the main axis</td></tr>
      <tr><td>Combine?</td><td colspan="2" style="text-align:center;">Yes — Grid for page regions, Flexbox inside a region</td></tr>
    </table>

    <div class="section-label">Named Areas Example</div>
    <div class="example-box sheet">
<pre>.page {
  display: grid;
  grid-template-columns: 220px 1fr;
  grid-template-areas:
    "sidebar header"
    "sidebar main"
    "sidebar footer";
}
.sidebar { grid-area: sidebar; }
.header  { grid-area: header;  }
.main    { grid-area: main;    }
.footer  { grid-area: footer;  }</pre>
      <div class="cap">Naming areas turns the CSS itself into a readable ASCII wireframe of the page.</div>
    </div>

    <div class="misconception">
      <div class="lbl">Common Misconception</div>
      "I should always use Grid instead of Flexbox now." They solve different problems. A single row of nav links is simpler and more robust as Flexbox; a page-level layout with distinct regions is simpler as Grid. Most real pages use both, nested.
    </div>
    <div class="misconception">
      <div class="lbl">Common Misconception</div>
      "<code>fr</code> units are percentages." <code>fr</code> distributes <em>remaining</em> space after fixed-size tracks and gaps are subtracted — <code>1fr 1fr</code> isn't always exactly 50/50 if other tracks or gaps are present.
    </div>

    <div class="exercise sheet">
      <div class="lbl">Practical Exercise</div>
      <p>You need a 3-column card gallery where columns automatically wrap to fewer columns on narrow screens, without writing a media query. Which value of <code>grid-template-columns</code> does that?</p>
      <select id="ex3sel" style="font-family:var(--font-display); background:var(--surface-2); color:var(--ink); border:1px solid var(--line-strong); padding:6px; border-radius:var(--radius);">
        <option value="">Choose…</option>
        <option>1fr 1fr 1fr</option>
        <option>repeat(3, 1fr)</option>
        <option>repeat(auto-fit, minmax(150px, 1fr))</option>
        <option>200px 200px 200px</option>
      </select>
      <button class="btn reveal-btn" data-reveal="ex3reveal">Check Answer</button>
      <div class="reveal-content" id="ex3reveal"><strong>repeat(auto-fit, minmax(150px, 1fr))</strong> — the grid recalculates how many 150px+ columns fit as the container shrinks, with zero media queries required.</div>
    </div>

    <div class="takeaways sheet">
      <h4>Key Takeaways</h4>
      <ul class="list-check">
        <li>Grid is two-dimensional; Flexbox is one-dimensional — pick based on the shape of the problem</li>
        <li><code>fr</code> shares out remaining space, not a fixed percentage</li>
        <li><code>auto-fit</code> + <code>minmax()</code> gives responsive columns without a single media query</li>
      </ul>
    </div>

    <div class="quiz sheet" id="quiz-3"></div>
  </section>

  <!-- ============ MODULE 4 — RESPONSIVE ============ -->
  <section class="module locked" id="module-4">
    <div class="module-head"><span class="module-num">04 / 04</span><h2>Responsive Design with Media Queries (Mobile-First)</h2></div>
    <p class="module-sub">Mastery — combining structure and layout so the page adapts to any screen.</p>

    <p>Mobile-first means writing your base CSS for the smallest, simplest layout first, then layering on complexity as screen space becomes available, using <code>min-width</code> media queries. Each query only adds rules — it never has to fight rules meant for a bigger screen.</p>

    <div class="analogy">
      <div class="lbl">Analogy</div>
      It's like packing for a trip by starting with the essentials that fit in a small bag, then adding items only as you're handed a bigger suitcase. Desktop-first is the opposite: you pack the biggest suitcase first, then have to unpack and repack items every time you're handed a smaller bag.
    </div>

    <div class="section-label">Interactive Diagram — Drag the Frame's Corner to Resize</div>
    <div class="vp-demo sheet">
      <div class="vp-badge">Current layout</div>
      <div class="vp-frame-outer">
        <div class="vp-frame" id="vpFrame">
          <div class="vp-content">
            <div class="vp-card">Card A</div>
            <div class="vp-card">Card B</div>
            <div class="vp-card">Card C</div>
          </div>
        </div>
      </div>
      <p class="vp-hint">Drag the bottom-right corner of the frame above. This uses native CSS <code>@container</code> queries — the layout responds to the <em>frame's</em> width, not the browser window's.</p>
    </div>

    <div class="section-label">Mobile-First vs. Desktop-First</div>
    <div class="two-col">
      <div class="example-box sheet">
<pre>/* Mobile-first (recommended) */
.card { width: 100%; }

@media (min-width: 600px) {
  .card { width: 50%; }
}
@media (min-width: 900px) {
  .card { width: 33.3%; }
}</pre>
        <div class="cap">Base styles ARE the mobile styles. Each query only adds.</div>
      </div>
      <div class="example-box sheet">
<pre>/* Desktop-first (legacy pattern) */
.card { width: 33.3%; }

@media (max-width: 899px) {
  .card { width: 50%; }
}
@media (max-width: 599px) {
  .card { width: 100%; }
}</pre>
        <div class="cap">Base styles target desktop; queries override downward, fighting earlier rules.</div>
      </div>
    </div>

    <table class="cmp">
      <tr><th>Common Breakpoint</th><th>Rough Target</th></tr>
      <tr><td><code>min-width: 480px</code></td><td>Large phones, phablets</td></tr>
      <tr><td><code>min-width: 768px</code></td><td>Tablets, small laptops</td></tr>
      <tr><td><code>min-width: 1024px</code></td><td>Laptops, desktops</td></tr>
      <tr><td><code>min-width: 1440px</code></td><td>Large/high-res desktop displays</td></tr>
    </table>

    <div class="misconception">
      <div class="lbl">Common Misconception</div>
      "Breakpoints should match specific real devices, like 'iPhone width.'" Devices and their resolutions change constantly. Choose breakpoints where <em>your content</em> starts to look cramped or awkwardly spaced — not where a particular phone happens to measure today.
    </div>
    <div class="misconception">
      <div class="lbl">Common Misconception</div>
      "Responsive design is just about shrinking things to fit." It's about rearranging — a 3-column grid might genuinely need to become 1 column with reordered priority content, not just a smaller version of the same layout.
    </div>

    <div class="exercise sheet">
      <div class="lbl">Practical Exercise</div>
      <p>Which media query correctly follows the mobile-first pattern to add a two-column layout starting at tablet width?</p>
      <select id="ex4sel" style="font-family:var(--font-display); background:var(--surface-2); color:var(--ink); border:1px solid var(--line-strong); padding:6px; border-radius:var(--radius);">
        <option value="">Choose…</option>
        <option>@media (max-width: 768px) { .grid { grid-template-columns: 1fr 1fr; } }</option>
        <option>@media (min-width: 768px) { .grid { grid-template-columns: 1fr 1fr; } }</option>
        <option>@media (width: 768px) { .grid { grid-template-columns: 1fr 1fr; } }</option>
      </select>
      <button class="btn reveal-btn" data-reveal="ex4reveal">Check Answer</button>
      <div class="reveal-content" id="ex4reveal"><strong>min-width: 768px</strong> — it adds the two-column rule once the viewport reaches at least tablet width, leaving the single-column mobile base untouched below that.</div>
    </div>

    <div class="takeaways sheet">
      <h4>Key Takeaways</h4>
      <ul class="list-check">
        <li>Mobile-first uses <code>min-width</code> and only ever adds rules going up</li>
        <li>Pick breakpoints where your own content breaks, not fixed device sizes</li>
        <li>Responsive means rearranging content, not just shrinking it uniformly</li>
      </ul>
    </div>

    <div class="quiz sheet" id="quiz-4"></div>
  </section>

  <!-- ============ FINAL CHALLENGE ============ -->
  <section class="module locked" id="final-challenge">
    <div class="module-head"><span class="module-num">FINAL</span><h2>Practical Challenge — Assemble the Full Page</h2></div>
    <p class="module-sub">Combine all four modules into one working responsive page.</p>
    <div class="challenge sheet">
      <p>Edit the HTML on the left. The preview updates live on the right. Your goal: build a mini page with a <code>&lt;header&gt;</code>, a <code>&lt;nav&gt;</code>, a <code>&lt;main&gt;</code> containing a Grid gallery of at least 3 cards, and a <code>&lt;footer&gt;</code> — styled with box-model spacing and at least one media query.</p>
      <div class="editor-grid">
        <textarea id="challengeEditor" spellcheck="false">&lt;style&gt;
  body{font-family:sans-serif; margin:0; padding:16px;}
  header{background:#222;color:#fff;padding:12px;}
  .gallery{display:grid; grid-template-columns:1fr; gap:10px; margin-top:12px;}
  .card{border:1px solid #ccc; padding:10px; border-radius:4px;}
  @media (min-width:480px){
    .gallery{grid-template-columns:1fr 1fr;}
  }
&lt;/style&gt;
&lt;header&gt;&lt;h1&gt;My Page&lt;/h1&gt;&lt;/header&gt;
&lt;main&gt;
  &lt;div class="gallery"&gt;
    &lt;div class="card"&gt;Card 1&lt;/div&gt;
    &lt;div class="card"&gt;Card 2&lt;/div&gt;
    &lt;div class="card"&gt;Card 3&lt;/div&gt;
  &lt;/div&gt;
&lt;/main&gt;
&lt;footer&gt;© You&lt;/footer&gt;</textarea>
        <iframe id="challengePreview" title="Live preview"></iframe>
      </div>
      <div class="checklist">
        <label><input type="checkbox" class="chk"> Used semantic tags (header / nav / main / footer)</label>
        <label><input type="checkbox" class="chk"> Adjusted padding/margin deliberately, not by accident</label>
        <label><input type="checkbox" class="chk"> Used CSS Grid for the gallery region</label>
        <label><input type="checkbox" class="chk"> Included at least one mobile-first media query</label>
        <label><input type="checkbox" class="chk"> Resized the preview frame (Module 4 demo) to confirm it adapts</label>
      </div>
      <div class="cta-row"><button class="btn primary" id="completeChallenge">Mark Challenge Complete</button></div>
      <div class="stamp" id="challengeStamp" style="margin-top:14px;">✓ Page Approved for Deployment</div>
    </div>
  </section>

  <!-- ============ CHEAT SHEET ============ -->
  <section id="cheatsheet" style="margin-top:64px;">
    <div class="section-label" style="margin-top:0;">Cheat Sheet</div>
    <div class="cheat-grid">
      <div class="cheat-card sheet"><h4>Semantic Skeleton</h4><pre>header · nav · main
  section (themed, has heading)
  article (standalone content)
  aside (related, separable)
footer</pre></div>
      <div class="cheat-card sheet"><h4>Box Model</h4><pre>margin  → outside border
border  → the frame
padding → inside border
content → the box itself
box-sizing: border-box;
  (width includes padding+border)</pre></div>
      <div class="cheat-card sheet"><h4>Positioning</h4><pre>static   → normal flow
relative → offset, space kept
absolute → out of flow, nearest
           positioned ancestor
fixed    → out of flow, viewport
sticky   → flow, then locks</pre></div>
      <div class="cheat-card sheet"><h4>Grid Essentials</h4><pre>display: grid;
grid-template-columns: 1fr 1fr;
gap: 16px;
grid-template-areas: "a b";
repeat(auto-fit,minmax(150px,1fr))</pre></div>
      <div class="cheat-card sheet"><h4>Mobile-First Query</h4><pre>/* base = mobile */
@media (min-width: 768px){ ... }
@media (min-width: 1024px){ ... }</pre></div>
      <div class="cheat-card sheet"><h4>Display Types</h4><pre>block        → full width, new line
inline       → flows, ignores w/h
inline-block → flows, respects w/h
none         → removed entirely</pre></div>
    </div>
  </section>

  <!-- ============ SUMMARY NOTES ============ -->
  <section style="margin-top:56px;">
    <div class="section-label" style="margin-top:0;">Summary Notes</div>
    <div class="card sheet">
      <p style="font-size:14.5px;">A complete responsive page is built in layers: semantic HTML gives every region a name and a role; the box model governs spacing and sizing at the element level; Grid arranges regions in two dimensions; and mobile-first media queries let that structure adapt gracefully from a phone to a wide desktop. None of these four skills works in isolation — a semantically perfect page with no responsive CSS still breaks on mobile, and a beautifully responsive layout built on div soup is still hard to navigate and maintain. Master all four together, and you can read, debug, and build the structural layer of nearly any web page.</p>
    </div>
  </section>

  <!-- ============ CONTINUE LEARNING ============ -->
  <section style="margin-top:56px;">
    <div class="section-label" style="margin-top:0;">Continue Learning</div>
    <div class="res-grid">
      <div class="res-card sheet"><h4>Books</h4><ul>
        <li>"HTML &amp; CSS: Design and Build Websites" — Jon Duckett</li>
        <li>"CSS: The Definitive Guide" — Eric Meyer &amp; Estelle Weyl</li>
        <li>"Responsive Web Design" — Ethan Marcotte</li>
      </ul></div>
      <div class="res-card sheet"><h4>Documentation</h4><ul>
        <li>MDN Web Docs — HTML elements reference</li>
        <li>MDN — CSS Box Model guide</li>
        <li>MDN — CSS Grid Layout guide</li>
        <li>web.dev — Responsive Design fundamentals</li>
      </ul></div>
      <div class="res-card sheet"><h4>Standards &amp; Specs</h4><ul>
        <li>WHATWG HTML Living Standard</li>
        <li>W3C CSS Grid Layout Module Level 1 spec</li>
        <li>W3C CSS Box Sizing Module spec</li>
      </ul></div>
      <div class="res-card sheet"><h4>Communities</h4><ul>
        <li>MDN Web Docs Discourse forum</li>
        <li>r/css and r/webdev on Reddit</li>
        <li>CSS-Tricks community &amp; almanac</li>
        <li>Frontend Masters community Discord</li>
      </ul></div>
      <div class="res-card sheet"><h4>Practice Platforms</h4><ul>
        <li>Frontend Mentor (real design-to-code challenges)</li>
        <li>CSS Grid Garden &amp; Flexbox Froggy (game-based drills)</li>
        <li>CodePen (quick layout experiments)</li>
      </ul></div>
      <div class="res-card sheet"><h4>Search Keywords</h4>
        <div class="kw-chip-row">
          <span class="kw-chip">semantic html landmarks</span>
          <span class="kw-chip">css box-sizing border-box</span>
          <span class="kw-chip">css grid-template-areas</span>
          <span class="kw-chip">mobile-first media queries</span>
          <span class="kw-chip">container queries css</span>
        </div>
      </div>
    </div>
    <div class="card sheet" style="margin-top:16px;">
      <h4 style="font-size:12.5px; text-transform:uppercase; letter-spacing:.06em; color:var(--accent);">Additional AI Prompts for Further Learning</h4>
      <ul class="list-check" style="margin-top:10px;">
        <li>"Review this HTML page and tell me where I could replace a &lt;div&gt; with a more meaningful semantic tag."</li>
        <li>"Explain the difference between box-sizing: content-box and border-box with a worked pixel example."</li>
        <li>"Give me three CSS Grid layout challenges of increasing difficulty, with no solutions, so I can attempt them myself."</li>
        <li>"Critique my media query breakpoints and tell me if they're based on content or on specific devices."</li>
      </ul>
    </div>
  </section>

  <footer>Interactive Learning Studio · Building a Complete Responsive Page · Built for hands-on, self-paced learning</footer>
</div>

<script>
(function(){
  "use strict";

  /* ---------------- THEME ---------------- */
  var html = document.documentElement;
  document.getElementById('themeToggle').addEventListener('click', function(){
    html.setAttribute('data-theme', html.getAttribute('data-theme') === 'dark' ? 'light' : 'dark');
  });
  document.getElementById('printBtn').addEventListener('click', function(){ window.print(); });
  document.getElementById('startBtn').addEventListener('click', function(){
    document.getElementById('module-1').scrollIntoView({behavior:'smooth'});
  });
  document.getElementById('cheatJump').addEventListener('click', function(){
    document.getElementById('cheatsheet').scrollIntoView({behavior:'smooth'});
  });

  /* ---------------- STATE ---------------- */
  var state = {
    modulesDone: [false,false,false,false],
    quizScores: [null,null,null,null],
    challengeDone: false
  };
  var POINTS_PER_QUIZ = 80; // 4 quizzes x 4 questions x 5 pts = 80, total 320 + 80 challenge = 400
  var badges = ["Skeleton Builder","Box Model Pro","Grid Architect","Responsive Master"];

  function updateProgress(){
    var doneCount = state.modulesDone.filter(Boolean).length;
    var totalScore = state.quizScores.reduce(function(a,b){return a + (b||0);}, 0);
    var challengePts = state.challengeDone ? 80 : 0;
    var totalPts = totalScore + challengePts;
    var pct = Math.round(((doneCount) / 4) * 90 + (state.challengeDone ? 10 : 0));
    document.getElementById('progressFill').style.width = pct + '%';
    document.getElementById('progressLabel').textContent = pct + '% built';
    document.getElementById('totalPointsLabel').textContent = totalPts + ' / 400';
    document.getElementById('modulesDoneLabel').textContent = doneCount + ' / 4';
    var scored = state.quizScores.filter(function(s){return s!==null;});
    document.getElementById('quizAvgLabel').textContent = scored.length ? Math.round(scored.reduce(function(a,b){return a+b;},0)/scored.length/80*100) + '%' : '—';

    var strip = document.getElementById('badgeStrip');
    strip.innerHTML = '';
    badges.forEach(function(b,i){
      var el = document.createElement('span');
      el.className = 'badge' + (state.modulesDone[i] ? ' earned' : '');
      el.textContent = (state.modulesDone[i] ? '★ ' : '☆ ') + b;
      strip.appendChild(el);
    });
    if(doneCount === 4){ document.getElementById('finalStamp').classList.add('show'); }
  }
  updateProgress();

  function unlockModule(n){
    var el = document.getElementById('module-' + n);
    if(el){ el.classList.remove('locked'); }
    if(n === 4){
      document.getElementById('final-challenge').classList.remove('locked');
    }
  }

  /* ---------------- SEMANTIC SKELETON TOGGLE ---------------- */
  document.querySelectorAll('#skeletonDiagram .sk-tag').forEach(function(tag){
    tag.addEventListener('click', function(){
      var open = tag.classList.contains('open');
      if(!open){
        var desc = document.createElement('div');
        desc.className = 'desc open';
        desc.textContent = tag.getAttribute('data-desc');
        if(!tag.querySelector('.desc')){ tag.appendChild(desc); }
        tag.classList.add('open');
      } else {
        tag.classList.remove('open');
        var d = tag.querySelector('.desc');
        if(d){ d.classList.remove('open'); }
      }
    });
  });

  /* ---------------- REVEAL BUTTONS (exercises) ---------------- */
  document.querySelectorAll('.reveal-btn').forEach(function(btn){
    btn.addEventListener('click', function(){
      var target = document.getElementById(btn.getAttribute('data-reveal'));
      target.classList.toggle('open');
    });
  });

  /* ---------------- BOX MODEL SLIDERS ---------------- */
  var mR = document.getElementById('mRange'), bR = document.getElementById('bRange'), pR = document.getElementById('pRange');
  function updateBoxModel(){
    document.getElementById('mVal').textContent = mR.value + 'px';
    document.getElementById('bVal').textContent = bR.value + 'px';
    document.getElementById('pVal').textContent = pR.value + 'px';
    document.getElementById('bmMargin').style.padding = mR.value + 'px';
    document.getElementById('bmBorder').style.borderWidth = bR.value + 'px';
    document.getElementById('bmPadding').style.padding = pR.value + 'px';
  }
  [mR,bR,pR].forEach(function(el){ el.addEventListener('input', updateBoxModel); });
  updateBoxModel();

  /* ---------------- POSITIONING DEMO ---------------- */
  var posSelect = document.getElementById('posSelect');
  var posDemo = document.getElementById('posDemo');
  var posExplain = document.getElementById('posExplain');
  var explains = {
    static: "static: box follows normal document flow; top/left/right/bottom are ignored.",
    relative: "relative: box stays in flow (space reserved) but is now visually offset from its original spot.",
    absolute: "absolute: box leaves the flow entirely and positions relative to the nearest positioned ancestor — here, the dashed container.",
    sticky: "sticky: box scrolls normally until it hits the defined offset, then locks in place within its container."
  };
  posSelect.addEventListener('change', function(){
    var v = posSelect.value;
    posDemo.style.position = v;
    if(v === 'relative'){ posDemo.style.top = '10px'; posDemo.style.left = '20px'; }
    else if(v === 'absolute'){ posDemo.style.top = '4px'; posDemo.style.right = '4px'; posDemo.style.left='auto'; }
    else if(v === 'sticky'){ posDemo.style.top = '0px'; posDemo.style.left='auto'; posDemo.style.right='auto';}
    else { posDemo.style.top=''; posDemo.style.left=''; posDemo.style.right=''; }
    posExplain.textContent = explains[v];
  });

  /* ---------------- GRID PLAYGROUND ---------------- */
  var gCols = document.getElementById('gCols'), gGap = document.getElementById('gGap'), gItems = document.getElementById('gItems');
  var gpGrid = document.getElementById('gpGrid'), gpCode = document.getElementById('gpCode');
  function renderGrid(){
    gpGrid.style.gridTemplateColumns = gCols.value;
    gpGrid.style.gap = gGap.value + 'px';
    document.getElementById('gGapVal').textContent = gGap.value + 'px';
    gpGrid.innerHTML = '';
    for(var i=1;i<=parseInt(gItems.value,10);i++){
      var d = document.createElement('div');
      d.className = 'gp-item';
      d.textContent = 'Item ' + i;
      gpGrid.appendChild(d);
    }
    gpCode.textContent = '.gallery {\n  display: grid;\n  grid-template-columns: ' + gCols.value + ';\n  gap: ' + gGap.value + 'px;\n}';
  }
  [gCols,gGap,gItems].forEach(function(el){ el.addEventListener('input', renderGrid); el.addEventListener('change', renderGrid); });
  renderGrid();

  /* ---------------- QUIZ ENGINE ---------------- */
  var quizzes = [
    { id:1, title:"Module 01 Check — Semantic HTML", questions:[
      {q:"Which element should appear only once at the document level and hold the page's primary content?", opts:["<section>","<main>","<div>","<article>"], correct:1, exp:"<main> represents the dominant content of the document and should not be duplicated at the page level."},
      {q:"A blog post that could stand alone in an RSS feed should be wrapped in which tag?", opts:["<aside>","<section>","<article>","<nav>"], correct:2, exp:"<article> is for self-contained, independently distributable content — exactly the RSS-feed test."},
      {q:"Why prefer semantic tags over an all-<div> structure?", opts:["They make CSS shorter automatically","They give assistive technology and search engines explicit structural meaning","They are required for the page to render","They remove the need for a stylesheet"], correct:1, exp:"Semantics don't change how much CSS you write — they communicate structure and meaning to screen readers, browsers, and search engines."},
      {q:"A sidebar of related links next to a blog post, removable without harming the post's meaning, belongs in:", opts:["<footer>","<aside>","<header>","<main>"], correct:1, exp:"<aside> is for tangential content that's related but separable — the definition of a sidebar of related links."}
    ]},
    { id:2, title:"Module 02 Check — Box Model & Positioning", questions:[
      {q:"Which box-model layer sits directly between the border and the content?", opts:["Margin","Padding","Outline","Another border"], correct:1, exp:"Order from outside in: margin → border → padding → content."},
      {q:"An element with position: absolute and no positioned ancestor will be positioned relative to:", opts:["Its nearest sibling","The <body> / initial containing block (viewport)","Its own default position","Nothing — it becomes invisible"], correct:1, exp:"With no positioned ancestor, absolute positioning falls back to the initial containing block, effectively the viewport."},
      {q:"Which display value keeps an element inline with text while still respecting an explicit width and height?", opts:["block","inline","inline-block","none"], correct:2, exp:"inline-block combines inline flow with block-style sizing — the best of both."},
      {q:"Two vertically stacked elements with margin-bottom: 20px and margin-top: 20px might produce a gap smaller than 40px because of:", opts:["A CSS bug in the browser","Margin collapsing","Padding overriding margin","box-sizing: border-box"], correct:1, exp:"Adjacent vertical margins can collapse into a single margin equal to the larger of the two — a well-defined CSS behavior, not a bug."}
    ]},
    { id:3, title:"Module 03 Check — CSS Grid", questions:[
      {q:"CSS Grid is best described as:", opts:["A one-dimensional layout system","A two-dimensional layout system (rows and columns)","A replacement for all HTML tags","A JavaScript animation library"], correct:1, exp:"Grid controls rows and columns simultaneously, unlike Flexbox's single axis."},
      {q:"In grid-template-columns: 1fr 2fr, the second column will be:", opts:["Exactly twice as wide as the first, of remaining space","Fixed at 2 pixels","Ignored by the browser","Equal to the first column"], correct:0, exp:"fr units divide the remaining available space proportionally — 1fr 2fr splits that space 1:2."},
      {q:"Which value makes a grid automatically add or remove columns as the container resizes, with no media query?", opts:["grid-template-columns: 3, 1fr","repeat(auto-fit, minmax(150px, 1fr))","display: flex","grid-gap: auto"], correct:1, exp:"auto-fit with minmax() recalculates how many tracks fit as space changes — fully responsive without breakpoints."},
      {q:"When should you reach for Flexbox instead of Grid?", opts:["Never, Grid replaces Flexbox entirely","For a single row or column of items, like a nav bar","For any layout with more than 2 items","Only inside a <table>"], correct:1, exp:"Flexbox shines for one-dimensional arrangements — a toolbar or nav bar is the classic case."}
    ]},
    { id:4, title:"Module 04 Check — Responsive Design", questions:[
      {q:"Mobile-first CSS primarily uses which type of media query?", opts:["max-width","min-width","orientation only","print"], correct:1, exp:"Mobile-first starts with base (mobile) styles and layers on complexity using min-width as the screen grows."},
      {q:"Why choose content-based breakpoints over device-specific ones?", opts:["Device sizes are standardized forever","New devices and resolutions constantly appear, but your content's needs don't change as often","Content-based breakpoints require less CSS","Browsers ignore device-based breakpoints"], correct:1, exp:"The device landscape keeps changing; breakpoints tied to where your own layout starts to break are more durable."},
      {q:"Responsive design fundamentally means:", opts:["Uniformly shrinking every element on smaller screens","Rearranging and prioritizing content for each screen size, not just scaling it down", "Hiding all images below 768px","Using only percentage widths"], correct:1, exp:"True responsiveness often restructures layout — e.g. 3 columns to 1 — not just proportionally shrinking the same arrangement."},
      {q:"CSS @container queries differ from @media queries because they respond to:", opts:["The user's operating system","The width of a specific container element, not the viewport","The number of DOM nodes on the page","The device's battery level"], correct:1, exp:"Container queries let a component adapt to the size of its own containing box, independent of overall viewport width."}
    ]}
  ];

  function renderQuiz(qz){
    var container = document.getElementById('quiz-' + qz.id);
    var html = '<h3>Quiz — ' + qz.title + '</h3><p style="font-size:13px;color:var(--ink-dim);">4 questions · instant feedback · pass with 3/4 or better to unlock the next module.</p>';
    qz.questions.forEach(function(q, qi){
      html += '<div class="q-item" data-qi="'+qi+'"><div class="q-text">'+ (qi+1) +'. ' + q.q + '</div><div class="q-opts">';
      q.opts.forEach(function(opt, oi){
        html += '<label class="q-opt" data-oi="'+oi+'"><input type="radio" name="q'+qz.id+'_'+qi+'" value="'+oi+'"><span>'+opt+'</span></label>';
      });
      html += '</div><div class="q-explain" id="exp-'+qz.id+'-'+qi+'"></div></div>';
    });
    html += '<button class="btn primary" id="submitQuiz'+qz.id+'">Submit Quiz</button>';
    html += '<div class="quiz-summary" id="summary'+qz.id+'"><div class="score" id="score'+qz.id+'"></div><p id="scoreMsg'+qz.id+'"></p><button class="btn primary" id="continueBtn'+qz.id+'" style="margin-top:10px;">Continue →</button></div>';
    container.innerHTML = html;

    container.querySelectorAll('.q-opt').forEach(function(lbl){
      lbl.addEventListener('click', function(){
        var qItem = lbl.closest('.q-item');
        qItem.querySelectorAll('.q-opt').forEach(function(o){ o.classList.remove('selected'); });
        lbl.classList.add('selected');
      });
    });

    document.getElementById('submitQuiz' + qz.id).addEventListener('click', function(){
      var correctCount = 0;
      qz.questions.forEach(function(q, qi){
        var qItem = container.querySelector('.q-item[data-qi="'+qi+'"]');
        var selected = qItem.querySelector('input:checked');
        var selIdx = selected ? parseInt(selected.value,10) : -1;
        var opts = qItem.querySelectorAll('.q-opt');
        opts.forEach(function(o, oi){
          o.style.pointerEvents = 'none';
          if(oi === q.correct){ o.classList.add('correct'); }
          else if(oi === selIdx){ o.classList.add('incorrect'); }
        });
        if(selIdx === q.correct){ correctCount++; }
        var exp = document.getElementById('exp-'+qz.id+'-'+qi);
        exp.textContent = (selIdx === q.correct ? '✓ Correct. ' : (selIdx === -1 ? '— No answer selected. ' : '✗ Not quite. ')) + q.exp;
        exp.classList.add('open');
      });
      var pts = correctCount * 20; // 4 questions x 20 = 80 max
      state.quizScores[qz.id - 1] = pts;
      var pct = Math.round(correctCount/4*100);
      document.getElementById('score' + qz.id).textContent = correctCount + ' / 4 (' + pct + '%)';
      var msg = pct >= 75 ? "Strong work — module unlocked." : (pct >= 50 ? "Passing, but review the explanations above before continuing." : "Below the pass threshold — review the module content and retry when ready.");
      document.getElementById('scoreMsg' + qz.id).textContent = msg;
      document.getElementById('summary' + qz.id).classList.add('open');
      document.getElementById('submitQuiz' + qz.id).style.display = 'none';

      var passed = pct >= 50;
      var continueBtn = document.getElementById('continueBtn' + qz.id);
      if(passed){
        state.modulesDone[qz.id - 1] = true;
        continueBtn.textContent = 'Continue to Next Section →';
        continueBtn.disabled = false;
      } else {
        continueBtn.textContent = 'Retry Quiz';
        continueBtn.disabled = false;
      }
      updateProgress();

      continueBtn.addEventListener('click', function(){
        if(passed){
          if(qz.id < 4){ unlockModule(qz.id + 1); document.getElementById('module-' + (qz.id+1)).scrollIntoView({behavior:'smooth'}); }
          else { unlockModule(4); document.getElementById('final-challenge').classList.remove('locked'); document.getElementById('final-challenge').scrollIntoView({behavior:'smooth'}); }
        } else {
          renderQuiz(qz);
          document.getElementById('quiz-' + qz.id).scrollIntoView({behavior:'smooth'});
        }
      }, {once:true});
    });
  }
  quizzes.forEach(renderQuiz);

  /* ---------------- FINAL CHALLENGE LIVE PREVIEW ---------------- */
  var editor = document.getElementById('challengeEditor');
  var preview = document.getElementById('challengePreview');
  function updatePreview(){ preview.srcdoc = editor.value; }
  editor.addEventListener('input', updatePreview);
  updatePreview();

  document.getElementById('completeChallenge').addEventListener('click', function(){
    state.challengeDone = true;
    document.getElementById('challengeStamp').classList.add('show');
    updateProgress();
  });

  /* ---------------- VIEWPORT DRAG HINT (native resize handled by CSS `resize`) ---------------- */
})();
</script>
</body>
</html>
