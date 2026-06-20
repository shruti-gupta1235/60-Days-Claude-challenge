<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>FacePuzzle — Slice & Solve</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@300;400;500;600;700&family=Space+Mono:wght@400;700&display=swap');
  *,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
  :root{
    --bg:#0a0a0f;--surface:#13131a;--surface2:#1c1c27;--border:#2a2a3d;
    --accent:#7c5cfc;--accent2:#c85cfc;--correct:#2ecc8a;--text:#e8e8f0;
    --text2:#8888aa;--glow:rgba(124,92,252,.35);--radius:16px;
    --font:'Space Grotesk',sans-serif;--mono:'Space Mono',monospace;
  }
  body{background:var(--bg);color:var(--text);font-family:var(--font);min-height:100vh;display:flex;flex-direction:column;align-items:center;overflow-x:hidden}
  body::before{content:'';position:fixed;inset:0;background-image:linear-gradient(rgba(124,92,252,.04) 1px,transparent 1px),linear-gradient(90deg,rgba(124,92,252,.04) 1px,transparent 1px);background-size:48px 48px;pointer-events:none;z-index:0}
  header{width:100%;padding:20px 32px;display:flex;align-items:center;justify-content:space-between;border-bottom:1px solid var(--border);position:relative;z-index:2;backdrop-filter:blur(10px);background:rgba(10,10,15,.7)}
  .logo{font-size:1.3rem;font-weight:700;letter-spacing:-.03em;background:linear-gradient(135deg,var(--accent),var(--accent2));-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text}
  .logo span{font-weight:300}
  .header-badge{font-family:var(--mono);font-size:.65rem;letter-spacing:.12em;color:var(--text2);border:1px solid var(--border);padding:4px 10px;border-radius:99px;text-transform:uppercase}
  .screen{display:none;width:100%;max-width:900px;padding:32px 20px 60px;position:relative;z-index:1;animation:fadeUp .4s ease}
  .screen.active{display:flex;flex-direction:column;align-items:center}
  @keyframes fadeUp{from{opacity:0;transform:translateY(16px)}to{opacity:1;transform:translateY(0)}}
  .camera-stage{position:relative;width:min(420px,100%);aspect-ratio:4/3;border-radius:var(--radius);overflow:hidden;border:1px solid var(--border);background:var(--surface);box-shadow:0 0 60px var(--glow)}
  #videoEl{width:100%;height:100%;object-fit:cover;transform:scaleX(-1);display:none}
  .camera-overlay{position:absolute;inset:0;pointer-events:none;background:linear-gradient(to bottom,transparent 55%,rgba(0,0,0,.65));display:none}
  .face-guide{position:absolute;inset:0;display:none;align-items:center;justify-content:center;pointer-events:none}
  .face-guide-oval{width:52%;height:75%;border:2px dashed rgba(124,92,252,.6);border-radius:50%;animation:pulse-border 2s ease-in-out infinite}
  @keyframes pulse-border{0%,100%{border-color:rgba(124,92,252,.5)}50%{border-color:rgba(200,92,252,.9)}}
  .camera-hint{position:absolute;bottom:12px;left:0;right:0;text-align:center;font-size:.75rem;color:rgba(255,255,255,.7);font-family:var(--mono);letter-spacing:.05em;display:none}
  #demoFaceWrap{width:100%;height:100%;display:flex;align-items:center;justify-content:center;flex-direction:column;gap:10px;background:linear-gradient(135deg,#1a1a2e,#16213e,#0f3460)}
  #demoFaceCanvas{border-radius:12px;box-shadow:0 0 40px rgba(124,92,252,.4)}
  #permissionError{display:none;flex-direction:column;align-items:center;gap:10px;padding:20px;text-align:center}
  .btn{display:inline-flex;align-items:center;justify-content:center;gap:8px;padding:12px 28px;border:none;border-radius:10px;font-family:var(--font);font-size:.95rem;font-weight:600;cursor:pointer;transition:all .18s ease;outline:none;position:relative;overflow:hidden}
  .btn::after{content:'';position:absolute;inset:0;background:rgba(255,255,255,0);transition:background .15s}
  .btn:hover::after{background:rgba(255,255,255,.06)}
  .btn:active{transform:scale(.97)}
  .btn-primary{background:linear-gradient(135deg,var(--accent),var(--accent2));color:#fff;box-shadow:0 4px 24px var(--glow)}
  .btn-primary:hover{box-shadow:0 6px 32px rgba(124,92,252,.55)}
  .btn-ghost{background:var(--surface2);color:var(--text2);border:1px solid var(--border)}
  .btn-ghost:hover{color:var(--text);border-color:var(--accent)}
  .btn-outline{background:transparent;color:var(--accent);border:1.5px solid var(--accent)}
  .btn-outline:hover{background:rgba(124,92,252,.1)}
  .btn-lg{padding:15px 36px;font-size:1.05rem;border-radius:12px}
  .btn-sm{padding:8px 18px;font-size:.82rem}
  .btn:disabled{opacity:.4;cursor:not-allowed}
  .section-label{font-family:var(--mono);font-size:.65rem;letter-spacing:.15em;text-transform:uppercase;color:var(--text2);margin-bottom:12px}
  .diff-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:12px;width:min(380px,100%)}
  .diff-card{background:var(--surface2);border:1.5px solid var(--border);border-radius:12px;padding:20px 12px;text-align:center;cursor:pointer;transition:all .2s;display:flex;flex-direction:column;gap:6px}
  .diff-card:hover{border-color:var(--accent);background:rgba(124,92,252,.07)}
  .diff-card.selected{border-color:var(--accent);background:rgba(124,92,252,.12);box-shadow:0 0 0 1px var(--accent)}
  .diff-card .grid-preview{display:grid;gap:3px;margin:0 auto 8px;width:48px;height:48px}
  .diff-card .grid-cell{background:linear-gradient(135deg,var(--accent),var(--accent2));border-radius:3px;opacity:.6}
  .diff-card.selected .grid-cell{opacity:1}
  .diff-card .diff-name{font-weight:700;font-size:1rem}
  .diff-card .diff-info{font-size:.72rem;color:var(--text2);font-family:var(--mono)}
  #previewCanvas{width:min(200px,45%);height:auto;border-radius:12px;border:2px solid var(--border);display:block}
  #gameScreen{gap:0}
  .game-layout{display:grid;grid-template-columns:1fr auto;gap:24px;width:100%;align-items:start}
  .stats-bar{display:flex;background:var(--surface);border:1px solid var(--border);border-radius:12px;overflow:hidden;margin-bottom:20px;width:100%}
  .stat-cell{flex:1;padding:12px 16px;display:flex;flex-direction:column;gap:2px;border-right:1px solid var(--border)}
  .stat-cell:last-child{border-right:none}
  .stat-label{font-family:var(--mono);font-size:.6rem;letter-spacing:.12em;text-transform:uppercase;color:var(--text2)}
  .stat-value{font-family:var(--mono);font-size:1.1rem;font-weight:700;color:var(--text)}
  #timerDisplay{color:var(--accent)}
  #progressDisplay{color:var(--correct)}
  .puzzle-wrap{position:relative}
  #puzzleBoard{display:grid;gap:4px;background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:8px;user-select:none}
  .puzzle-cell{position:relative;border-radius:6px;overflow:hidden;background:var(--surface2)}
  .puzzle-piece{width:100%;height:100%;position:absolute;inset:0;border-radius:6px;cursor:grab;overflow:hidden;transition:box-shadow .15s,transform .05s;touch-action:none}
  .puzzle-piece canvas{display:block;width:100%;height:100%}
  .puzzle-piece:hover{box-shadow:0 0 0 2px rgba(124,92,252,.5)}
  .puzzle-piece.dragging{cursor:grabbing;box-shadow:0 8px 32px rgba(0,0,0,.5),0 0 0 2.5px var(--accent);z-index:1000;transform:scale(1.07)}
  .puzzle-piece.correct{box-shadow:0 0 0 2.5px var(--correct),0 0 12px rgba(46,204,138,.3)}
  .puzzle-cell.drop-target{box-shadow:inset 0 0 0 2px var(--accent2)}
  .sidebar{width:220px;display:flex;flex-direction:column;gap:16px;flex-shrink:0}
  .sidebar-card{background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:16px}
  .sidebar-card h3{font-size:.75rem;font-family:var(--mono);letter-spacing:.1em;text-transform:uppercase;color:var(--text2);margin-bottom:12px}
  .preview-thumb{width:100%;border-radius:8px;display:block;opacity:.8}
  .action-stack{display:flex;flex-direction:column;gap:8px}
  .lb-list{display:flex;flex-direction:column;gap:8px}
  .lb-entry{display:grid;grid-template-columns:20px 1fr auto;gap:8px;align-items:center;padding:8px 10px;background:var(--surface2);border-radius:8px;border:1px solid var(--border)}
  .lb-rank{font-family:var(--mono);font-size:.65rem;color:var(--text2);text-align:center}
  .lb-rank.gold{color:#ffd166}.lb-rank.silver{color:#c0c0c0}.lb-rank.bronze{color:#cd7f32}
  .lb-info{display:flex;flex-direction:column;gap:1px}
  .lb-time{font-family:var(--mono);font-size:.82rem;font-weight:700;color:var(--text)}
  .lb-meta{font-size:.65rem;color:var(--text2)}
  .lb-diff{font-family:var(--mono);font-size:.6rem;padding:2px 7px;border-radius:99px;background:rgba(124,92,252,.15);color:var(--accent);border:1px solid rgba(124,92,252,.25)}
  .lb-empty{text-align:center;color:var(--text2);font-size:.8rem;padding:12px 0}
  #winOverlay{display:none;position:fixed;inset:0;z-index:999;background:rgba(5,5,10,.88);backdrop-filter:blur(8px);align-items:center;justify-content:center}
  #winOverlay.active{display:flex;animation:fadeIn .35s ease}
  @keyframes fadeIn{from{opacity:0}to{opacity:1}}
  .win-card{background:var(--surface);border:1px solid var(--border);border-radius:24px;padding:40px 36px;max-width:480px;width:90%;text-align:center;animation:popIn .45s cubic-bezier(.34,1.56,.64,1);box-shadow:0 0 80px rgba(124,92,252,.25);position:relative;overflow:hidden}
  .win-card::before{content:'';position:absolute;top:-60px;left:50%;transform:translateX(-50%);width:200px;height:200px;background:radial-gradient(circle,rgba(124,92,252,.3),transparent 70%);pointer-events:none}
  @keyframes popIn{from{opacity:0;transform:scale(.8)}to{opacity:1;transform:scale(1)}}
  .win-emoji{font-size:3.5rem;margin-bottom:8px;display:block}
  .win-title{font-size:1.8rem;font-weight:700;margin-bottom:4px;background:linear-gradient(135deg,var(--accent),var(--accent2));-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text}
  .win-sub{color:var(--text2);font-size:.9rem;margin-bottom:28px}
  .win-stats{display:grid;grid-template-columns:repeat(3,1fr);gap:1px;background:var(--border);border-radius:12px;overflow:hidden;margin-bottom:28px}
  .win-stat{background:var(--surface2);padding:16px 10px;display:flex;flex-direction:column;gap:4px}
  .win-stat-label{font-family:var(--mono);font-size:.6rem;letter-spacing:.12em;text-transform:uppercase;color:var(--text2)}
  .win-stat-val{font-family:var(--mono);font-size:1.2rem;font-weight:700;color:var(--text)}
  .win-actions{display:flex;gap:12px;justify-content:center;flex-wrap:wrap}
  .confetti-piece{position:fixed;border-radius:2px;pointer-events:none;z-index:9999;animation:confetti-fall linear forwards}
  @keyframes confetti-fall{0%{transform:translateY(-20px) rotate(0deg);opacity:1}100%{transform:translateY(110vh) rotate(720deg);opacity:0}}
  .mt-8{margin-top:8px}.mt-16{margin-top:16px}.mt-24{margin-top:24px}.mt-32{margin-top:32px}
  .mb-8{margin-bottom:8px}.mb-16{margin-bottom:16px}.mb-24{margin-bottom:24px}
  .text-center{text-align:center}
  .row{display:flex;gap:12px;align-items:center;justify-content:center;flex-wrap:wrap}
  .hero-label{font-family:var(--mono);font-size:.7rem;letter-spacing:.2em;text-transform:uppercase;color:var(--accent);margin-bottom:10px}
  h2{font-size:1.6rem;font-weight:700;letter-spacing:-.03em;margin-bottom:4px}
  p.sub{color:var(--text2);font-size:.9rem;line-height:1.5}
  .avatar-strip{display:flex;gap:10px;flex-wrap:wrap;justify-content:center;margin-top:16px}
  .avatar-thumb{width:72px;height:72px;border-radius:50%;border:2px solid var(--border);cursor:pointer;transition:all .2s;overflow:hidden;flex-shrink:0;position:relative}
  .avatar-thumb:hover{border-color:var(--accent);transform:scale(1.1)}
  .avatar-thumb.selected{border-color:var(--accent);box-shadow:0 0 0 3px rgba(124,92,252,.4)}
  .avatar-thumb canvas{width:100%;height:100%;display:block}
  .avatar-label{position:absolute;bottom:-20px;left:50%;transform:translateX(-50%);font-size:.6rem;color:var(--text2);white-space:nowrap;font-family:var(--mono)}
  .avatar-wrap{display:flex;flex-direction:column;align-items:center;gap:4px;padding-bottom:20px}
  .or-divider{display:flex;align-items:center;gap:12px;color:var(--text2);font-size:.8rem;margin:20px 0 0;width:min(420px,100%)}
  .or-divider::before,.or-divider::after{content:'';flex:1;height:1px;background:var(--border)}
  @media(max-width:680px){
    header{padding:14px 16px}.logo{font-size:1.05rem}.header-badge{display:none}
    .game-layout{grid-template-columns:1fr}
    .sidebar{width:100%;flex-direction:row;flex-wrap:wrap}
    .sidebar-card{flex:1;min-width:140px}
    .diff-grid{gap:8px}.diff-card{padding:14px 8px}
    .win-card{padding:28px 20px}
  }
</style>
</head>
<body>
<header>
  <div class="logo">Face<span>Puzzle</span></div>
  <div class="header-badge">Demo Mode · Slice &amp; Solve</div>
</header>

<!-- SCREEN 1: FACE CHOOSER -->
<div id="cameraScreen" class="screen active">
  <div class="hero-label mt-16">Step 1 of 3</div>
  <h2 class="text-center">Pick a face to puzzle</h2>
  <p class="sub text-center mb-8">Choose a demo avatar below, or take a photo with your webcam.</p>

  <div class="section-label mt-20 text-center">Demo Avatars — click one to select</div>
  <div class="avatar-strip" id="avatarStrip"></div>

  <div class="or-divider">or use your webcam</div>

  <div class="camera-stage mt-16" id="cameraStage">
    <div id="demoFaceWrap">
      <canvas id="demoFaceCanvas" width="320" height="320"></canvas>
      <div style="font-size:.7rem;color:rgba(255,255,255,.45);font-family:var(--mono);margin-top:6px">SELECT AN AVATAR ABOVE</div>
    </div>
    <video id="videoEl" autoplay playsinline muted></video>
    <div class="camera-overlay"></div>
    <div class="face-guide"><div class="face-guide-oval"></div></div>
    <div class="camera-hint" id="camHint">⬤ LIVE</div>
    <div id="permissionError">
      <div style="font-size:2.5rem">🚫</div>
      <p style="font-weight:600">Camera denied</p>
      <p style="color:var(--text2);font-size:.82rem">Use a demo avatar above instead.</p>
    </div>
  </div>

  <canvas id="hiddenCanvas" style="display:none"></canvas>

  <div class="row mt-24">
    <button class="btn btn-ghost" id="startCamBtn">📷 Use Webcam</button>
    <button class="btn btn-primary btn-lg" id="takePhotoBtn" disabled>✂️ Use This Face →</button>
  </div>
  <p class="sub text-center mt-16" style="font-size:.75rem">📸 Webcam photos stay on your device — nothing is uploaded.</p>
</div>

<!-- SCREEN 2: DIFFICULTY -->
<div id="diffScreen" class="screen">
  <div class="hero-label mt-8">Step 2 of 3</div>
  <h2 class="text-center">Choose difficulty</h2>
  <p class="sub text-center mb-24">More pieces = harder puzzle.</p>
  <canvas id="previewCanvas"></canvas>
  <div class="section-label mt-24 mb-8 text-center">Grid size</div>
  <div class="diff-grid">
    <div class="diff-card" data-grid="3">
      <div class="grid-preview" style="grid-template-columns:repeat(3,1fr);grid-template-rows:repeat(3,1fr)">
        <div class="grid-cell"></div><div class="grid-cell"></div><div class="grid-cell"></div>
        <div class="grid-cell"></div><div class="grid-cell"></div><div class="grid-cell"></div>
        <div class="grid-cell"></div><div class="grid-cell"></div><div class="grid-cell"></div>
      </div>
      <div class="diff-name">3 × 3</div><div class="diff-info">9 pieces · Easy</div>
    </div>
    <div class="diff-card selected" data-grid="4">
      <div class="grid-preview" style="grid-template-columns:repeat(4,1fr);grid-template-rows:repeat(4,1fr)">
        <div class="grid-cell"></div><div class="grid-cell"></div><div class="grid-cell"></div><div class="grid-cell"></div>
        <div class="grid-cell"></div><div class="grid-cell"></div><div class="grid-cell"></div><div class="grid-cell"></div>
        <div class="grid-cell"></div><div class="grid-cell"></div><div class="grid-cell"></div><div class="grid-cell"></div>
        <div class="grid-cell"></div><div class="grid-cell"></div><div class="grid-cell"></div><div class="grid-cell"></div>
      </div>
      <div class="diff-name">4 × 4</div><div class="diff-info">16 pieces · Medium</div>
    </div>
    <div class="diff-card" data-grid="5">
      <div class="grid-preview" style="grid-template-columns:repeat(5,1fr);grid-template-rows:repeat(5,1fr)">
        <div class="grid-cell"></div><div class="grid-cell"></div><div class="grid-cell"></div><div class="grid-cell"></div><div class="grid-cell"></div>
        <div class="grid-cell"></div><div class="grid-cell"></div><div class="grid-cell"></div><div class="grid-cell"></div><div class="grid-cell"></div>
        <div class="grid-cell"></div><div class="grid-cell"></div><div class="grid-cell"></div><div class="grid-cell"></div><div class="grid-cell"></div>
        <div class="grid-cell"></div><div class="grid-cell"></div><div class="grid-cell"></div><div class="grid-cell"></div><div class="grid-cell"></div>
        <div class="grid-cell"></div><div class="grid-cell"></div><div class="grid-cell"></div><div class="grid-cell"></div><div class="grid-cell"></div>
      </div>
      <div class="diff-name">5 × 5</div><div class="diff-info">25 pieces · Hard</div>
    </div>
  </div>
  <div class="row mt-32">
    <button class="btn btn-ghost" id="retakeBtn1">↩ Back</button>
    <button class="btn btn-primary btn-lg" id="startPuzzleBtn">Start Puzzle →</button>
  </div>
</div>

<!-- SCREEN 3: GAME -->
<div id="gameScreen" class="screen">
  <div class="stats-bar" style="width:100%">
    <div class="stat-cell"><div class="stat-label">Time</div><div class="stat-value" id="timerDisplay">00:00.0</div></div>
    <div class="stat-cell"><div class="stat-label">Moves</div><div class="stat-value" id="movesDisplay">0</div></div>
    <div class="stat-cell"><div class="stat-label">Correct</div><div class="stat-value" id="progressDisplay">0/0</div></div>
    <div class="stat-cell"><div class="stat-label">Grid</div><div class="stat-value" id="diffDisplay">4x4</div></div>
  </div>
  <div class="game-layout mt-8" style="width:100%">
    <div class="puzzle-wrap"><div id="puzzleBoard"></div></div>
    <div class="sidebar">
      <div class="sidebar-card">
        <h3>Reference</h3>
        <canvas id="thumbCanvas" class="preview-thumb"></canvas>
      </div>
      <div class="sidebar-card">
        <h3>Actions</h3>
        <div class="action-stack">
          <button class="btn btn-outline btn-sm" id="shuffleBtn">🔀 Shuffle</button>
          <button class="btn btn-ghost btn-sm" id="hintBtn">💡 Hint (3s)</button>
          <button class="btn btn-ghost btn-sm" id="retakeBtn2">🏠 New Face</button>
        </div>
      </div>
      <div class="sidebar-card">
        <h3>Best Times</h3>
        <div class="lb-list" id="leaderboardList"><div class="lb-empty">No records yet</div></div>
      </div>
    </div>
  </div>
</div>

<!-- WIN OVERLAY -->
<div id="winOverlay">
  <div class="win-card">
    <span class="win-emoji">🎉</span>
    <div class="win-title">Puzzle Solved!</div>
    <div class="win-sub" id="winSub">You nailed the 4x4 grid!</div>
    <div class="win-stats">
      <div class="win-stat"><div class="win-stat-label">Time</div><div class="win-stat-val" id="winTime">—</div></div>
      <div class="win-stat"><div class="win-stat-label">Moves</div><div class="win-stat-val" id="winMoves">—</div></div>
      <div class="win-stat"><div class="win-stat-label">Grid</div><div class="win-stat-val" id="winDiff">—</div></div>
    </div>
    <div class="win-actions">
      <button class="btn btn-ghost" id="winNewFaceBtn">🏠 New Face</button>
      <button class="btn btn-outline" id="winPlayAgainBtn">🔄 Play Again</button>
      <button class="btn btn-primary" id="winHarderBtn">Try Harder →</button>
    </div>
  </div>
</div>

<script>
/* ═══════════════════════════════════════════════
   CANVAS HELPERS
═══════════════════════════════════════════════ */
function mk(s){const c=document.createElement('canvas');c.width=c.height=s;return c}
function fc(x,cx,cy,r,col){x.fillStyle=col;x.beginPath();x.arc(cx,cy,r,0,Math.PI*2);x.fill()}
function fe(x,cx,cy,rx,ry,col){x.fillStyle=col;x.beginPath();x.ellipse(cx,cy,rx,ry,0,0,Math.PI*2);x.fill()}
function fr(x,lx,ly,w,h,col){x.fillStyle=col;x.fillRect(lx,ly,w,h)}
function grd(x,s,c1,c2){const g=x.createLinearGradient(0,0,0,s);g.addColorStop(0,c1);g.addColorStop(1,c2);x.fillStyle=g;x.fillRect(0,0,s,s)}
function rr(x,lx,ly,w,h,r,col){x.fillStyle=col;x.beginPath();x.moveTo(lx+r,ly);x.lineTo(lx+w-r,ly);x.quadraticCurveTo(lx+w,ly,lx+w,ly+r);x.lineTo(lx+w,ly+h-r);x.quadraticCurveTo(lx+w,ly+h,lx+w-r,ly+h);x.lineTo(lx+r,ly+h);x.quadraticCurveTo(lx,ly+h,lx,ly+h-r);x.lineTo(lx,ly+r);x.quadraticCurveTo(lx,ly,lx+r,ly);x.closePath();x.fill()}
function brow(x,cx,cy,w,col,lw){x.strokeStyle=col;x.lineWidth=lw;x.lineCap='round';x.beginPath();x.moveTo(cx-w/2,cy+2);x.quadraticCurveTo(cx,cy-4,cx+w/2,cy+2);x.stroke()}
function eye(x,cx,cy,r,iris,hi){fe(x,cx,cy,r*1.3,r,'#fff');fc(x,cx,cy,r*.7,iris);fc(x,cx,cy,r*.35,'#000');fc(x,cx-r*.25,cy-r*.25,r*.18,hi||'#fff')}
function smile(x,cx,cy,w,lip,inn){x.fillStyle=lip;x.beginPath();x.ellipse(cx,cy,w,w*.35,0,0,Math.PI);x.fill();x.fillStyle=inn;x.beginPath();x.ellipse(cx,cy+w*.06,w*.75,w*.2,0,0,Math.PI);x.fill();x.strokeStyle=lip;x.lineWidth=1.5;x.beginPath();x.moveTo(cx-w,cy);x.quadraticCurveTo(cx,cy-w*.2,cx+w,cy);x.stroke()}
function blush(x,cx,cy,col,a){x.save();x.globalAlpha=a;fe(x,cx,cy,22,13,col);x.restore()}
function nose(x,cx,cy,r,col){x.fillStyle=col;x.beginPath();x.arc(cx-r,cy,r*.4,0,Math.PI);x.arc(cx+r,cy,r*.4,0,Math.PI);x.fill();x.strokeStyle=col;x.lineWidth=1.5;x.beginPath();x.moveTo(cx-r,cy);x.quadraticCurveTo(cx,cy-r*1.5,cx+r,cy);x.stroke()}

/* ═══════════════════════════════════════════════
   6 DEMO FACE GENERATORS (each → 512×512 canvas)
═══════════════════════════════════════════════ */
const GENERATORS = [
  // 0 Warm Skin · brown skin, dark hair
  function(S){
    const c=mk(S),x=c.getContext('2d');
    grd(x,S,'#1a1a2e','#0f3460');
    fc(x,S/2,S*.33,S*.32,'#1a0a00');fr(x,S*.17,S*.17,S*.66,S*.3,'#1a0a00');
    fe(x,S*.14,S*.52,S*.07,S*.09,'#c68642');fe(x,S*.86,S*.52,S*.07,S*.09,'#c68642');
    fe(x,S/2,S*.56,S*.28,S*.34,'#c68642');
    brow(x,S*.35,S*.42,S*.12,'#3d1c00',3);brow(x,S*.65,S*.42,S*.12,'#3d1c00',3);
    eye(x,S*.35,S*.5,S*.07,'#3d1c00');eye(x,S*.65,S*.5,S*.07,'#3d1c00');
    nose(x,S/2,S*.61,S*.06,'#a0652a');
    smile(x,S/2,S*.72,S*.13,'#c0392b','#e8a598');
    blush(x,S*.28,S*.65,'#e8967a',.35);blush(x,S*.72,S*.65,'#e8967a',.35);
    return c;
  },
  // 1 Fair Skin · pale, blonde hair, blue eyes
  function(S){
    const c=mk(S),x=c.getContext('2d');
    grd(x,S,'#0f0c29','#302b63');
    fc(x,S/2,S*.31,S*.32,'#d4a017');fr(x,S*.17,S*.19,S*.66,S*.28,'#d4a017');
    fc(x,S*.16,S*.48,S*.1,'#d4a017');fc(x,S*.84,S*.48,S*.1,'#d4a017');
    fe(x,S*.16,S*.52,S*.066,S*.086,'#fdeef0');fe(x,S*.84,S*.52,S*.066,S*.086,'#fdeef0');
    fe(x,S/2,S*.57,S*.27,S*.33,'#fde8d0');
    brow(x,S*.35,S*.43,S*.11,'#b8860b',2.5);brow(x,S*.65,S*.43,S*.11,'#b8860b',2.5);
    eye(x,S*.35,S*.51,S*.068,'#1a6fb5');eye(x,S*.65,S*.51,S*.068,'#1a6fb5');
    nose(x,S/2,S*.61,S*.055,'#e0a87c');
    smile(x,S/2,S*.73,S*.12,'#c0392b','#f9b8b4');
    blush(x,S*.29,S*.66,'#f4a0a0',.3);blush(x,S*.71,S*.66,'#f4a0a0',.3);
    return c;
  },
  // 2 Deep Skin · dark skin, afro
  function(S){
    const c=mk(S),x=c.getContext('2d');
    grd(x,S,'#0d0d1a','#1a0533');
    fc(x,S/2,S*.3,S*.36,'#0a0a0a');fc(x,S*.2,S*.37,S*.19,'#111');fc(x,S*.8,S*.37,S*.19,'#111');
    fe(x,S*.15,S*.53,S*.066,S*.086,'#6b3a2a');fe(x,S*.85,S*.53,S*.066,S*.086,'#6b3a2a');
    fe(x,S/2,S*.58,S*.27,S*.33,'#8d5524');
    brow(x,S*.35,S*.44,S*.12,'#1a0800',3.5);brow(x,S*.65,S*.44,S*.12,'#1a0800',3.5);
    eye(x,S*.35,S*.52,S*.07,'#1c0a00');eye(x,S*.65,S*.52,S*.07,'#1c0a00');
    nose(x,S/2,S*.62,S*.075,'#5c3317');
    smile(x,S/2,S*.74,S*.14,'#7b1c1c','#d4856a');
    blush(x,S*.28,S*.67,'#a05030',.25);blush(x,S*.72,S*.67,'#a05030',.25);
    return c;
  },
  // 3 Anime · pink hair, big purple eyes
  function(S){
    const c=mk(S),x=c.getContext('2d');
    grd(x,S,'#1a0033','#330066');
    x.fillStyle='#cc44aa';x.beginPath();x.ellipse(S/2,S*.75,S*.4,S*.55,0,0,Math.PI*2);x.fill();
    fc(x,S/2,S*.29,S*.33,'#cc44aa');fr(x,S*.16,S*.11,S*.68,S*.33,'#cc44aa');
    x.fillStyle='#bb33aa';x.beginPath();x.moveTo(S*.19,S*.32);x.quadraticCurveTo(S*.34,S*.27,S*.37,S*.45);x.lineTo(S*.23,S*.45);x.fill();
    x.beginPath();x.moveTo(S*.81,S*.32);x.quadraticCurveTo(S*.66,S*.27,S*.63,S*.45);x.lineTo(S*.77,S*.45);x.fill();
    fe(x,S*.16,S*.5,S*.056,S*.076,'#fdeef0');fe(x,S*.84,S*.5,S*.056,S*.076,'#fdeef0');
    fe(x,S/2,S*.57,S*.24,S*.34,'#fdeef0');
    // big anime eyes
    fe(x,S*.34,S*.5,S*.09,S*.12,'#fff');fe(x,S*.66,S*.5,S*.09,S*.12,'#fff');
    fe(x,S*.34,S*.5,S*.07,S*.09,'#7700cc');fe(x,S*.66,S*.5,S*.07,S*.09,'#7700cc');
    fe(x,S*.34,S*.5,S*.04,S*.05,'#000');fe(x,S*.66,S*.5,S*.04,S*.05,'#000');
    fc(x,S*.32,S*.47,S*.018,'#fff');fc(x,S*.64,S*.47,S*.018,'#fff');
    x.strokeStyle='#000';x.lineWidth=2;
    x.beginPath();x.ellipse(S*.34,S*.5,S*.095,S*.1,0,Math.PI,Math.PI*2);x.stroke();
    x.beginPath();x.ellipse(S*.66,S*.5,S*.095,S*.1,0,Math.PI,Math.PI*2);x.stroke();
    x.beginPath();x.moveTo(S/2-S*.02,S*.63);x.lineTo(S/2,S*.648);x.lineTo(S/2+S*.02,S*.63);x.strokeStyle='rgba(180,100,120,.5)';x.lineWidth=1.5;x.stroke();
    x.beginPath();x.arc(S/2,S*.72,S*.05,.1,Math.PI-.1);x.strokeStyle='#e06080';x.lineWidth=2;x.stroke();
    blush(x,S*.3,S*.62,'#ff99bb',.4);blush(x,S*.7,S*.62,'#ff99bb',.4);
    return c;
  },
  // 4 Robot · green LED eyes, metal face
  function(S){
    const c=mk(S),x=c.getContext('2d');
    grd(x,S,'#001a00','#003300');
    fr(x,S/2-3,S*.06,6,S*.15,'#00ff88');fc(x,S/2,S*.07,S*.04,'#00ffaa');
    rr(x,S*.2,S*.22,S*.6,S*.58,20,'#1a3a1a');
    rr(x,S*.23,S*.25,S*.54,S*.52,14,'#2a5a2a');
    fc(x,S*.19,S*.4,S*.04,'#00aa55');fc(x,S*.81,S*.4,S*.04,'#00aa55');
    // LED eyes
    function led(cx,cy,r){
      x.strokeStyle='#00ff88';x.lineWidth=2;x.beginPath();x.arc(cx,cy,r,0,Math.PI*2);x.stroke();
      const g=x.createRadialGradient(cx,cy,0,cx,cy,r);g.addColorStop(0,'rgba(0,255,136,.8)');g.addColorStop(1,'rgba(0,255,136,0)');
      x.fillStyle=g;x.beginPath();x.arc(cx,cy,r,0,Math.PI*2);x.fill();
      fc(x,cx,cy,r*.3,'#001a00');
    }
    led(S*.35,S*.43,S*.08);led(S*.65,S*.43,S*.08);
    for(let i=0;i<3;i++){fr(x,S/2-S*.04,S*.6+i*S*.03,S*.08,S*.015,'#00ff88')}
    rr(x,S*.3,S*.71,S*.4,S*.06,5,'#00aa44');
    for(let i=0;i<5;i++){fr(x,S*.31+i*S*.074,S*.713,S*.04,S*.036,'#001a00')}
    return c;
  },
  // 5 Fantasy · teal skin, silver hair, glowing eyes
  function(S){
    const c=mk(S),x=c.getContext('2d');
    grd(x,S,'#000d1a','#001a33');
    // silver flowing hair
    x.fillStyle='#c0c8d0';
    x.beginPath();x.ellipse(S/2,S*.78,S*.38,S*.52,0,0,Math.PI*2);x.fill();
    fc(x,S/2,S*.28,S*.32,'#d0d8e0');fr(x,S*.15,S*.12,S*.7,S*.32,'#c8d0d8');
    fc(x,S*.14,S*.46,S*.1,'#c0c8d0');fc(x,S*.86,S*.46,S*.1,'#c0c8d0');
    // teal face
    fe(x,S*.14,S*.53,S*.065,S*.085,'#2a8080');fe(x,S*.86,S*.53,S*.065,S*.085,'#2a8080');
    fe(x,S/2,S*.57,S*.27,S*.33,'#2a9090');
    // pointed ears
    x.fillStyle='#2a8080';
    x.beginPath();x.moveTo(S*.14,S*.47);x.lineTo(S*.08,S*.38);x.lineTo(S*.19,S*.44);x.fill();
    x.beginPath();x.moveTo(S*.86,S*.47);x.lineTo(S*.92,S*.38);x.lineTo(S*.81,S*.44);x.fill();
    brow(x,S*.35,S*.42,S*.11,'#006666',2.5);brow(x,S*.65,S*.42,S*.11,'#006666',2.5);
    // glowing purple eyes
    function gEye(cx,cy,r){
      const g=x.createRadialGradient(cx,cy,0,cx,cy,r*1.4);g.addColorStop(0,'rgba(180,100,255,.5)');g.addColorStop(1,'transparent');
      x.fillStyle=g;x.beginPath();x.arc(cx,cy,r*1.4,0,Math.PI*2);x.fill();
      fe(x,cx,cy,r*1.3,r,'#e8e0ff');fe(x,cx,cy,r*.75,r*.85,'#8833cc');fc(x,cx,cy,r*.4,'#220044');fc(x,cx-r*.3,cy-r*.3,r*.2,'#fff');
    }
    gEye(S*.35,S*.5,S*.07);gEye(S*.65,S*.5,S*.07);
    nose(x,S/2,S*.61,S*.05,'#1a6666');
    // thin lips
    x.fillStyle='#006688';x.beginPath();x.ellipse(S/2,S*.72,S*.1,S*.025,0,0,Math.PI*2);x.fill();
    x.beginPath();x.arc(S/2,S*.72,S*.08,.15,Math.PI-.15);x.strokeStyle='#004455';x.lineWidth=1.5;x.stroke();
    blush(x,S*.29,S*.65,'#00aaaa',.2);blush(x,S*.71,S*.65,'#00aaaa',.2);
    return c;
  },
];

const FACE_LABELS = ['Warm Skin','Fair Skin','Deep Skin','Anime','Robot','Fantasy Elf'];

/* ═══════════════════════════════════════════════
   BUILD AVATAR STRIP
═══════════════════════════════════════════════ */
const bigCanvases = [];
let selectedAvatar = null;

GENERATORS.forEach((gen,i)=>{
  const big = gen(512);
  bigCanvases.push(big);

  const wrap = document.createElement('div');
  wrap.className = 'avatar-wrap';

  const thumb = document.createElement('div');
  thumb.className = 'avatar-thumb';
  thumb.title = FACE_LABELS[i];

  const mini = mk(72);
  mini.getContext('2d').drawImage(big,0,0,72,72);
  thumb.appendChild(mini);

  const lbl = document.createElement('div');
  lbl.style.cssText='font-size:.62rem;color:var(--text2);font-family:var(--mono);text-align:center;white-space:nowrap';
  lbl.textContent = FACE_LABELS[i];

  wrap.appendChild(thumb);
  wrap.appendChild(lbl);
  document.getElementById('avatarStrip').appendChild(wrap);

  thumb.addEventListener('click',()=>{
    document.querySelectorAll('.avatar-thumb').forEach(t=>t.classList.remove('selected'));
    thumb.classList.add('selected');
    selectedAvatar = big;
    // Show in stage
    const dw = document.getElementById('demoFaceWrap');
    const dc = document.getElementById('demoFaceCanvas');
    document.getElementById('videoEl').style.display='none';
    document.querySelector('.face-guide').style.display='none';
    document.querySelector('.camera-overlay').style.display='none';
    document.getElementById('permissionError').style.display='none';
    document.getElementById('camHint').style.display='none';
    dw.style.display='flex';
    dc.getContext('2d').drawImage(big,0,0,320,320);
    dw.querySelector('div').textContent=FACE_LABELS[i].toUpperCase()+' · SELECTED';
    document.getElementById('takePhotoBtn').disabled=false;
    cameraActive=false;
  });
});

// Auto-select first avatar on load
document.querySelector('.avatar-thumb').click();

/* ═══════════════════════════════════════════════
   STATE
═══════════════════════════════════════════════ */
const state={photoData:null,gridSize:4,pieces:[],timerInterval:null,startTime:null,elapsed:0,moves:0,solved:false,boardSize:0,pieceSize:0};
const $=id=>document.getElementById(id);
const sc={camera:$('cameraScreen'),diff:$('diffScreen'),game:$('gameScreen')};
let cameraActive=false;

function showScreen(name){Object.values(sc).forEach(s=>s.classList.remove('active'));sc[name].classList.add('active')}

/* ═══════════════════════════════════════════════
   CAMERA
═══════════════════════════════════════════════ */
$('startCamBtn').addEventListener('click',async()=>{
  try{
    const stream=await navigator.mediaDevices.getUserMedia({video:{facingMode:'user',width:{ideal:1280},height:{ideal:960}},audio:false});
    const v=$('videoEl');
    v.srcObject=stream;v.style.display='block';
    $('demoFaceWrap').style.display='none';
    document.querySelector('.face-guide').style.display='flex';
    document.querySelector('.camera-overlay').style.display='block';
    $('camHint').style.display='block';
    $('permissionError').style.display='none';
    document.querySelectorAll('.avatar-thumb').forEach(t=>t.classList.remove('selected'));
    selectedAvatar=null;cameraActive=true;
    $('takePhotoBtn').disabled=false;
    $('takePhotoBtn').textContent='📸 Take Photo';
  }catch(e){
    $('permissionError').style.display='flex';
    $('videoEl').style.display='none';
    $('camHint').style.display='none';
  }
});

$('takePhotoBtn').addEventListener('click',()=>{
  if(cameraActive&&$('videoEl').srcObject) captureCamera();
  else if(selectedAvatar) captureAvatar();
});

function captureCamera(){
  const v=$('videoEl'),hc=$('hiddenCanvas');
  const w=v.videoWidth||640,h=v.videoHeight||480,s=Math.min(w,h);
  hc.width=hc.height=s;
  const ctx=hc.getContext('2d');
  ctx.save();ctx.translate(s,0);ctx.scale(-1,1);
  ctx.drawImage(v,(w-s)/2,(h-s)/2,s,s,0,0,s,s);ctx.restore();
  state.photoData=hc.toDataURL('image/jpeg',.93);
  stopCam();goToDiff();
}

function captureAvatar(){
  const hc=$('hiddenCanvas');
  hc.width=hc.height=selectedAvatar.width;
  hc.getContext('2d').drawImage(selectedAvatar,0,0);
  state.photoData=hc.toDataURL('image/jpeg',.95);
  goToDiff();
}

function goToDiff(){
  const img=new Image();
  img.onload=()=>{
    const pc=$('previewCanvas'),tc=$('thumbCanvas');
    pc.width=img.width;pc.height=img.height;pc.getContext('2d').drawImage(img,0,0);
    tc.width=img.width;tc.height=img.height;tc.getContext('2d').drawImage(img,0,0);
  };
  img.src=state.photoData;
  showScreen('diff');
}

function stopCam(){
  const v=$('videoEl');
  if(v.srcObject){v.srcObject.getTracks().forEach(t=>t.stop());v.srcObject=null;}
  cameraActive=false;
}

/* ═══════════════════════════════════════════════
   DIFFICULTY
═══════════════════════════════════════════════ */
document.querySelectorAll('.diff-card').forEach(card=>{
  card.addEventListener('click',()=>{
    document.querySelectorAll('.diff-card').forEach(c=>c.classList.remove('selected'));
    card.classList.add('selected');
    state.gridSize=parseInt(card.dataset.grid);
  });
});
$('retakeBtn1').addEventListener('click',()=>showScreen('camera'));
$('startPuzzleBtn').addEventListener('click',initPuzzle);

/* ═══════════════════════════════════════════════
   PUZZLE
═══════════════════════════════════════════════ */
function initPuzzle(){
  if(!state.photoData)return;
  stopPuzzle();
  state.moves=0;state.solved=false;state.elapsed=0;
  const n=state.gridSize;
  const maxB=Math.min(window.innerWidth-48,560);
  const side=window.innerWidth<680?maxB:Math.min(500,maxB);
  state.boardSize=side;
  state.pieceSize=Math.floor((side-16-4*(n-1))/n);
  $('diffDisplay').textContent=n+'x'+n;
  $('progressDisplay').textContent='0/'+n*n;
  $('movesDisplay').textContent='0';
  $('timerDisplay').textContent='00:00.0';
  buildBoard(n);
  showScreen('game');
  loadLB();
}

function buildBoard(n){
  const img=new Image();
  img.onload=()=>{
    state.pieces=[];
    const ps=state.pieceSize,sw=img.width/n,sh=img.height/n;
    for(let i=0;i<n*n;i++){
      const r=Math.floor(i/n),col=i%n,pc=mk(ps);
      pc.getContext('2d').drawImage(img,col*sw,r*sh,sw,sh,0,0,ps,ps);
      state.pieces.push({correctIndex:i,currentCell:i,canvas:pc});
    }
    scramble();renderBoard();
    state.startTime=performance.now();
    state.timerInterval=setInterval(tick,100);
  };
  img.src=state.photoData;
}

function scramble(){
  const n=state.pieces.length;
  for(let k=0;k<n*8;k++){
    const a=Math.floor(Math.random()*n),b=Math.floor(Math.random()*n);
    if(a!==b){
      const pa=state.pieces.find(p=>p.currentCell===a),pb=state.pieces.find(p=>p.currentCell===b);
      if(pa)pa.currentCell=b;if(pb)pb.currentCell=a;
    }
  }
}

function renderBoard(){
  const n=state.gridSize,ps=state.pieceSize,tot=n*n;
  const board=$('puzzleBoard');
  board.style.gridTemplateColumns='repeat('+n+','+ps+'px)';
  board.style.gridTemplateRows='repeat('+n+','+ps+'px)';
  board.innerHTML='';
  const cells=[];
  for(let i=0;i<tot;i++){
    const cell=document.createElement('div');
    cell.className='puzzle-cell';cell.dataset.cell=i;
    cell.style.width=ps+'px';cell.style.height=ps+'px';
    board.appendChild(cell);cells.push(cell);
  }
  state.pieces.forEach((p,idx)=>cells[p.currentCell].appendChild(makePieceEl(p,idx)));
  updateCorrect();
}

function makePieceEl(piece,idx){
  const el=document.createElement('div');
  el.className='puzzle-piece';el.dataset.pieceIdx=idx;
  el.appendChild(piece.canvas);
  if(piece.currentCell===piece.correctIndex)el.classList.add('correct');
  attachDrag(el,idx);
  return el;
}

/* ═══════════════════════════════════════════════
   DRAG & DROP
═══════════════════════════════════════════════ */
let drag=null;

function attachDrag(el,idx){
  el.addEventListener('mousedown',e=>startDrag(e,idx,'mouse'),{passive:false});
  el.addEventListener('touchstart',e=>startDrag(e,idx,'touch'),{passive:false});
}

function getXY(e,type){
  if(type==='touch'){const t=e.touches[0]||e.changedTouches[0];return{x:t.clientX,y:t.clientY};}
  return{x:e.clientX,y:e.clientY};
}

function startDrag(e,idx,type){
  if(state.solved)return;
  e.preventDefault();
  const{x,y}=getXY(e,type);
  const el=e.currentTarget,rect=el.getBoundingClientRect(),ps=state.pieceSize;
  el.classList.add('dragging');
  el.style.cssText+='position:fixed;width:'+ps+'px;height:'+ps+'px;left:'+rect.left+'px;top:'+rect.top+'px;z-index:1000;pointer-events:none;';
  document.body.appendChild(el);
  drag={idx,el,type,ox:x-rect.left,oy:y-rect.top,from:state.pieces[idx].currentCell};
  const mv=ev=>moveDrag(ev,type),up=ev=>endDrag(ev,type,mv,up);
  if(type==='mouse'){document.addEventListener('mousemove',mv);document.addEventListener('mouseup',up);}
  else{document.addEventListener('touchmove',mv,{passive:false});document.addEventListener('touchend',up);}
}

function moveDrag(e,type){
  if(!drag)return;e.preventDefault();
  const{x,y}=getXY(e,type);
  drag.el.style.left=(x-drag.ox)+'px';drag.el.style.top=(y-drag.oy)+'px';
  document.querySelectorAll('.puzzle-cell.drop-target').forEach(c=>c.classList.remove('drop-target'));
  const t=cellUnder(x,y);if(t)t.classList.add('drop-target');
}

function endDrag(e,type,mv,up){
  if(!drag)return;
  if(e.preventDefault)e.preventDefault();
  document.querySelectorAll('.puzzle-cell.drop-target').forEach(c=>c.classList.remove('drop-target'));
  if(type==='mouse'){document.removeEventListener('mousemove',mv);document.removeEventListener('mouseup',up);}
  else{document.removeEventListener('touchmove',mv);document.removeEventListener('touchend',up);}
  const{x,y}=getXY(e,type);
  const target=cellUnder(x,y),fromCell=drag.from,idx=drag.idx,el=drag.el;
  el.style.position='';el.style.left='';el.style.top='';el.style.width='';el.style.height='';el.style.zIndex='';el.style.pointerEvents='';
  el.classList.remove('dragging');
  const board=$('puzzleBoard');
  if(target){
    const toCell=parseInt(target.dataset.cell);
    if(toCell!==fromCell){swapPieces(fromCell,toCell);state.moves++;$('movesDisplay').textContent=state.moves;}
    else board.querySelector('[data-cell="'+fromCell+'"]').appendChild(el);
  }else board.querySelector('[data-cell="'+fromCell+'"]').appendChild(el);
  drag=null;updateCorrect();checkWin();
}

function cellUnder(x,y){
  for(const cell of $('puzzleBoard').querySelectorAll('.puzzle-cell')){
    const r=cell.getBoundingClientRect();
    if(x>=r.left&&x<=r.right&&y>=r.top&&y<=r.bottom)return cell;
  }
  return null;
}

function swapPieces(from,to){
  const pa=state.pieces.find(p=>p.currentCell===from),pb=state.pieces.find(p=>p.currentCell===to);
  if(pa)pa.currentCell=to;if(pb)pb.currentCell=from;
  const board=$('puzzleBoard');
  const ca=board.querySelector('[data-cell="'+from+'"]'),cb=board.querySelector('[data-cell="'+to+'"]');
  ca.innerHTML='';cb.innerHTML='';
  if(pb)ca.appendChild(makePieceEl(pb,state.pieces.indexOf(pb)));
  if(pa)cb.appendChild(makePieceEl(pa,state.pieces.indexOf(pa)));
}

function updateCorrect(){
  let n=0;
  const board=$('puzzleBoard');
  state.pieces.forEach(p=>{
    const ce=board.querySelector('[data-cell="'+p.currentCell+'"]');
    const pe=ce&&ce.querySelector('.puzzle-piece');if(!pe)return;
    if(p.currentCell===p.correctIndex){n++;pe.classList.add('correct');}else pe.classList.remove('correct');
  });
  $('progressDisplay').textContent=n+'/'+state.pieces.length;
  return n===state.pieces.length;
}

/* ═══════════════════════════════════════════════
   WIN
═══════════════════════════════════════════════ */
function checkWin(){
  if(state.solved||!state.pieces.every(p=>p.currentCell===p.correctIndex))return;
  state.solved=true;stopTimer();saveScore();
  const n=state.gridSize;
  $('winSub').textContent='You nailed the '+n+'x'+n+' grid!';
  $('winTime').textContent=fmt(state.elapsed);
  $('winMoves').textContent=state.moves;
  $('winDiff').textContent=n+'x'+n;
  $('winOverlay').classList.add('active');
  loadLB();confetti();
}

$('winOverlay').addEventListener('click',e=>{if(e.target===$('winOverlay'))$('winOverlay').classList.remove('active')});
$('winNewFaceBtn').addEventListener('click',()=>{$('winOverlay').classList.remove('active');stopPuzzle();showScreen('camera')});
$('winPlayAgainBtn').addEventListener('click',()=>{$('winOverlay').classList.remove('active');initPuzzle()});
$('winHarderBtn').addEventListener('click',()=>{$('winOverlay').classList.remove('active');stopPuzzle();showScreen('diff')});

/* ═══════════════════════════════════════════════
   TIMER
═══════════════════════════════════════════════ */
function tick(){state.elapsed=performance.now()-state.startTime;$('timerDisplay').textContent=fmt(state.elapsed)}
function fmt(ms){const t=Math.floor(ms/100);return pad(Math.floor(t/600))+':'+pad(Math.floor(t/10)%60)+'.'+t%10}
function pad(n){return String(n).padStart(2,'0')}
function stopTimer(){clearInterval(state.timerInterval);state.timerInterval=null}
function stopPuzzle(){stopTimer();$('winOverlay').classList.remove('active');$('puzzleBoard').innerHTML=''}

/* ═══════════════════════════════════════════════
   ACTIONS
═══════════════════════════════════════════════ */
$('shuffleBtn').addEventListener('click',()=>{
  if(state.solved)return;
  scramble();renderBoard();state.moves=0;$('movesDisplay').textContent='0';state.startTime=performance.now();
});
$('hintBtn').addEventListener('click',()=>{
  if(state.solved)return;
  const ov=document.createElement('div');
  ov.style.cssText='position:absolute;inset:0;z-index:50;border-radius:12px;background:url('+state.photoData+') center/cover;border:2px solid var(--accent);';
  const w=$('puzzleBoard').parentElement;w.style.position='relative';w.appendChild(ov);setTimeout(()=>ov.remove(),3000);
});
$('retakeBtn2').addEventListener('click',()=>{stopPuzzle();stopCam();showScreen('camera')});

/* ═══════════════════════════════════════════════
   LEADERBOARD
═══════════════════════════════════════════════ */
const LK='facepuzzle_v3';
function saveScore(){
  let s=[];try{s=JSON.parse(localStorage.getItem(LK))||[];}catch{}
  s.push({time:state.elapsed,moves:state.moves,grid:state.gridSize+'x'+state.gridSize,date:new Date().toLocaleDateString()});
  s.sort((a,b)=>a.time-b.time);s=s.slice(0,5);
  try{localStorage.setItem(LK,JSON.stringify(s));}catch{}
}
function loadLB(){
  let s=[];try{s=JSON.parse(localStorage.getItem(LK))||[];}catch{}
  const list=$('leaderboardList');
  if(!s.length){list.innerHTML='<div class="lb-empty">No records yet</div>';return;}
  const rc=['gold','silver','bronze'],rg=['🥇','🥈','🥉'];
  list.innerHTML=s.map((e,i)=>'<div class="lb-entry"><div class="lb-rank '+( rc[i]||'')+'">'+(rg[i]||(i+1))+'</div><div class="lb-info"><div class="lb-time">'+fmt(e.time)+'</div><div class="lb-meta">'+e.moves+' moves · '+e.date+'</div></div><div class="lb-diff">'+e.grid+'</div></div>').join('');
}

/* ═══════════════════════════════════════════════
   CONFETTI
═══════════════════════════════════════════════ */
function confetti(){
  const cols=['#7c5cfc','#c85cfc','#2ecc8a','#fc8c5c','#ffd166','#fff'];
  for(let i=0;i<90;i++){
    const el=document.createElement('div'),sz=6+Math.random()*10;
    el.className='confetti-piece';
    el.style.cssText='left:'+Math.random()*100+'vw;top:0;width:'+sz+'px;height:'+sz+'px;background:'+cols[Math.floor(Math.random()*cols.length)]+';border-radius:'+(Math.random()>.5?'50%':'2px')+';animation-duration:'+(1.5+Math.random()*2)+'s;animation-delay:'+Math.random()*.8+'s;';
    document.body.appendChild(el);
    el.addEventListener('animationend',()=>el.remove());
  }
}

loadLB();
</script>
</body>
</html>
