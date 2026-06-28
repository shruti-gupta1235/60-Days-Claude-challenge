<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Hospital Admission Readiness Simulator</title>
<script src="https://cdn.tailwindcss.com"></script>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
:root {
  --bg: #F0F4F8;
  --surface: #FFFFFF;
  --border: #D1E3F8;
  --accent: #2563EB;
  --accent-dark: #1E40AF;
  --teal: #0D9488;
  --amber: #D97706;
  --red: #DC2626;
  --green: #16A34A;
  --text: #1E293B;
  --text-2: #475569;
  --text-3: #94A3B8;
  --mono: 'JetBrains Mono', monospace;
}
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
body { font-family: 'Inter', sans-serif; background: var(--bg); color: var(--text); min-height: 100vh; }

/* ── Header ── */
.app-bar {
  background: linear-gradient(135deg, #1E3A8A 0%, #0F766E 100%);
  padding: 14px 28px;
  display: flex; align-items: center; gap: 14px;
  box-shadow: 0 2px 16px rgba(0,0,0,0.18);
  position: sticky; top: 0; z-index: 50;
}
.app-bar h1 { font-size: 1.1rem; font-weight: 700; color: #fff; letter-spacing: -0.02em; }
.app-bar p { font-size: 0.7rem; color: rgba(255,255,255,0.65); margin-top: 2px; }
.role-badge {
  margin-left: auto;
  background: rgba(255,255,255,0.15);
  border: 1px solid rgba(255,255,255,0.25);
  border-radius: 99px;
  padding: 4px 12px;
  font-size: 0.68rem;
  font-weight: 600;
  color: rgba(255,255,255,0.9);
  letter-spacing: 0.06em;
  text-transform: uppercase;
}

/* ── Layout ── */
.main { max-width: 1100px; margin: 0 auto; padding: 28px 20px 60px; }

/* ── Cards ── */
.card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 14px;
  padding: 22px 24px;
  margin-bottom: 20px;
}
.card-title {
  font-size: 0.72rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  color: var(--text-3);
  margin-bottom: 14px;
}
.card-title span { color: var(--accent); }

/* ── Form ── */
.form-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; }
@media (max-width: 640px) { .form-grid { grid-template-columns: 1fr; } }

label { font-size: 0.78rem; font-weight: 600; color: var(--text-2); display: block; margin-bottom: 5px; }

input[type="text"], input[type="date"], select {
  width: 100%;
  padding: 9px 13px;
  border: 1.5px solid #CBD5E1;
  border-radius: 8px;
  font-size: 0.85rem;
  font-family: 'Inter', sans-serif;
  color: var(--text);
  background: #F8FAFC;
  transition: border-color 0.2s, box-shadow 0.2s;
  appearance: none;
}
input[type="text"]:focus, input[type="date"]:focus, select:focus {
  outline: none;
  border-color: var(--accent);
  box-shadow: 0 0 0 3px rgba(37,99,235,0.1);
  background: #fff;
}
select { cursor: pointer; }

.notice-box {
  background: #FFF7ED;
  border: 1px solid #FED7AA;
  border-left: 4px solid var(--amber);
  border-radius: 8px;
  padding: 10px 14px;
  font-size: 0.78rem;
  color: #7C2D12;
  line-height: 1.55;
  margin-top: 12px;
}
.notice-box.blue {
  background: #EFF6FF; border-color: #BFDBFE; border-left-color: var(--accent); color: #1E3A5F;
}
.notice-box.red {
  background: #FEF2F2; border-color: #FECACA; border-left-color: var(--red); color: #7F1D1D;
}
.notice-box.green {
  background: #F0FDF4; border-color: #BBF7D0; border-left-color: var(--green); color: #14532D;
}

/* ── Analyze button ── */
.btn-analyze {
  width: 100%;
  background: linear-gradient(135deg, #2563EB, #0D9488);
  color: #fff;
  border: none;
  border-radius: 10px;
  padding: 14px;
  font-size: 0.95rem;
  font-weight: 700;
  font-family: 'Inter', sans-serif;
  cursor: pointer;
  letter-spacing: -0.01em;
  transition: all 0.2s;
  margin-top: 6px;
}
.btn-analyze:hover { transform: translateY(-1px); box-shadow: 0 8px 24px rgba(37,99,235,0.28); }
.btn-analyze:disabled { opacity: 0.5; cursor: not-allowed; transform: none; }

/* ── Score Ring ── */
.score-ring-wrap { display: flex; align-items: center; gap: 28px; }
.ring-container { position: relative; width: 110px; height: 110px; flex-shrink: 0; }
.ring-container svg { transform: rotate(-90deg); }
.ring-center {
  position: absolute; inset: 0;
  display: flex; flex-direction: column; align-items: center; justify-content: center;
}
.ring-pct { font-family: var(--mono); font-size: 1.5rem; font-weight: 700; color: var(--text); }
.ring-label { font-size: 0.6rem; font-weight: 700; text-transform: uppercase; letter-spacing: 0.08em; color: var(--text-3); margin-top: 1px; }

/* ── Progress bar ── */
.prog-track { height: 7px; background: #E2E8F0; border-radius: 99px; overflow: hidden; }
.prog-fill { height: 100%; border-radius: 99px; transition: width 0.7s cubic-bezier(0.4,0,0.2,1); }

/* ── Status badges ── */
.badge {
  display: inline-flex; align-items: center; gap: 5px;
  font-size: 0.68rem; font-weight: 700;
  padding: 3px 9px;
  border-radius: 99px;
  text-transform: uppercase; letter-spacing: 0.06em;
}
.badge-green { background: #DCFCE7; color: #15803D; }
.badge-amber { background: #FEF9C3; color: #92400E; }
.badge-red { background: #FEE2E2; color: #B91C1C; }
.badge-blue { background: #DBEAFE; color: #1D4ED8; }
.badge-gray { background: #F1F5F9; color: #64748B; }

/* ── Status grid ── */
.status-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 12px; }
@media (max-width: 640px) { .status-grid { grid-template-columns: repeat(2, 1fr); } }

.status-item {
  background: #F8FAFC;
  border: 1px solid var(--border);
  border-radius: 10px;
  padding: 12px 14px;
}
.status-item .si-label { font-size: 0.65rem; font-weight: 700; text-transform: uppercase; letter-spacing: 0.08em; color: var(--text-3); margin-bottom: 5px; }
.status-item .si-value { font-size: 0.8rem; font-weight: 600; color: var(--text); }
.status-item .si-detail { font-size: 0.7rem; color: var(--text-2); margin-top: 3px; line-height: 1.4; }

/* ── PA Panel ── */
.pa-branch { border: 1.5px solid var(--border); border-radius: 12px; overflow: hidden; }
.pa-branch-header {
  padding: 12px 16px;
  font-weight: 700;
  font-size: 0.85rem;
  display: flex; align-items: center; gap: 10px;
}
.pa-approved-header { background: #F0FDF4; color: var(--green); border-bottom: 1px solid #BBF7D0; }
.pa-pending-header { background: #FFFBEB; color: #92400E; border-bottom: 1px solid #FDE68A; }
.pa-denied-header { background: #FEF2F2; color: var(--red); border-bottom: 1px solid #FECACA; }
.pa-branch-body { padding: 16px; display: flex; flex-direction: column; gap: 10px; }

/* ── Action buttons ── */
.action-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 10px; }
@media (max-width: 480px) { .action-grid { grid-template-columns: 1fr; } }

.action-btn {
  background: var(--surface);
  border: 1.5px solid var(--border);
  border-radius: 10px;
  padding: 12px 14px;
  text-align: left;
  cursor: pointer;
  font-family: 'Inter', sans-serif;
  font-size: 0.82rem;
  font-weight: 500;
  color: var(--text);
  transition: all 0.2s;
  display: flex; align-items: flex-start; gap: 10px;
}
.action-btn:not(.done):hover { border-color: var(--accent); background: #EFF6FF; transform: translateY(-1px); }
.action-btn.done { background: #F0FDF4; border-color: #86EFAC; color: #15803D; cursor: default; }
.action-btn .ab-icon { font-size: 1.1rem; flex-shrink: 0; }
.action-btn .ab-label { font-weight: 600; font-size: 0.82rem; }
.action-btn .ab-desc { font-size: 0.72rem; color: var(--text-3); margin-top: 2px; line-height: 1.4; }

/* ── Timeline ── */
.timeline { display: flex; gap: 0; flex-wrap: wrap; }
.tl-step {
  display: flex; flex-direction: column; align-items: center;
  flex: 1; min-width: 80px; position: relative;
}
.tl-step:not(:last-child)::after {
  content: '';
  position: absolute;
  top: 12px; left: 50%; right: -50%;
  height: 2px;
  background: #E2E8F0;
  z-index: 0;
}
.tl-step.done:not(:last-child)::after { background: var(--green); }
.tl-dot {
  width: 26px; height: 26px;
  border-radius: 50%;
  border: 2px solid #CBD5E1;
  background: #F8FAFC;
  display: flex; align-items: center; justify-content: center;
  font-size: 0.7rem;
  z-index: 1;
  position: relative;
  transition: all 0.3s;
}
.tl-step.done .tl-dot { border-color: var(--green); background: #DCFCE7; color: var(--green); }
.tl-step.active .tl-dot { border-color: var(--accent); background: #DBEAFE; color: var(--accent); animation: pulse-dot 1.5s infinite; }
.tl-name { font-size: 0.6rem; font-weight: 600; text-align: center; margin-top: 5px; color: var(--text-3); max-width: 80px; line-height: 1.3; }
.tl-step.done .tl-name { color: var(--green); }
.tl-step.active .tl-name { color: var(--accent); }

@keyframes pulse-dot {
  0%, 100% { box-shadow: 0 0 0 0 rgba(37,99,235,0.4); }
  50% { box-shadow: 0 0 0 6px rgba(37,99,235,0); }
}

/* ── Care Coordination ── */
.care-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); gap: 12px; }
.care-card {
  background: #F8FAFC;
  border: 1px solid var(--border);
  border-radius: 10px;
  padding: 14px;
}
.care-card .cc-role { font-size: 0.65rem; font-weight: 700; text-transform: uppercase; letter-spacing: 0.08em; color: var(--text-3); }
.care-card .cc-name { font-size: 0.88rem; font-weight: 700; color: var(--text); margin-top: 3px; }
.care-card .cc-detail { font-size: 0.72rem; color: var(--text-2); margin-top: 6px; line-height: 1.5; }
.care-card.ur-card { border-left: 3px solid var(--amber); }

/* ── Risk Tracker ── */
.risk-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 10px; }
.risk-item {
  background: #F8FAFC;
  border: 1px solid var(--border);
  border-radius: 10px;
  padding: 12px 14px;
}
.risk-item .ri-label { font-size: 0.68rem; font-weight: 700; text-transform: uppercase; letter-spacing: 0.08em; color: var(--text-3); display: flex; align-items: center; gap: 6px; margin-bottom: 6px; }
.risk-item .ri-bar-wrap { height: 5px; background: #E2E8F0; border-radius: 99px; overflow: hidden; }
.risk-item .ri-bar { height: 100%; border-radius: 99px; transition: width 0.6s ease; }
.risk-high .ri-bar { background: var(--red); }
.risk-med .ri-bar { background: var(--amber); }
.risk-low .ri-bar { background: var(--green); }
.risk-item .ri-level { font-size: 0.7rem; font-weight: 600; margin-top: 4px; }
.risk-high .ri-level { color: var(--red); }
.risk-med .ri-level { color: var(--amber); }
.risk-low .ri-level { color: var(--green); }

/* ── Governance ── */
.gov-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 12px; }
@media (max-width: 540px) { .gov-grid { grid-template-columns: 1fr; } }
.gov-stat {
  text-align: center;
  background: linear-gradient(135deg, #EFF6FF, #F0FDFA);
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 16px 12px;
}
.gov-stat .gs-num { font-family: var(--mono); font-size: 1.4rem; font-weight: 700; color: var(--accent); }
.gov-stat .gs-label { font-size: 0.68rem; font-weight: 600; color: var(--text-2); margin-top: 4px; line-height: 1.4; }

/* ── Decision ── */
.decision-admit {
  background: linear-gradient(135deg, #F0FDF4, #F0FDFA);
  border: 2px solid var(--green);
  border-radius: 14px;
  padding: 24px;
  text-align: center;
}
.decision-hold {
  background: linear-gradient(135deg, #FFFBEB, #FEF2F2);
  border: 2px solid var(--amber);
  border-radius: 14px;
  padding: 24px;
  text-align: center;
}
.decision-title { font-size: 1.4rem; font-weight: 800; letter-spacing: -0.02em; margin-bottom: 6px; }
.admit-title { color: var(--green); }
.hold-title { color: var(--amber); }
.decision-sub { font-size: 0.82rem; color: var(--text-2); margin-bottom: 16px; }
.summary-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(160px, 1fr)); gap: 10px; text-align: left; margin-top: 16px; }
.summary-item {
  background: rgba(255,255,255,0.7);
  border: 1px solid rgba(0,0,0,0.07);
  border-radius: 8px;
  padding: 10px 12px;
}
.summary-item .sil { font-size: 0.65rem; font-weight: 700; text-transform: uppercase; letter-spacing: 0.08em; color: var(--text-3); }
.summary-item .siv { font-size: 0.82rem; font-weight: 600; color: var(--text); margin-top: 3px; }
.missing-list { list-style: none; text-align: left; margin-top: 14px; display: flex; flex-direction: column; gap: 6px; }
.missing-list li { display: flex; align-items: flex-start; gap: 8px; font-size: 0.8rem; color: var(--text); }

/* ── Restart ── */
.btn-restart {
  display: inline-flex; align-items: center; gap: 8px;
  background: #F1F5F9; border: 1.5px solid var(--border);
  border-radius: 8px; padding: 9px 18px;
  font-size: 0.82rem; font-weight: 600; font-family: 'Inter', sans-serif;
  color: var(--text-2); cursor: pointer; transition: all 0.2s;
  margin-top: 16px;
}
.btn-restart:hover { background: #E2E8F0; color: var(--text); }

/* ── Misc ── */
.section-sep { border: none; border-top: 1px solid var(--border); margin: 4px 0 20px; }
.interqual-note {
  background: #FEF3C7; border: 1px solid #FCD34D; border-left: 4px solid #F59E0B;
  border-radius: 8px; padding: 10px 14px; font-size: 0.78rem; color: #78350F; line-height: 1.55;
}
.hidden { display: none !important; }
</style>
</head>
<body>

<header class="app-bar">
  <div style="font-size:26px">🏥</div>
  <div>
    <h1>Hospital Admission Readiness Simulator</h1>
    <p>⚠ For training purposes only — all names are illustrative training data</p>
  </div>
  <div class="role-badge">Admission Coordinator</div>
</header>

<div class="main">

  <!-- ── SETUP FORM ── -->
  <div id="setup-section">
    <div class="card">
      <div class="card-title">🗂 Patient & Admission Setup</div>
      <div class="form-grid">
        <div>
          <label>Provider / Facility</label>
          <input type="text" id="provider" placeholder="e.g. Riverside General Hospital" value="Riverside General Hospital">
        </div>
        <div>
          <label>Attending Physician</label>
          <input type="text" id="physician" placeholder="e.g. Dr. A. Sharma, MD" value="Dr. A. Sharma, MD">
        </div>
        <div>
          <label>Diagnosis</label>
          <select id="diagnosis">
            <option value="">— Select diagnosis —</option>
            <option value="Acute MI">Acute MI (Myocardial Infarction)</option>
            <option value="CHF">CHF (Congestive Heart Failure)</option>
            <option value="Pneumonia">Pneumonia</option>
            <option value="Elective Surgery">Elective Surgery</option>
            <option value="Hip Fracture">Hip Fracture</option>
          </select>
        </div>
        <div>
          <label>Admission Type</label>
          <select id="admission-type">
            <option value="">— Select type —</option>
            <option value="Inpatient">Inpatient</option>
            <option value="Observation">Observation</option>
            <option value="Emergency">Emergency</option>
            <option value="ICU">ICU</option>
            <option value="Same-Day Surgery">Same-Day Surgery</option>
          </select>
        </div>
        <div>
          <label>Prior Authorization Status</label>
          <select id="pa-status">
            <option value="">— Select PA status —</option>
            <option value="Approved">Approved</option>
            <option value="Pending">Pending</option>
            <option value="Denied">Denied</option>
          </select>
        </div>
        <div>
          <label>Admission Date</label>
          <input type="date" id="admission-date">
        </div>
      </div>

      <!-- Observation CMS notice — shown when Observation selected -->
      <div id="obs-notice" class="notice-box hidden" style="margin-top:14px">
        ⚠️ <strong>CMS 2-Midnight Rule applies</strong> — Observation status carries different cost-sharing, SNF eligibility, and billing rules than inpatient admission. Medicare patients require written MOON (Medicare Outpatient Observation Notice) notification before or within 36 hours of placement.
      </div>

      <!-- InterQual note — shown for Acute MI / CHF -->
      <div id="iq-note" class="interqual-note hidden" style="margin-top:14px">
        📋 <strong>InterQual / Milliman criteria apply</strong> — Ensure clinical documentation meets medical necessity thresholds before Utilization Review. Incomplete documentation is the leading cause of concurrent review denials for cardiac diagnoses.
      </div>

      <button class="btn-analyze" id="analyze-btn" onclick="analyze()" style="margin-top:18px">
        🏥 Analyze Admission Readiness
      </button>
    </div>
  </div>

  <!-- ── ANALYSIS SECTION ── -->
  <div id="analysis-section" class="hidden">

    <!-- Readiness Score -->
    <div class="card">
      <div class="card-title">📊 Admission Readiness Score</div>
      <div class="score-ring-wrap">
        <div class="ring-container">
          <svg width="110" height="110" viewBox="0 0 110 110">
            <circle cx="55" cy="55" r="46" fill="none" stroke="#E2E8F0" stroke-width="10"/>
            <circle id="score-ring" cx="55" cy="55" r="46" fill="none" stroke="#2563EB" stroke-width="10"
              stroke-dasharray="289" stroke-dashoffset="289" stroke-linecap="round"
              style="transition: stroke-dashoffset 0.9s cubic-bezier(0.4,0,0.2,1); transition-delay:0.2s"/>
          </svg>
          <div class="ring-center">
            <div class="ring-pct" id="score-pct">0%</div>
            <div class="ring-label">Readiness</div>
          </div>
        </div>
        <div style="flex:1">
          <div style="font-size:0.82rem; color:var(--text-2); line-height:1.6;" id="score-narrative"></div>
          <div style="margin-top:12px; display:flex; flex-direction:column; gap:8px;" id="weight-bars"></div>
        </div>
      </div>
    </div>

    <!-- Component Status -->
    <div class="card">
      <div class="card-title">🔍 Component Status</div>
      <div class="status-grid" id="status-grid"></div>
    </div>

    <!-- PA Branch -->
    <div class="card" id="pa-card">
      <div class="card-title">🔐 Prior Authorization Management</div>
      <div id="pa-branch-ui"></div>
    </div>

    <!-- InterQual note if applicable -->
    <div id="iq-analysis-note" class="card hidden" style="background:#FFFBEB; border-color:#FCD34D;">
      <div class="interqual-note" style="margin:0; border-radius:8px;">
        📋 <strong>InterQual / Milliman thresholds apply</strong> — ensure documentation meets medical necessity standards before UR review. For Acute MI / CHF, documentation must include troponin levels, EF%, hemodynamic data, and active treatment plan.
      </div>
    </div>

    <!-- Workflow Actions -->
    <div class="card">
      <div class="card-title">⚙️ Workflow Actions</div>
      <div class="action-grid" id="action-grid"></div>
    </div>

    <!-- Timeline -->
    <div class="card">
      <div class="card-title">🗓 Admission Timeline</div>
      <div class="timeline" id="timeline"></div>
    </div>

    <!-- Care Coordination -->
    <div class="card">
      <div class="card-title">👥 Care Coordination Team</div>
      <div class="care-grid" id="care-grid"></div>
    </div>

    <!-- Risk Tracking -->
    <div class="card">
      <div class="card-title">⚠ Risk Tracker</div>
      <div class="risk-grid" id="risk-grid"></div>
    </div>

    <!-- Governance Snapshot — shown at ≥75% -->
    <div class="card hidden" id="gov-card">
      <div class="card-title"><span>📐</span> Governance Snapshot</div>
      <div class="notice-box blue" style="margin-bottom:14px;">
        Industry benchmarks shown below are <strong>estimates only</strong> based on publicly available data. Use for training context, not operational benchmarking.
      </div>
      <div class="gov-grid">
        <div class="gov-stat">
          <div class="gs-num">3–5 days</div>
          <div class="gs-label">PA turnaround benchmark (industry estimate)</div>
        </div>
        <div class="gov-stat">
          <div class="gs-num">~8–10%</div>
          <div class="gs-label">Inpatient denial rate (CMS estimate)</div>
        </div>
        <div class="gov-stat">
          <div class="gs-num">~$11</div>
          <div class="gs-label">PA rework cost per transaction (CAQH estimate)</div>
        </div>
      </div>
    </div>

    <!-- Final Decision -->
    <div id="decision-card" class="hidden"></div>

    <button class="btn-restart" onclick="restart()">↺ Restart with New Patient</button>
  </div>

</div><!-- /main -->

<script>
// ─── State ────────────────────────────────────────────────────────────────────
let S = {};  // current simulation state

// ─── Setup listeners ──────────────────────────────────────────────────────────
document.getElementById('admission-type').addEventListener('change', function() {
  document.getElementById('obs-notice').classList.toggle('hidden', this.value !== 'Observation');
});
document.getElementById('diagnosis').addEventListener('change', function() {
  const cardiac = ['Acute MI','CHF'].includes(this.value);
  document.getElementById('iq-note').classList.toggle('hidden', !cardiac);
});

// Default date = today
document.getElementById('admission-date').value = new Date().toISOString().split('T')[0];

// ─── Analyze ──────────────────────────────────────────────────────────────────
function analyze() {
  const provider = document.getElementById('provider').value.trim() || 'Riverside General Hospital';
  const physician = document.getElementById('physician').value.trim() || 'Dr. A. Sharma, MD';
  const diagnosis = document.getElementById('diagnosis').value;
  const admType = document.getElementById('admission-type').value;
  const paStatus = document.getElementById('pa-status').value;
  const admDate = document.getElementById('admission-date').value;

  if (!diagnosis || !admType || !paStatus || !admDate) {
    alert('Please complete all fields before analyzing.');
    return;
  }

  // Initialize state
  S = {
    provider, physician, diagnosis, admType, paStatus, admDate,
    paResolved: paStatus === 'Approved',
    paAppealed: false,
    actionsCompleted: new Set(),
    tlDone: new Set(),
  };

  document.getElementById('setup-section').classList.add('hidden');
  document.getElementById('analysis-section').classList.remove('hidden');

  renderAll();
}

// ─── Score Calculation ────────────────────────────────────────────────────────
function calcScore() {
  const done = S.actionsCompleted;
  const icuDenied = S.admType === 'ICU' && S.paStatus === 'Denied' && !S.paResolved;

  // Weights: PA 25, Clinical Docs 20, Physician Orders 20, Insurance 15, Consent 10, Bed 10
  let scores = {
    pa: 0, docs: 0, orders: 0, insurance: 0, consent: 0, bed: 0
  };

  // PA
  if (S.paResolved || S.paStatus === 'Approved') scores.pa = 25;
  else if (S.paStatus === 'Pending') scores.pa = 8;
  else scores.pa = 0; // Denied

  // Docs
  if (done.has('upload-docs')) scores.docs = 20;
  else scores.docs = 5;

  // Physician Orders
  if (done.has('contact-physician')) scores.orders = 20;
  else scores.orders = 5;

  // Insurance
  if (done.has('verify-insurance')) scores.insurance = 15;
  else scores.insurance = 4;

  // Consent
  if (done.has('complete-consent')) scores.consent = 10;
  else scores.consent = 1;

  // Bed
  if (done.has('assign-bed')) scores.bed = 10;
  else scores.bed = 2;

  let total = Object.values(scores).reduce((a,b) => a+b, 0);

  // Cap: Denied PA + ICU cannot reach 70% from admin alone
  if (icuDenied) total = Math.min(total, 62);

  // Initial range 30–60% before actions
  const baseBonus = 30;
  const noActionsYet = done.size === 0 && !S.paResolved;
  if (noActionsYet) total = Math.max(30, Math.min(total, 60));

  return { total: Math.min(total, 100), components: scores };
}

// ─── Render Everything ────────────────────────────────────────────────────────
function renderAll() {
  renderScore();
  renderStatusGrid();
  renderPABranch();
  renderActions();
  renderTimeline();
  renderCareTeam();
  renderRisks();
  renderDecision();

  const { total } = calcScore();
  document.getElementById('gov-card').classList.toggle('hidden', total < 75);

  // InterQual note
  const cardiac = ['Acute MI','CHF'].includes(S.diagnosis);
  document.getElementById('iq-analysis-note').classList.toggle('hidden', !cardiac);
}

// ─── Score Ring ───────────────────────────────────────────────────────────────
function renderScore() {
  const { total, components } = calcScore();
  const pct = total;

  // Ring
  const circ = 2 * Math.PI * 46; // 289
  const offset = circ - (pct / 100) * circ;
  const ring = document.getElementById('score-ring');
  ring.style.strokeDashoffset = offset;
  ring.setAttribute('stroke', pct >= 90 ? '#16A34A' : pct >= 75 ? '#2563EB' : pct >= 60 ? '#D97706' : '#DC2626');
  document.getElementById('score-pct').textContent = pct + '%';

  const narrative = pct >= 90
    ? 'All critical criteria met. Patient may proceed to admission.'
    : pct >= 75
    ? 'Most criteria cleared. Complete remaining workflow actions to finalize readiness.'
    : pct >= 60
    ? 'Moderate readiness. Key gaps remain — action required before admission.'
    : 'Low readiness. Significant barriers must be resolved. Do not proceed to admission.';
  document.getElementById('score-narrative').textContent = narrative;

  // Weight bars
  const bars = document.getElementById('weight-bars');
  bars.innerHTML = '';
  const items = [
    { label: 'PA Status', weight: 25, score: components.pa, color: '#2563EB' },
    { label: 'Clinical Docs', weight: 20, score: components.docs, color: '#0D9488' },
    { label: 'Physician Orders', weight: 20, score: components.orders, color: '#7C3AED' },
    { label: 'Insurance', weight: 15, score: components.insurance, color: '#D97706' },
    { label: 'Consent', weight: 10, score: components.consent, color: '#DC2626' },
    { label: 'Bed Assignment', weight: 10, score: components.bed, color: '#64748B' },
  ];
  items.forEach(item => {
    const pctFilled = Math.round((item.score / item.weight) * 100);
    const el = document.createElement('div');
    el.innerHTML = `
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:3px;">
        <span style="font-size:0.68rem;font-weight:600;color:var(--text-2)">${item.label}</span>
        <span style="font-family:var(--mono);font-size:0.68rem;color:var(--text-3)">${item.score}/${item.weight}pts</span>
      </div>
      <div class="prog-track"><div class="prog-fill" style="width:${pctFilled}%;background:${item.color}"></div></div>
    `;
    bars.appendChild(el);
  });
}

// ─── Status Grid ──────────────────────────────────────────────────────────────
function renderStatusGrid() {
  const done = S.actionsCompleted;
  const paResolved = S.paResolved || S.paStatus === 'Approved';

  const items = [
    {
      label: 'PA Status',
      value: paResolved ? '✅ Approved' : S.paStatus === 'Pending' ? '⏳ Pending' : '❌ Denied',
      detail: paResolved ? 'Authorization on file' : S.paStatus === 'Pending' ? 'Awaiting payer review' : 'Appeal required',
      cls: paResolved ? 'badge-green' : S.paStatus === 'Pending' ? 'badge-amber' : 'badge-red'
    },
    {
      label: 'Insurance',
      value: done.has('verify-insurance') ? '✅ Verified' : '⏳ Pending',
      detail: done.has('verify-insurance') ? 'Coverage confirmed' : 'Run eligibility check',
      cls: done.has('verify-insurance') ? 'badge-green' : 'badge-amber'
    },
    {
      label: 'Bed Assignment',
      value: done.has('assign-bed') ? '✅ Assigned' : '⏳ Unassigned',
      detail: done.has('assign-bed') ? `${S.admType} bed confirmed` : 'Contact bed management',
      cls: done.has('assign-bed') ? 'badge-green' : 'badge-amber'
    },
    {
      label: 'Clinical Docs',
      value: done.has('upload-docs') ? '✅ Complete' : '⚠ Incomplete',
      detail: done.has('upload-docs') ? 'H&P, labs, orders uploaded' : 'Upload required before UR',
      cls: done.has('upload-docs') ? 'badge-green' : 'badge-red'
    },
    {
      label: 'Physician Orders',
      value: done.has('contact-physician') ? '✅ Signed' : '⚠ Pending',
      detail: done.has('contact-physician') ? 'Attending orders confirmed' : 'Physician sign-off needed',
      cls: done.has('contact-physician') ? 'badge-green' : 'badge-red'
    },
    {
      label: 'Consent',
      value: done.has('complete-consent') ? '✅ Signed' : '⏳ Not Obtained',
      detail: done.has('complete-consent') ? 'Consent form on file' : 'Required before admission',
      cls: done.has('complete-consent') ? 'badge-green' : 'badge-amber'
    },
  ];

  const grid = document.getElementById('status-grid');
  grid.innerHTML = '';
  items.forEach(item => {
    const el = document.createElement('div');
    el.className = 'status-item';
    el.innerHTML = `
      <div class="si-label">${item.label}</div>
      <div><span class="badge ${item.cls}">${item.value}</span></div>
      <div class="si-detail">${item.detail}</div>
    `;
    grid.appendChild(el);
  });
}

// ─── PA Branch ────────────────────────────────────────────────────────────────
function renderPABranch() {
  const el = document.getElementById('pa-branch-ui');
  el.innerHTML = '';

  const paResolved = S.paResolved || S.paStatus === 'Approved';

  if (paResolved) {
    el.innerHTML = `
      <div class="pa-branch">
        <div class="pa-branch-header pa-approved-header">✅ Prior Authorization — Approved</div>
        <div class="pa-branch-body">
          <div class="notice-box green">Authorization is on file with the payer (illustrative: StarCare Health). No additional PA submission required for this admission. Reference number issued. Continue with remaining workflow steps.</div>
        </div>
      </div>`;
    return;
  }

  if (S.paStatus === 'Pending') {
    el.innerHTML = `
      <div class="pa-branch">
        <div class="pa-branch-header pa-pending-header">⏳ Prior Authorization — Pending Review</div>
        <div class="pa-branch-body">
          <div class="notice-box" style="margin:0">PA submitted to payer (illustrative: StarCare Health). Awaiting clinical review. Standard turnaround: 3–5 business days.</div>
          <div class="action-grid">
            <button class="action-btn ${S.actionsCompleted.has('pa-followup')?'done':''}" onclick="paAction('pa-followup')">
              <span class="ab-icon">📞</span>
              <div><div class="ab-label">${S.actionsCompleted.has('pa-followup')?'✓ Followed Up':'Follow Up with Payer'}</div><div class="ab-desc">Call StarCare Health PA line to check status</div></div>
            </button>
            <button class="action-btn ${S.actionsCompleted.has('pa-docs')?'done':''}" onclick="paAction('pa-docs')">
              <span class="ab-icon">📂</span>
              <div><div class="ab-label">${S.actionsCompleted.has('pa-docs')?'✓ Docs Uploaded':'Upload Supporting Docs'}</div><div class="ab-desc">Submit H&P, labs, and clinical notes to payer</div></div>
            </button>
            <button class="action-btn ${S.actionsCompleted.has('pa-physician')?'done':''}" onclick="paAction('pa-physician')">
              <span class="ab-icon">👨‍⚕️</span>
              <div><div class="ab-label">${S.actionsCompleted.has('pa-physician')?'✓ Physician Contacted':'Contact Attending Physician'}</div><div class="ab-desc">Confirm ${S.physician} is available for peer-to-peer if needed</div></div>
            </button>
          </div>
          ${allPAPendingDone() ? `<button class="btn-analyze" style="margin-top:6px;padding:10px" onclick="resolvePA('Approved')">✅ Simulate PA Approval Received</button>` : ''}
        </div>
      </div>`;
    return;
  }

  if (S.paStatus === 'Denied') {
    el.innerHTML = `
      <div class="pa-branch">
        <div class="pa-branch-header pa-denied-header">❌ Prior Authorization — Denied${S.paAppealed ? ' (Appeal Filed)' : ''}</div>
        <div class="pa-branch-body">
          <div class="notice-box red" style="margin:0">PA denied by StarCare Health (illustrative). Denial reason: Insufficient medical necessity documentation. A denial is not permanent — appeal rights are available within the plan's timeframe.</div>
          <div class="action-grid">
            <button class="action-btn ${S.actionsCompleted.has('pa-review')?'done':''}" onclick="paAction('pa-review')">
              <span class="ab-icon">🔍</span>
              <div><div class="ab-label">${S.actionsCompleted.has('pa-review')?'✓ Denial Reviewed':'Review Denial Reason'}</div><div class="ab-desc">Obtain written denial rationale from payer</div></div>
            </button>
            <button class="action-btn ${S.actionsCompleted.has('pa-contact')?'done':''}" onclick="paAction('pa-contact')">
              <span class="ab-icon">📞</span>
              <div><div class="ab-label">${S.actionsCompleted.has('pa-contact')?'✓ Insurance Contacted':'Contact StarCare Health'}</div><div class="ab-desc">Request peer-to-peer review with Medical Director</div></div>
            </button>
            <button class="action-btn ${S.actionsCompleted.has('pa-appeal')?'done':''}" onclick="paAction('pa-appeal')">
              <span class="ab-icon">⚖️</span>
              <div><div class="ab-label">${S.actionsCompleted.has('pa-appeal')?'✓ Appeal Filed':'Submit Formal Appeal'}</div><div class="ab-desc">File appeal with clinical justification and LMN</div></div>
            </button>
          </div>
          ${allPADeniedDone() ? `<button class="btn-analyze" style="margin-top:6px;padding:10px;background:linear-gradient(135deg,#16A34A,#0D9488)" onclick="resolvePA('Approved')">✅ Simulate Successful Appeal — Convert to Approved</button>` : ''}
        </div>
      </div>`;
  }
}

function allPAPendingDone() {
  return ['pa-followup','pa-docs','pa-physician'].every(k => S.actionsCompleted.has(k));
}
function allPADeniedDone() {
  return ['pa-review','pa-contact','pa-appeal'].every(k => S.actionsCompleted.has(k));
}

function paAction(key) {
  S.actionsCompleted.add(key);
  renderAll();
}

function resolvePA(status) {
  S.paResolved = true;
  S.paStatus = 'Approved';
  if (status === 'Approved') S.paAppealed = true;
  renderAll();
}

// ─── Workflow Actions ─────────────────────────────────────────────────────────
const ACTIONS = [
  { key:'assign-bed', icon:'🛏', label:'Assign Bed', desc:`Confirm ${'' }bed availability in appropriate unit` },
  { key:'verify-insurance', icon:'💳', label:'Verify Insurance', desc:'Run real-time eligibility check with StarCare Health (illustrative)' },
  { key:'upload-docs', icon:'📂', label:'Upload Documentation', desc:'Submit H&P, labs, imaging, and physician notes' },
  { key:'complete-consent', icon:'✍', label:'Complete Consent', desc:'Obtain signed consent and HIPAA acknowledgment' },
  { key:'contact-physician', icon:'👨‍⚕️', label:'Contact Physician', desc:'Confirm attending orders and admission plan' },
  { key:'notify-nursing', icon:'🩺', label:'Notify Nursing', desc:'Alert unit nursing staff of incoming patient' },
  { key:'prepare-arrival', icon:'🚑', label:'Prepare Patient Arrival', desc:'Coordinate transport, registration, and room prep' },
];

function renderActions() {
  const grid = document.getElementById('action-grid');
  grid.innerHTML = '';
  ACTIONS.forEach(a => {
    const done = S.actionsCompleted.has(a.key);
    const btn = document.createElement('button');
    btn.className = `action-btn ${done ? 'done' : ''}`;
    const descText = a.key === 'assign-bed' ? `Confirm ${S.admType} bed availability in appropriate unit` : a.desc;
    btn.innerHTML = `
      <span class="ab-icon">${done ? '✅' : a.icon}</span>
      <div>
        <div class="ab-label">${done ? '✓ ' : ''}${a.label}</div>
        <div class="ab-desc">${descText}</div>
      </div>`;
    if (!done) btn.addEventListener('click', () => { S.actionsCompleted.add(a.key); renderAll(); });
    grid.appendChild(btn);
  });
}

// ─── Timeline ─────────────────────────────────────────────────────────────────
const TL_STEPS = [
  { key:'pa', label:'PA Review' },
  { key:'ins', label:'Insurance Verification' },
  { key:'bed', label:'Bed Assignment' },
  { key:'doc', label:'Documentation' },
  { key:'cons', label:'Consent' },
  { key:'arr', label:'Patient Arrival' },
  { key:'reg', label:'Registration' },
  { key:'clin', label:'Clinical Assessment' },
  { key:'adm', label:'Admission Complete' },
];

function getTLDone() {
  const d = new Set();
  const pa = S.paResolved || S.paStatus === 'Approved';
  const done = S.actionsCompleted;
  if (pa) d.add('pa');
  if (done.has('verify-insurance')) d.add('ins');
  if (done.has('assign-bed')) d.add('bed');
  if (done.has('upload-docs')) d.add('doc');
  if (done.has('complete-consent')) d.add('cons');
  if (done.has('prepare-arrival')) d.add('arr');
  if (done.has('prepare-arrival') && done.has('verify-insurance')) d.add('reg');
  if (done.has('contact-physician')) d.add('clin');
  const { total } = calcScore();
  if (total >= 90) d.add('adm');
  return d;
}

function renderTimeline() {
  const tl = document.getElementById('timeline');
  tl.innerHTML = '';
  const done = getTLDone();

  // Find first non-done = active
  let activeSet = false;
  TL_STEPS.forEach(step => {
    const div = document.createElement('div');
    const isDone = done.has(step.key);
    const isActive = !isDone && !activeSet;
    if (isActive) activeSet = true;
    div.className = `tl-step ${isDone ? 'done' : isActive ? 'active' : ''}`;
    div.innerHTML = `
      <div class="tl-dot">${isDone ? '✓' : isActive ? '→' : '·'}</div>
      <div class="tl-name">${step.label}</div>`;
    tl.appendChild(div);
  });
}

// ─── Care Team ────────────────────────────────────────────────────────────────
function renderCareTeam() {
  const grid = document.getElementById('care-grid');
  grid.innerHTML = '';

  const members = [
    { role:'Attending Physician', name: S.physician, detail:'Responsible for admission orders, clinical plan, and medical necessity documentation. Must sign all orders before patient arrival.', ur:false },
    { role:'Case Manager', name:'Sarah L. Torres, RN, CCM (illustrative)', detail:'Coordinates care transitions, insurance communication, and discharge planning from day one of admission.', ur:false },
    { role:'Nursing — Unit Lead', name:`${S.admType} Unit Charge Nurse (illustrative)`, detail:'Notified of incoming patient. Responsible for bed preparation, initial assessment, and care team communication.', ur:false },
    { role:'Utilization Review (UR)', name:'James K. Park, RN (illustrative)', detail:'Performs concurrent review using InterQual and Milliman clinical criteria. Identifies denial risk early. Monitors length of stay and medical necessity on an ongoing basis. Escalates cases at risk for payer denial.', ur:true },
    { role:'Discharge Planner', name:'Maria D. Chen, MSW (illustrative)', detail:'Begins discharge planning at admission. Coordinates SNF, home health, or rehab placement as clinically indicated. Critical for Observation patients regarding SNF eligibility.', ur:false },
  ];

  members.forEach(m => {
    const el = document.createElement('div');
    el.className = `care-card ${m.ur ? 'ur-card' : ''}`;
    el.innerHTML = `
      <div class="cc-role">${m.role}${m.ur ? ' ⭐' : ''}</div>
      <div class="cc-name">${m.name}</div>
      <div class="cc-detail">${m.detail}</div>`;
    grid.appendChild(el);
  });
}

// ─── Risk Tracker ─────────────────────────────────────────────────────────────
function renderRisks() {
  const done = S.actionsCompleted;
  const clinicHigh = ['Acute MI','CHF','Hip Fracture'].includes(S.diagnosis) || S.admType === 'ICU';

  const risks = [
    {
      label: 'Documentation Risk',
      level: done.has('upload-docs') ? 'low' : done.has('contact-physician') ? 'med' : 'high',
      bar: done.has('upload-docs') ? 18 : done.has('contact-physician') ? 50 : 85,
      detail: done.has('upload-docs') ? 'Docs complete — UR review ready' : 'Missing docs risk concurrent denial',
    },
    {
      label: 'Insurance Risk',
      level: done.has('verify-insurance') ? 'low' : 'med',
      bar: done.has('verify-insurance') ? 12 : 55,
      detail: done.has('verify-insurance') ? 'Eligibility confirmed' : 'Coverage gaps unverified',
    },
    {
      label: 'Bed Risk',
      level: done.has('assign-bed') ? 'low' : S.admType === 'ICU' ? 'high' : 'med',
      bar: done.has('assign-bed') ? 8 : S.admType === 'ICU' ? 80 : 45,
      detail: done.has('assign-bed') ? 'Bed confirmed' : S.admType === 'ICU' ? 'ICU capacity critical — escalate immediately' : 'Bed pending availability',
    },
    {
      label: 'Clinical Risk',
      level: !clinicHigh ? 'low' : done.has('contact-physician') ? 'med' : 'high',
      bar: !clinicHigh ? 15 : done.has('contact-physician') ? 38 : 82,
      detail: clinicHigh
        ? done.has('contact-physician')
          ? `${S.diagnosis} — physician engaged, monitoring active`
          : `${S.diagnosis} — elevated clinical risk, physician contact urgent`
        : 'Standard clinical profile',
    },
  ];

  const grid = document.getElementById('risk-grid');
  grid.innerHTML = '';
  risks.forEach(r => {
    const el = document.createElement('div');
    el.className = `risk-item risk-${r.level}`;
    const colors = { high: '#DC2626', med: '#D97706', low: '#16A34A' };
    el.innerHTML = `
      <div class="ri-label">
        <span>${r.level === 'high' ? '🔴' : r.level === 'med' ? '🟡' : '🟢'}</span>
        ${r.label}
      </div>
      <div class="ri-bar-wrap"><div class="ri-bar" style="width:${r.bar}%;background:${colors[r.level]}"></div></div>
      <div class="ri-level">${r.level.toUpperCase()} RISK — ${r.detail}</div>`;
    grid.appendChild(el);
  });
}

// ─── Decision ─────────────────────────────────────────────────────────────────
function renderDecision() {
  const { total } = calcScore();
  const card = document.getElementById('decision-card');
  const done = S.actionsCompleted;
  const paOk = S.paResolved || S.paStatus === 'Approved';

  card.innerHTML = '';
  card.className = '';

  if (total >= 90) {
    card.className = 'card';
    const missing = [];
    if (!done.has('notify-nursing')) missing.push({ text:'Nursing has not been notified', sev:'low' });
    if (!done.has('prepare-arrival')) missing.push({ text:'Patient arrival not prepared', sev:'low' });

    card.innerHTML = `
      <div class="decision-admit">
        <div class="decision-title admit-title">✅ Admit — Patient Ready</div>
        <div class="decision-sub">All critical admission criteria have been met. Readiness score: <strong>${total}%</strong>. Patient may proceed to ${S.admType} admission.</div>
        <div class="summary-grid">
          <div class="summary-item"><div class="sil">Patient</div><div class="siv">[Patient Name]</div></div>
          <div class="summary-item"><div class="sil">Provider</div><div class="siv">${S.provider}</div></div>
          <div class="summary-item"><div class="sil">Attending</div><div class="siv">${S.physician}</div></div>
          <div class="summary-item"><div class="sil">Diagnosis</div><div class="siv">${S.diagnosis}</div></div>
          <div class="summary-item"><div class="sil">Admission Type</div><div class="siv">${S.admType}</div></div>
          <div class="summary-item"><div class="sil">PA Status</div><div class="siv">✅ Approved</div></div>
          <div class="summary-item"><div class="sil">Admission Date</div><div class="siv">${S.admDate}</div></div>
          <div class="summary-item"><div class="sil">Insurance</div><div class="siv">Verified</div></div>
          <div class="summary-item"><div class="sil">Bed</div><div class="siv">Assigned</div></div>
        </div>
        ${missing.length > 0 ? `<div style="margin-top:12px;font-size:0.75rem;color:var(--text-2)">⚠ Recommended before patient arrives: ${missing.map(m=>m.text).join(' · ')}</div>` : ''}
        ${S.admType === 'Observation' ? `<div class="notice-box" style="margin-top:14px;text-align:left">⚠ CMS 2-Midnight Rule applies. Medicare patients must receive MOON notification. Document Observation status clearly — different billing, cost-sharing, and SNF eligibility than Inpatient.</div>` : ''}
      </div>`;
    card.classList.remove('hidden');
  } else {
    card.className = 'card';
    const missing = [];
    if (!paOk) missing.push({ icon:'🔐', text: S.paStatus === 'Denied' ? 'PA denied — appeal must be completed and approved before admission' : 'PA pending — authorization must be obtained before admission' });
    if (!done.has('upload-docs')) missing.push({ icon:'📂', text:'Clinical documentation not uploaded — required for UR review' });
    if (!done.has('contact-physician')) missing.push({ icon:'👨‍⚕️', text:'Physician orders not confirmed — attending must sign off' });
    if (!done.has('verify-insurance')) missing.push({ icon:'💳', text:'Insurance eligibility not verified' });
    if (!done.has('complete-consent')) missing.push({ icon:'✍', text:'Patient consent not obtained' });
    if (!done.has('assign-bed')) missing.push({ icon:'🛏', text:'No bed assigned — contact bed management' });

    card.innerHTML = `
      <div class="decision-hold">
        <div class="decision-title hold-title">⚠ Not Ready — Admission Blocked</div>
        <div class="decision-sub">Current readiness: <strong>${total}%</strong>. Minimum required: <strong>90%</strong>. Resolve the items below before proceeding.</div>
        <ul class="missing-list">
          ${missing.map(m => `<li><span style="font-size:1.1rem">${m.icon}</span> <span>${m.text}</span></li>`).join('')}
        </ul>
        ${S.admType === 'Observation' ? `<div class="notice-box" style="margin-top:14px;text-align:left">📋 CMS 2-Midnight Rule: if/when patient is admitted to Observation, Medicare MOON notification is required.</div>` : ''}
      </div>`;
    card.classList.remove('hidden');
  }
}

// ─── Restart ──────────────────────────────────────────────────────────────────
function restart() {
  S = {};
  document.getElementById('analysis-section').classList.add('hidden');
  document.getElementById('setup-section').classList.remove('hidden');
  document.getElementById('obs-notice').classList.add('hidden');
  document.getElementById('iq-note').classList.add('hidden');
  document.getElementById('diagnosis').value = '';
  document.getElementById('admission-type').value = '';
  document.getElementById('pa-status').value = '';
  window.scrollTo({ top: 0, behavior: 'smooth' });
}
</script>
</body>
</html>
