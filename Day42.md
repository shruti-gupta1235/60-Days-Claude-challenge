<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Personal Financial Command Center</title>
<style>
  :root{
    --ink:#1B2430;
    --ink-2:#232E3D;
    --ink-3:#2C3849;
    --paper:#F6F4EF;
    --paper-2:#FFFFFF;
    --paper-3:#ECE8DE;
    --teal:#2AC9B6;
    --teal-dim:#1D8F82;
    --gold:#F2B84B;
    --gold-dim:#C9922C;
    --coral:#F1706B;
    --coral-dim:#C24F4A;
    --slate:#7C8798;
    --line:#3A4657;
    --line-light:#DCD6C7;
    --text:#EDEFF2;
    --text-dim:#A9B2C1;
    --text-ink:#20293A;
    --text-ink-dim:#5B6472;
    --font-display:'Space Grotesk','Avenir Next','Century Gothic',system-ui,-apple-system,sans-serif;
    --font-body:'Inter','Segoe UI',system-ui,-apple-system,sans-serif;
    --font-mono:'IBM Plex Mono','SFMono-Regular',Consolas,'Liberation Mono',monospace;
    --radius:14px;
    --shadow:0 8px 30px rgba(0,0,0,0.25);
  }
  html.light{
    --bg:var(--paper); --surface:var(--paper-2); --surface-2:var(--paper-3);
    --fg:var(--text-ink); --fg-dim:var(--text-ink-dim); --border:var(--line-light);
    --shadow:0 8px 24px rgba(40,35,20,0.08);
  }
  html.dark{
    --bg:var(--ink); --surface:var(--ink-2); --surface-2:var(--ink-3);
    --fg:var(--text); --fg-dim:var(--text-dim); --border:var(--line);
  }
  *{box-sizing:border-box;}
  html{scroll-behavior:smooth;}
  body{
    margin:0; background:var(--bg); color:var(--fg);
    font-family:var(--font-body); transition:background .35s ease,color .35s ease;
    -webkit-font-smoothing:antialiased;
  }
  h1,h2,h3,.display{font-family:var(--font-display); letter-spacing:-0.01em;}
  .mono{font-family:var(--font-mono);}
  a{color:inherit;}
  button{font-family:var(--font-body); cursor:pointer;}
  ::selection{background:var(--teal); color:#0B1119;}

  /* ---------- Top bar / schedule tabs ---------- */
  .topbar{
    position:sticky; top:0; z-index:50; background:var(--bg); border-bottom:1px solid var(--border);
    backdrop-filter:blur(8px);
  }
  .topbar-inner{max-width:1240px; margin:0 auto; padding:14px 24px 0; }
  .brand-row{display:flex; align-items:center; justify-content:space-between; padding-bottom:12px;}
  .brand{display:flex; align-items:baseline; gap:10px;}
  .brand .mark{
    width:34px; height:34px; border-radius:9px; background:linear-gradient(135deg,var(--teal),var(--gold));
    display:flex; align-items:center; justify-content:center; font-family:var(--font-display); font-weight:700; color:#0B1119; font-size:15px;
  }
  .brand h1{font-size:19px; margin:0; font-weight:700;}
  .brand .sub{font-size:11px; color:var(--fg-dim); text-transform:uppercase; letter-spacing:.14em; margin-top:1px;}
  .top-actions{display:flex; gap:8px; align-items:center;}
  .icon-btn{
    background:var(--surface); border:1px solid var(--border); color:var(--fg); border-radius:10px;
    width:38px; height:38px; display:flex; align-items:center; justify-content:center; font-size:16px;
    transition:transform .15s ease, border-color .15s ease;
  }
  .icon-btn:hover{transform:translateY(-1px); border-color:var(--teal);}
  .schedule-tabs{display:flex; gap:4px; overflow-x:auto; padding-bottom:0; scrollbar-width:thin;}
  .schedule-tabs::-webkit-scrollbar{height:4px;}
  .schedule-tabs::-webkit-scrollbar-thumb{background:var(--border); border-radius:4px;}
  .tab{
    flex:0 0 auto; padding:10px 16px; font-size:12.5px; font-weight:600; color:var(--fg-dim);
    border-bottom:2px solid transparent; white-space:nowrap; letter-spacing:.02em;
    transition:color .15s ease, border-color .15s ease; text-decoration:none;
  }
  .tab:hover{color:var(--fg);}
  .tab.active{color:var(--teal); border-color:var(--teal);}

  .wrap{max-width:1240px; margin:0 auto; padding:32px 24px 80px;}
  section{scroll-margin-top:96px; margin-bottom:56px;}
  .section-head{display:flex; align-items:baseline; justify-content:space-between; gap:16px; margin-bottom:18px; flex-wrap:wrap;}
  .section-head .eyebrow{font-size:11px; text-transform:uppercase; letter-spacing:.16em; color:var(--teal); font-weight:700; display:block; margin-bottom:4px;}
  .section-head h2{font-size:26px; margin:0; font-weight:700;}
  .section-head p{color:var(--fg-dim); font-size:13.5px; margin:6px 0 0; max-width:520px;}

  .grid{display:grid; gap:16px;}
  .grid-2{grid-template-columns:repeat(2,1fr);}
  .grid-3{grid-template-columns:repeat(3,1fr);}
  .grid-4{grid-template-columns:repeat(4,1fr);}
  @media(max-width:900px){.grid-2,.grid-3,.grid-4{grid-template-columns:1fr;}}

  .card{
    background:var(--surface); border:1px solid var(--border); border-radius:var(--radius);
    padding:20px; box-shadow:var(--shadow);
  }
  .card h3{font-size:14px; margin:0 0 12px; font-weight:700;}
  .stat-card .num{font-family:var(--font-mono); font-size:26px; font-weight:600; margin:2px 0;}
  .stat-card .label{font-size:11.5px; color:var(--fg-dim); text-transform:uppercase; letter-spacing:.08em;}
  .stat-card .delta{font-size:11.5px; margin-top:6px; font-weight:600;}
  .up{color:var(--teal);} .down{color:var(--coral);} .flat{color:var(--gold);}

  /* ---------- Hero: Transcript ---------- */
  .transcript{
    display:grid; grid-template-columns:280px 1fr; gap:0; border:1px solid var(--border); border-radius:18px;
    overflow:hidden; background:var(--surface); box-shadow:var(--shadow);
  }
  @media(max-width:820px){.transcript{grid-template-columns:1fr;}}
  .transcript-gpa{
    background:linear-gradient(160deg,var(--ink-2),var(--ink-3));
    color:#EDEFF2; padding:30px 26px; display:flex; flex-direction:column; justify-content:space-between; position:relative;
  }
  .transcript-gpa .eyebrow{font-size:10.5px; letter-spacing:.18em; text-transform:uppercase; color:var(--gold); font-weight:700;}
  .gpa-num{font-family:var(--font-display); font-size:64px; font-weight:700; line-height:1; margin:14px 0 2px;}
  .gpa-letter{
    display:inline-flex; align-items:center; justify-content:center; width:46px; height:46px; border-radius:50%;
    border:2px solid var(--teal); color:var(--teal); font-family:var(--font-display); font-weight:700; font-size:19px; margin-top:10px;
  }
  .gpa-note{font-size:12px; color:#A9B2C1; margin-top:14px; line-height:1.5;}
  .transcript-rows{padding:22px 26px;}
  .transcript-rows h3{font-size:12px; text-transform:uppercase; letter-spacing:.1em; color:var(--fg-dim); margin:0 0 14px;}
  .trow{display:grid; grid-template-columns:1fr 90px 56px; align-items:center; gap:12px; padding:10px 0; border-bottom:1px solid var(--border);}
  .trow:last-child{border-bottom:none;}
  .trow .name{font-size:13.5px; font-weight:600;}
  .trow .desc{font-size:11.5px; color:var(--fg-dim); margin-top:1px;}
  .bar-track{height:6px; background:var(--surface-2); border-radius:4px; overflow:hidden;}
  .bar-fill{height:100%; border-radius:4px;}
  .grade-badge{
    justify-self:end; font-family:var(--font-mono); font-size:12.5px; font-weight:700; padding:3px 9px; border-radius:6px;
    text-align:center;
  }

  /* ---------- Forms ---------- */
  .form-row{display:flex; gap:10px; flex-wrap:wrap; margin-bottom:14px;}
  .field{flex:1; min-width:120px; display:flex; flex-direction:column; gap:5px;}
  .field label{font-size:11px; color:var(--fg-dim); text-transform:uppercase; letter-spacing:.06em;}
  input,select{
    background:var(--surface-2); border:1px solid var(--border); border-radius:9px; padding:9px 11px;
    color:var(--fg); font-size:13.5px; font-family:var(--font-body); width:100%;
  }
  input:focus,select:focus,button:focus,.tab:focus,.icon-btn:focus{outline:2px solid var(--teal); outline-offset:2px;}
  .btn{
    background:var(--teal); color:#0B1119; border:none; border-radius:9px; padding:10px 18px;
    font-weight:700; font-size:13px; transition:transform .12s ease, opacity .12s ease;
  }
  .btn:hover{transform:translateY(-1px);}
  .btn.secondary{background:transparent; color:var(--fg); border:1px solid var(--border);}
  .btn.gold{background:var(--gold); color:#241A05;}
  .btn.small{padding:6px 12px; font-size:12px;}

  .list{display:flex; flex-direction:column; gap:8px; max-height:280px; overflow-y:auto; margin-top:6px; padding-right:4px;}
  .list::-webkit-scrollbar{width:5px;} .list::-webkit-scrollbar-thumb{background:var(--border); border-radius:4px;}
  .list-row{
    display:flex; justify-content:space-between; align-items:center; background:var(--surface-2);
    border-radius:9px; padding:9px 12px; font-size:13px;
  }
  .list-row .meta{color:var(--fg-dim); font-size:11px;}
  .list-row .amt{font-family:var(--font-mono); font-weight:600;}
  .list-row .del{background:none; border:none; color:var(--coral); font-size:14px; margin-left:10px;}

  /* ---------- Budget bars ---------- */
  .budget-row{margin-bottom:14px;}
  .budget-row .top{display:flex; justify-content:space-between; font-size:12.5px; margin-bottom:5px;}
  .budget-row .top .cat{font-weight:600;}
  .budget-row .top .nums{font-family:var(--font-mono); color:var(--fg-dim);}
  .budget-track{height:9px; background:var(--surface-2); border-radius:6px; overflow:hidden;}
  .budget-fill{height:100%; border-radius:6px; transition:width .4s ease;}

  /* ---------- Goal ring ---------- */
  .goal-wrap{display:flex; gap:26px; align-items:center; flex-wrap:wrap;}
  .goal-info{flex:1; min-width:200px;}
  .goal-info .amt-big{font-family:var(--font-mono); font-size:30px; font-weight:600;}
  .goal-info .target{color:var(--fg-dim); font-size:13px; margin-top:2px;}
  .goal-info .eta{margin-top:12px; font-size:12.5px; padding:8px 12px; background:var(--surface-2); border-radius:9px; display:inline-block;}

  /* ---------- Charts (SVG) ---------- */
  .chart-card svg{width:100%; height:auto; display:block;}
  .legend{display:flex; flex-wrap:wrap; gap:10px; margin-top:12px;}
  .legend .item{display:flex; align-items:center; gap:6px; font-size:11.5px; color:var(--fg-dim);}
  .legend .swatch{width:9px; height:9px; border-radius:3px;}

  /* ---------- What-if sliders ---------- */
  .slider-row{margin-bottom:20px;}
  .slider-row .top{display:flex; justify-content:space-between; font-size:13px; margin-bottom:8px;}
  .slider-row .top .val{font-family:var(--font-mono); color:var(--teal); font-weight:700;}
  input[type=range]{
    -webkit-appearance:none; width:100%; height:5px; border-radius:4px; background:var(--surface-2); outline:none;
  }
  input[type=range]::-webkit-slider-thumb{
    -webkit-appearance:none; width:18px; height:18px; border-radius:50%; background:var(--teal); cursor:pointer;
    border:3px solid var(--bg); box-shadow:0 0 0 1px var(--teal);
  }
  .whatif-result{
    background:var(--surface-2); border-radius:12px; padding:16px; margin-top:6px;
    display:grid; grid-template-columns:repeat(3,1fr); gap:14px;
  }
  @media(max-width:700px){.whatif-result{grid-template-columns:1fr;}}
  .whatif-result .k{font-size:11px; color:var(--fg-dim); text-transform:uppercase; letter-spacing:.06em;}
  .whatif-result .v{font-family:var(--font-mono); font-size:19px; font-weight:700; margin-top:3px;}

  /* ---------- Insights ---------- */
  .insight{
    display:flex; gap:12px; padding:14px 16px; border-radius:12px; background:var(--surface-2); margin-bottom:10px;
    border-left:3px solid var(--teal);
  }
  .insight.warn{border-left-color:var(--coral);}
  .insight.tip{border-left-color:var(--gold);}
  .insight .txt{font-size:13px; line-height:1.5;}
  .insight .txt b{color:var(--fg);}

  /* ---------- Checklist ---------- */
  .checklist-item{display:flex; align-items:center; gap:10px; padding:10px 4px; border-bottom:1px solid var(--border); font-size:13.5px;}
  .checklist-item:last-child{border-bottom:none;}
  .checklist-item input{width:auto;}
  .checklist-item.done label{text-decoration:line-through; color:var(--fg-dim);}

  /* ---------- Prompt chips ---------- */
  .chip-grid{display:flex; flex-wrap:wrap; gap:8px;}
  .chip{
    background:var(--surface-2); border:1px solid var(--border); border-radius:20px; padding:8px 14px; font-size:12px;
    cursor:pointer; transition:border-color .15s ease;
  }
  .chip:hover{border-color:var(--teal);}

  footer{text-align:center; padding:30px 20px; color:var(--fg-dim); font-size:12px; border-top:1px solid var(--border);}

  @media print{
    .topbar,.icon-btn,.btn,.no-print{display:none !important;}
    body{background:#fff; color:#111;}
    section{margin-bottom:24px;}
    .card{box-shadow:none; border:1px solid #ccc;}
  }
</style>
</head>
<body>

<div class="topbar">
  <div class="topbar-inner">
    <div class="brand-row">
      <div class="brand">
        <div class="mark">$</div>
        <div>
          <h1>Personal Financial Command Center</h1>
          <div class="sub">Student &middot; Part-Time Income &middot; Savings Track</div>
        </div>
      </div>
      <div class="top-actions">
        <button class="icon-btn" id="printBtn" title="Print report">&#128424;</button>
        <button class="icon-btn" id="themeBtn" title="Toggle theme">&#9789;</button>
      </div>
    </div>
    <nav class="schedule-tabs" id="tabNav">
      <a class="tab active" href="#overview">Overview</a>
      <a class="tab" href="#income">Income</a>
      <a class="tab" href="#expenses">Expenses</a>
      <a class="tab" href="#budget">Budget</a>
      <a class="tab" href="#goal">Savings Goal</a>
      <a class="tab" href="#subscriptions">Subscriptions</a>
      <a class="tab" href="#cashflow">Cash Flow</a>
      <a class="tab" href="#whatif">What-If</a>
      <a class="tab" href="#insights">Insights</a>
      <a class="tab" href="#calculators">Calculators</a>
      <a class="tab" href="#literacy">Literacy</a>
    </nav>
  </div>
</div>

<div class="wrap">

  <!-- OVERVIEW -->
  <section id="overview">
    <div class="section-head">
      <div>
        <span class="eyebrow">Report Card</span>
        <h2>Your Money GPA</h2>
        <p>A transcript-style read on how your money is doing this term &mdash; savings rate, budget discipline, spending control, and goal progress, graded like a course.</p>
      </div>
    </div>

    <div class="transcript">
      <div class="transcript-gpa">
        <div>
          <span class="eyebrow">Overall</span>
          <div class="gpa-num" id="gpaNum">0.0</div>
          <div style="font-size:12px;color:#A9B2C1;">out of 4.0</div>
          <div class="gpa-letter" id="gpaLetter">-</div>
        </div>
        <div class="gpa-note" id="gpaNote">Log a few income and expense entries to see your grade update in real time.</div>
      </div>
      <div class="transcript-rows" id="transcriptRows">
        <h3>Course Breakdown</h3>
        <!-- rows injected by JS -->
      </div>
    </div>

    <div class="grid grid-4" style="margin-top:16px;">
      <div class="card stat-card">
        <div class="label">Monthly Income</div>
        <div class="num mono" id="statIncome">$0</div>
        <div class="delta flat" id="statIncomeSrc">&mdash;</div>
      </div>
      <div class="card stat-card">
        <div class="label">Monthly Spending</div>
        <div class="num mono" id="statExpenses">$0</div>
        <div class="delta" id="statExpenseDelta">&mdash;</div>
      </div>
      <div class="card stat-card">
        <div class="label">Net Cash Flow</div>
        <div class="num mono" id="statNet">$0</div>
        <div class="delta" id="statNetNote">&mdash;</div>
      </div>
      <div class="card stat-card">
        <div class="label">Goal Progress</div>
        <div class="num mono" id="statGoal">0%</div>
        <div class="delta flat" id="statGoalNote">&mdash;</div>
      </div>
    </div>
  </section>

  <!-- INCOME -->
  <section id="income">
    <div class="section-head">
      <div>
        <span class="eyebrow">Cash In</span>
        <h2>Income</h2>
        <p>Track your part-time job, internship stipend, or any other income streams.</p>
      </div>
    </div>
    <div class="grid grid-2">
      <div class="card">
        <h3>Add income</h3>
        <div class="form-row">
          <div class="field"><label>Source</label><input id="incSource" placeholder="e.g. Campus Job"></div>
          <div class="field"><label>Amount ($)</label><input id="incAmount" type="number" min="0" step="0.01" placeholder="180"></div>
        </div>
        <div class="form-row">
          <div class="field"><label>Frequency</label>
            <select id="incFreq">
              <option value="weekly">Weekly</option>
              <option value="biweekly">Bi-weekly</option>
              <option value="monthly" selected>Monthly</option>
              <option value="onetime">One-time</option>
            </select>
          </div>
          <div class="field"><label>Date</label><input id="incDate" type="date"></div>
        </div>
        <button class="btn" id="addIncomeBtn">Add income</button>
      </div>
      <div class="card">
        <h3>Income sources <span class="mono" id="incTotal" style="float:right;color:var(--teal);"></span></h3>
        <div class="list" id="incomeList"></div>
      </div>
    </div>
  </section>

  <!-- EXPENSES -->
  <section id="expenses">
    <div class="section-head">
      <div>
        <span class="eyebrow">Cash Out</span>
        <h2>Expenses</h2>
        <p>Log spending by category to see exactly where your money goes each month.</p>
      </div>
    </div>
    <div class="grid grid-2">
      <div class="card">
        <h3>Add expense</h3>
        <div class="form-row">
          <div class="field"><label>Category</label>
            <select id="expCategory">
              <option>Food</option>
              <option>Textbooks &amp; Supplies</option>
              <option>Transport</option>
              <option>Entertainment</option>
              <option>Subscriptions</option>
              <option>Housing</option>
              <option>Other</option>
            </select>
          </div>
          <div class="field"><label>Amount ($)</label><input id="expAmount" type="number" min="0" step="0.01" placeholder="12.50"></div>
        </div>
        <div class="form-row">
          <div class="field"><label>Note</label><input id="expNote" placeholder="e.g. Groceries"></div>
          <div class="field"><label>Date</label><input id="expDate" type="date"></div>
        </div>
        <button class="btn" id="addExpenseBtn">Add expense</button>
      </div>
      <div class="card chart-card">
        <h3>Spending by category</h3>
        <div id="donutHolder"></div>
        <div class="legend" id="donutLegend"></div>
      </div>
    </div>
    <div class="card" style="margin-top:16px;">
      <h3>Recent expenses <span class="mono" id="expTotal" style="float:right;color:var(--coral);"></span></h3>
      <div class="list" id="expenseList"></div>
    </div>
  </section>

  <!-- BUDGET -->
  <section id="budget">
    <div class="section-head">
      <div>
        <span class="eyebrow">Envelopes</span>
        <h2>Budget</h2>
        <p>Set a monthly ceiling per category and watch it fill as you spend.</p>
      </div>
    </div>
    <div class="card" id="budgetCard">
      <h3>Category limits</h3>
      <div id="budgetRows"></div>
    </div>
  </section>

  <!-- GOAL -->
  <section id="goal">
    <div class="section-head">
      <div>
        <span class="eyebrow">Big Goal</span>
        <h2>Savings Goal</h2>
        <p>Whatever you're saving for &mdash; laptop, trip, car &mdash; track it here and see your projected finish line.</p>
      </div>
    </div>
    <div class="card">
      <div class="form-row">
        <div class="field"><label>Goal name</label><input id="goalName" placeholder="New Laptop"></div>
        <div class="field"><label>Target amount ($)</label><input id="goalTarget" type="number" min="0" step="1"></div>
        <div class="field"><label>Target date</label><input id="goalDate" type="date"></div>
      </div>
      <button class="btn secondary small" id="saveGoalBtn">Update goal</button>
      <hr style="border:none;border-top:1px solid var(--border);margin:20px 0;">
      <div class="goal-wrap">
        <div id="goalRingHolder"></div>
        <div class="goal-info">
          <div class="target" id="goalTitle">New Laptop</div>
          <div class="amt-big" id="goalAmt">$0 <span style="font-size:14px;color:var(--fg-dim);font-weight:400;">saved</span></div>
          <div class="target" id="goalTargetLabel">of $0 target</div>
          <div class="eta" id="goalEta">Add income &amp; expenses to project your finish date.</div>
          <div style="margin-top:14px;display:flex;gap:8px;">
            <input id="goalContribAmt" type="number" placeholder="Add $" style="width:110px;">
            <button class="btn gold small" id="addContribBtn">Add to goal</button>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- SUBSCRIPTIONS -->
  <section id="subscriptions">
    <div class="section-head">
      <div>
        <span class="eyebrow">Recurring</span>
        <h2>Subscriptions</h2>
        <p>Streaming, gym, cloud storage &mdash; the small charges that add up fast.</p>
      </div>
    </div>
    <div class="grid grid-2">
      <div class="card">
        <h3>Add subscription</h3>
        <div class="form-row">
          <div class="field"><label>Name</label><input id="subName" placeholder="Spotify"></div>
          <div class="field"><label>Amount ($)</label><input id="subAmount" type="number" min="0" step="0.01" placeholder="5.99"></div>
        </div>
        <div class="field" style="margin-bottom:12px;"><label>Billing cycle</label>
          <select id="subCycle"><option value="monthly">Monthly</option><option value="yearly">Yearly</option></select>
        </div>
        <button class="btn" id="addSubBtn">Add subscription</button>
      </div>
      <div class="card">
        <h3>Active subscriptions <span class="mono" id="subTotal" style="float:right;color:var(--gold);"></span></h3>
        <div class="list" id="subList"></div>
      </div>
    </div>
  </section>

  <!-- CASH FLOW -->
  <section id="cashflow">
    <div class="section-head">
      <div>
        <span class="eyebrow">Trend</span>
        <h2>Cash Flow</h2>
        <p>Income versus expenses over recent months, built from what you've logged.</p>
      </div>
    </div>
    <div class="card chart-card">
      <div id="cashflowHolder"></div>
      <div class="legend">
        <div class="item"><span class="swatch" style="background:var(--teal);"></span>Income</div>
        <div class="item"><span class="swatch" style="background:var(--coral);"></span>Expenses</div>
      </div>
    </div>
  </section>

  <!-- WHAT-IF -->
  <section id="whatif">
    <div class="section-head">
      <div>
        <span class="eyebrow">Simulator</span>
        <h2>What-If Scenarios</h2>
        <p>Drag the levers to see how small changes shift your goal timeline and Money GPA.</p>
      </div>
    </div>
    <div class="grid grid-2">
      <div class="card">
        <div class="slider-row">
          <div class="top"><span>Extra saved per week</span><span class="val" id="whatifSaveVal">$0</span></div>
          <input type="range" id="whatifSave" min="0" max="100" step="5" value="0">
        </div>
        <div class="slider-row">
          <div class="top"><span>Cut subscriptions by</span><span class="val" id="whatifSubVal">$0</span></div>
          <input type="range" id="whatifSub" min="0" max="60" step="1" value="0">
        </div>
        <div class="slider-row">
          <div class="top"><span>Extra work hours / week</span><span class="val" id="whatifHoursVal">0 hrs</span></div>
          <input type="range" id="whatifHours" min="0" max="15" step="1" value="0">
        </div>
        <div class="field"><label>Hourly wage ($)</label><input id="whatifWage" type="number" value="14" style="max-width:120px;"></div>
      </div>
      <div class="card">
        <h3>Projected impact</h3>
        <div class="whatif-result">
          <div><div class="k">New Monthly Savings</div><div class="v" id="wifSavings">$0</div></div>
          <div><div class="k">Goal Finish Date</div><div class="v" id="wifDate">&mdash;</div></div>
          <div><div class="k">Projected GPA</div><div class="v" id="wifGpa">0.0</div></div>
        </div>
        <p style="font-size:12px;color:var(--fg-dim);margin-top:14px;">This is a simulation only &mdash; nothing here changes your saved data until you act on it.</p>
      </div>
    </div>
  </section>

  <!-- INSIGHTS -->
  <section id="insights">
    <div class="section-head">
      <div>
        <span class="eyebrow">Auto-Generated</span>
        <h2>Financial Insights</h2>
        <p>Rule-based observations from your own numbers &mdash; refreshed every time your data changes.</p>
      </div>
    </div>
    <div class="card" id="insightsCard"></div>
  </section>

  <!-- CALCULATORS -->
  <section id="calculators">
    <div class="section-head">
      <div>
        <span class="eyebrow">Tools</span>
        <h2>Calculators</h2>
        <p>Quick math for planning ahead.</p>
      </div>
    </div>
    <div class="grid grid-2">
      <div class="card">
        <h3>Savings goal calculator</h3>
        <div class="form-row">
          <div class="field"><label>Target ($)</label><input id="calcTarget" type="number" value="1200"></div>
          <div class="field"><label>Already saved ($)</label><input id="calcSaved" type="number" value="350"></div>
        </div>
        <div class="field" style="margin-bottom:12px;"><label>Monthly contribution ($)</label><input id="calcMonthly" type="number" value="120"></div>
        <button class="btn secondary small" id="calcGoalBtn">Calculate</button>
        <div class="whatif-result" style="grid-template-columns:1fr 1fr;margin-top:14px;" id="calcGoalResult"></div>
      </div>
      <div class="card">
        <h3>Part-time paycheck planner</h3>
        <div class="form-row">
          <div class="field"><label>Hours / week</label><input id="calcHours" type="number" value="12"></div>
          <div class="field"><label>Hourly wage ($)</label><input id="calcRate" type="number" value="14"></div>
        </div>
        <div class="field" style="margin-bottom:12px;"><label>Estimated tax withholding (%)</label><input id="calcTax" type="number" value="12"></div>
        <button class="btn secondary small" id="calcPayBtn">Calculate</button>
        <div class="whatif-result" style="grid-template-columns:1fr 1fr;margin-top:14px;" id="calcPayResult"></div>
      </div>
    </div>
  </section>

  <!-- LITERACY: tips, checklist, resources, prompts -->
  <section id="literacy">
    <div class="section-head">
      <div>
        <span class="eyebrow">Study Guide</span>
        <h2>Financial Literacy</h2>
        <p>Tips, a planning checklist, and prompts to keep leveling up your money skills.</p>
      </div>
    </div>
    <div class="grid grid-3">
      <div class="card">
        <h3>Tips for students</h3>
        <div class="list" style="max-height:none;">
          <div class="insight tip"><div class="txt">Automate a fixed transfer to savings the day your paycheck lands, before you can spend it.</div></div>
          <div class="insight tip"><div class="txt">Audit subscriptions every semester &mdash; cancel anything unused in the last 30 days.</div></div>
          <div class="insight tip"><div class="txt">Use student discounts on software, transit, and streaming before paying full price.</div></div>
          <div class="insight tip"><div class="txt">Keep a small buffer (even $100&ndash;200) so one surprise expense doesn't wreck your budget.</div></div>
        </div>
      </div>
      <div class="card">
        <h3>Planning checklist</h3>
        <div id="checklistHolder"></div>
      </div>
      <div class="card">
        <h3>Resources to explore</h3>
        <div class="list" style="max-height:none;">
          <div class="list-row"><span>Campus financial aid / student services office</span></div>
          <div class="list-row"><span>Free budgeting worksheets from your bank's student account</span></div>
          <div class="list-row"><span>Employer HR page for pay stubs &amp; tax withholding forms</span></div>
          <div class="list-row"><span>A trusted personal finance book from your library</span></div>
        </div>
      </div>
    </div>
    <div class="card" style="margin-top:16px;">
      <h3>Prompts to ask Claude next</h3>
      <div class="chip-grid" id="promptChips"></div>
    </div>
  </section>

</div>

<footer>Personal Financial Command Center &middot; All data stays in your browser's local storage &middot; Built for planning, not professional financial advice.</footer>

<script>
(function(){
  "use strict";

  /* ---------------- Storage helpers ---------------- */
  const LS = {
    get(k, fallback){ try{ const v = localStorage.getItem(k); return v ? JSON.parse(v) : fallback; }catch(e){ return fallback; } },
    set(k, v){ try{ localStorage.setItem(k, JSON.stringify(v)); }catch(e){} }
  };

  const todayISO = ()=> new Date().toISOString().slice(0,10);
  const fmt = (n)=> '$' + (Math.round((n||0)*100)/100).toLocaleString(undefined,{minimumFractionDigits:0, maximumFractionDigits:0});
  const fmt2 = (n)=> '$' + (Math.round((n||0)*100)/100).toLocaleString(undefined,{minimumFractionDigits:2, maximumFractionDigits:2});
  const uid = ()=> Math.random().toString(36).slice(2,10);

  /* ---------------- Theme ---------------- */
  const root = document.documentElement;
  function applyTheme(t){ root.classList.remove('light','dark'); root.classList.add(t); LS.set('pfcc_theme', t); }
  applyTheme(LS.get('pfcc_theme','dark'));
  document.getElementById('themeBtn').addEventListener('click', ()=>{
    applyTheme(root.classList.contains('dark') ? 'light' : 'dark');
  });
  document.getElementById('printBtn').addEventListener('click', ()=> window.print());

  /* ---------------- Tab nav active state ---------------- */
  const tabs = Array.from(document.querySelectorAll('.tab'));
  const sections = tabs.map(t=> document.querySelector(t.getAttribute('href')));
  function onScroll(){
    let idx = 0;
    const y = window.scrollY + 120;
    sections.forEach((s,i)=>{ if(s && s.offsetTop <= y) idx = i; });
    tabs.forEach((t,i)=> t.classList.toggle('active', i===idx));
  }
  window.addEventListener('scroll', onScroll, {passive:true});

  /* ---------------- Seed data (first run only) ---------------- */
  function seedIfEmpty(){
    if(LS.get('pfcc_seeded', false)) return;
    const d = new Date(); const iso = (offset)=>{ const x=new Date(d); x.setDate(x.getDate()-offset); return x.toISOString().slice(0,10); };
    LS.set('pfcc_income', [
      {id:uid(), source:'Campus Job', amount:180, frequency:'weekly', date: iso(3)},
      {id:uid(), source:'Weekend Tutoring', amount:60, frequency:'biweekly', date: iso(10)}
    ]);
    LS.set('pfcc_expenses', [
      {id:uid(), category:'Food', amount:9.5, note:'Cafeteria lunch', date: iso(1)},
      {id:uid(), category:'Food', amount:32, note:'Groceries', date: iso(5)},
      {id:uid(), category:'Transport', amount:20, note:'Bus pass top-up', date: iso(6)},
      {id:uid(), category:'Textbooks & Supplies', amount:45, note:'Lab manual', date: iso(12)},
      {id:uid(), category:'Entertainment', amount:15, note:'Movie night', date: iso(8)},
      {id:uid(), category:'Subscriptions', amount:9.99, note:'Netflix', date: iso(20)},
      {id:uid(), category:'Other', amount:18, note:'Printing & supplies', date: iso(15)}
    ]);
    LS.set('pfcc_budget', {Food:180, 'Textbooks & Supplies':60, Transport:50, Entertainment:40, Subscriptions:30, Housing:0, Other:40});
    LS.set('pfcc_goal', {name:'New Laptop', target:1200, saved:350, targetDate:(()=>{ const x=new Date(); x.setMonth(x.getMonth()+4); return x.toISOString().slice(0,10); })()});
    LS.set('pfcc_subscriptions', [
      {id:uid(), name:'Spotify', amount:5.99, cycle:'monthly'},
      {id:uid(), name:'Netflix', amount:9.99, cycle:'monthly'},
      {id:uid(), name:'Campus Gym', amount:15, cycle:'monthly'}
    ]);
    LS.set('pfcc_checklist', {});
    LS.set('pfcc_seeded', true);
  }
  seedIfEmpty();

  /* ---------------- State ---------------- */
  let income = LS.get('pfcc_income', []);
  let expenses = LS.get('pfcc_expenses', []);
  let budget = LS.get('pfcc_budget', {});
  let goal = LS.get('pfcc_goal', {name:'Savings Goal', target:1000, saved:0, targetDate: todayISO()});
  let subs = LS.get('pfcc_subscriptions', []);
  let checklist = LS.get('pfcc_checklist', {});

  const CATS = ['Food','Textbooks & Supplies','Transport','Entertainment','Subscriptions','Housing','Other'];
  const CAT_COLORS = {
    'Food':'#2AC9B6','Textbooks & Supplies':'#F2B84B','Transport':'#7C8798',
    'Entertainment':'#F1706B','Subscriptions':'#8E7BE0','Housing':'#4FA7DE','Other':'#B1A98F'
  };

  function monthlyIncome(){
    return income.reduce((sum,i)=>{
      const mult = i.frequency==='weekly'?4.33 : i.frequency==='biweekly'?2.166 : i.frequency==='monthly'?1 : 0;
      return sum + (i.amount||0)*mult;
    },0);
  }
  function currentMonthExpenses(){
    const now = new Date(); const ym = now.getFullYear()+'-'+String(now.getMonth()+1).padStart(2,'0');
    return expenses.filter(e=> (e.date||'').slice(0,7)===ym);
  }
  function totalExpensesAllTime(){ return expenses.reduce((s,e)=> s+(e.amount||0),0); }
  function monthlySubTotal(){
    return subs.reduce((s,x)=> s + (x.cycle==='yearly' ? x.amount/12 : x.amount), 0);
  }
  function expensesByCategory(list){
    const map = {}; CATS.forEach(c=> map[c]=0);
    list.forEach(e=>{ map[e.category] = (map[e.category]||0) + (e.amount||0); });
    return map;
  }

  /* ---------------- Render: Income ---------------- */
  function renderIncome(){
    const list = document.getElementById('incomeList');
    list.innerHTML = '';
    if(income.length===0){ list.innerHTML = '<div style="color:var(--fg-dim);font-size:12.5px;">No income logged yet.</div>'; }
    income.slice().reverse().forEach(i=>{
      const row = document.createElement('div'); row.className='list-row';
      row.innerHTML = `<div><div>${escapeHtml(i.source)}</div><div class="meta">${i.frequency} &middot; ${i.date}</div></div>
        <div style="display:flex;align-items:center;"><span class="amt">${fmt2(i.amount)}</span><button class="del" data-id="${i.id}" title="Delete">&times;</button></div>`;
      row.querySelector('.del').addEventListener('click', ()=>{
        income = income.filter(x=>x.id!==i.id); LS.set('pfcc_income', income); renderAll();
      });
      list.appendChild(row);
    });
    document.getElementById('incTotal').textContent = fmt(monthlyIncome())+' / mo';
  }

  /* ---------------- Render: Expenses + donut ---------------- */
  function renderExpenses(){
    const list = document.getElementById('expenseList');
    list.innerHTML='';
    if(expenses.length===0){ list.innerHTML = '<div style="color:var(--fg-dim);font-size:12.5px;">No expenses logged yet.</div>'; }
    expenses.slice().reverse().slice(0,40).forEach(e=>{
      const row = document.createElement('div'); row.className='list-row';
      row.innerHTML = `<div><div>${escapeHtml(e.note||e.category)}</div><div class="meta">${escapeHtml(e.category)} &middot; ${e.date}</div></div>
        <div style="display:flex;align-items:center;"><span class="amt">${fmt2(e.amount)}</span><button class="del" data-id="${e.id}" title="Delete">&times;</button></div>`;
      row.querySelector('.del').addEventListener('click', ()=>{
        expenses = expenses.filter(x=>x.id!==e.id); LS.set('pfcc_expenses', expenses); renderAll();
      });
      list.appendChild(row);
    });
    document.getElementById('expTotal').textContent = fmt(currentMonthExpenses().reduce((s,e)=>s+e.amount,0)) + ' this month';
    renderDonut();
  }

  function renderDonut(){
    const data = expensesByCategory(currentMonthExpenses());
    const entries = Object.entries(data).filter(([,v])=> v>0);
    const holder = document.getElementById('donutHolder');
    const legend = document.getElementById('donutLegend');
    legend.innerHTML='';
    if(entries.length===0){ holder.innerHTML = '<div style="color:var(--fg-dim);font-size:12.5px;padding:20px 0;">No spending logged this month yet.</div>'; return; }
    const total = entries.reduce((s,[,v])=>s+v,0);
    const cx=110, cy=110, r=80, inner=48;
    let angle = -90;
    let paths = '';
    entries.forEach(([cat,val])=>{
      const frac = val/total; const sweep = frac*360;
      const x1 = cx + r*Math.cos(angle*Math.PI/180), y1 = cy + r*Math.sin(angle*Math.PI/180);
      const endAngle = angle+sweep;
      const x2 = cx + r*Math.cos(endAngle*Math.PI/180), y2 = cy + r*Math.sin(endAngle*Math.PI/180);
      const ix1 = cx + inner*Math.cos(angle*Math.PI/180), iy1 = cy + inner*Math.sin(angle*Math.PI/180);
      const ix2 = cx + inner*Math.cos(endAngle*Math.PI/180), iy2 = cy + inner*Math.sin(endAngle*Math.PI/180);
      const large = sweep>180?1:0;
      const d = `M ${x1} ${y1} A ${r} ${r} 0 ${large} 1 ${x2} ${y2} L ${ix2} ${iy2} A ${inner} ${inner} 0 ${large} 0 ${ix1} ${iy1} Z`;
      paths += `<path d="${d}" fill="${CAT_COLORS[cat]||'#999'}"><title>${cat}: ${fmt(val)}</title></path>`;
      legend.innerHTML += `<div class="item"><span class="swatch" style="background:${CAT_COLORS[cat]||'#999'};"></span>${cat} &middot; ${fmt(val)}</div>`;
      angle = endAngle;
    });
    holder.innerHTML = `<svg viewBox="0 0 220 220" style="max-width:240px;margin:0 auto;">${paths}
      <text x="110" y="106" text-anchor="middle" font-family="var(--font-mono)" font-size="18" fill="currentColor" font-weight="700">${fmt(total)}</text>
      <text x="110" y="124" text-anchor="middle" font-size="10" fill="currentColor" opacity="0.6">this month</text></svg>`;
  }

  /* ---------------- Render: Budget ---------------- */
  function renderBudget(){
    const holder = document.getElementById('budgetRows');
    const spentByCat = expensesByCategory(currentMonthExpenses());
    holder.innerHTML='';
    CATS.forEach(cat=>{
      const cap = budget[cat]||0;
      const spent = spentByCat[cat]||0;
      const pct = cap>0 ? Math.min(100,(spent/cap)*100) : (spent>0?100:0);
      const over = cap>0 && spent>cap;
      const row = document.createElement('div'); row.className='budget-row';
      row.innerHTML = `
        <div class="top"><span class="cat">${cat}</span><span class="nums mono">${fmt(spent)} / <input type="number" data-cat="${cat}" class="budget-input mono" value="${cap}" style="width:64px;padding:2px 6px;display:inline-block;"></span></div>
        <div class="budget-track"><div class="budget-fill" style="width:${pct}%;background:${over?'var(--coral)':'var(--teal)'};"></div></div>`;
      holder.appendChild(row);
    });
    holder.querySelectorAll('.budget-input').forEach(inp=>{
      inp.addEventListener('change', ()=>{
        budget[inp.dataset.cat] = parseFloat(inp.value)||0;
        LS.set('pfcc_budget', budget); renderAll();
      });
    });
  }

  /* ---------------- Render: Goal ---------------- */
  function projectGoalDate(extraMonthly){
    const netMonthly = monthlyIncome() - (currentMonthExpenses().reduce((s,e)=>s+e.amount,0)) + (extraMonthly||0);
    const remaining = Math.max(0,(goal.target||0) - (goal.saved||0));
    if(netMonthly<=0 || remaining===0) return null;
    const months = remaining/netMonthly;
    const d = new Date(); d.setMonth(d.getMonth()+Math.ceil(months));
    return {date:d, months};
  }

  function renderGoal(){
    document.getElementById('goalName').value = goal.name;
    document.getElementById('goalTarget').value = goal.target;
    document.getElementById('goalDate').value = goal.targetDate;
    document.getElementById('goalTitle').textContent = goal.name;
    document.getElementById('goalAmt').innerHTML = fmt(goal.saved)+' <span style="font-size:14px;color:var(--fg-dim);font-weight:400;">saved</span>';
    document.getElementById('goalTargetLabel').textContent = 'of '+fmt(goal.target)+' target';

    const pct = goal.target>0 ? Math.min(100,(goal.saved/goal.target)*100) : 0;
    const r=70, c=2*Math.PI*r;
    document.getElementById('goalRingHolder').innerHTML = `
      <svg width="180" height="180" viewBox="0 0 180 180">
        <circle cx="90" cy="90" r="${r}" fill="none" stroke="var(--surface-2)" stroke-width="14"/>
        <circle cx="90" cy="90" r="${r}" fill="none" stroke="var(--gold)" stroke-width="14" stroke-linecap="round"
          stroke-dasharray="${c}" stroke-dashoffset="${c-(pct/100)*c}" transform="rotate(-90 90 90)"/>
        <text x="90" y="86" text-anchor="middle" font-family="var(--font-mono)" font-size="24" font-weight="700" fill="currentColor">${Math.round(pct)}%</text>
        <text x="90" y="106" text-anchor="middle" font-size="10" fill="currentColor" opacity="0.6">to goal</text>
      </svg>`;

    const proj = projectGoalDate(0);
    const etaEl = document.getElementById('goalEta');
    if(!proj){ etaEl.textContent = goal.saved>=goal.target ? 'Goal reached! 🎉' : 'Log income to project a finish date.'; }
    else{ etaEl.textContent = `At current pace, projected finish: ${proj.date.toLocaleDateString(undefined,{month:'short',year:'numeric'})} (${Math.ceil(proj.months)} mo)`; }
  }

  /* ---------------- Render: Subscriptions ---------------- */
  function renderSubs(){
    const list = document.getElementById('subList'); list.innerHTML='';
    if(subs.length===0){ list.innerHTML='<div style="color:var(--fg-dim);font-size:12.5px;">No subscriptions added yet.</div>'; }
    subs.slice().reverse().forEach(s=>{
      const row = document.createElement('div'); row.className='list-row';
      row.innerHTML = `<div><div>${escapeHtml(s.name)}</div><div class="meta">${s.cycle}</div></div>
        <div style="display:flex;align-items:center;"><span class="amt">${fmt2(s.amount)}</span><button class="del" data-id="${s.id}">&times;</button></div>`;
      row.querySelector('.del').addEventListener('click', ()=>{
        subs = subs.filter(x=>x.id!==s.id); LS.set('pfcc_subscriptions', subs); renderAll();
      });
      list.appendChild(row);
    });
    document.getElementById('subTotal').textContent = fmt(monthlySubTotal())+' / mo';
  }

  /* ---------------- Render: Cash flow (last 6 months) ---------------- */
  function renderCashflow(){
    const months = [];
    const now = new Date();
    for(let i=5;i>=0;i--){
      const d = new Date(now.getFullYear(), now.getMonth()-i, 1);
      months.push({ym: d.getFullYear()+'-'+String(d.getMonth()+1).padStart(2,'0'), label:d.toLocaleDateString(undefined,{month:'short'})});
    }
    const incMonthly = monthlyIncome();
    const bars = months.map(m=>{
      const exp = expenses.filter(e=>(e.date||'').slice(0,7)===m.ym).reduce((s,e)=>s+e.amount,0);
      return {label:m.label, income: incMonthly, expense: exp};
    });
    const max = Math.max(1, ...bars.map(b=>Math.max(b.income,b.expense)));
    const w=680, h=220, padL=30, padB=26, groupW = (w-padL)/bars.length;
    let svg = `<svg viewBox="0 0 ${w} ${h}" style="width:100%;height:auto;">`;
    bars.forEach((b,i)=>{
      const x = padL + i*groupW + 10;
      const incH = (b.income/max)*(h-padB-10);
      const expH = (b.expense/max)*(h-padB-10);
      svg += `<rect x="${x}" y="${h-padB-incH}" width="16" height="${incH}" rx="3" fill="var(--teal)"><title>Income ${fmt(b.income)}</title></rect>`;
      svg += `<rect x="${x+20}" y="${h-padB-expH}" width="16" height="${expH}" rx="3" fill="var(--coral)"><title>Expenses ${fmt(b.expense)}</title></rect>`;
      svg += `<text x="${x+18}" y="${h-8}" text-anchor="middle" font-size="10" fill="currentColor" opacity="0.6">${b.label}</text>`;
    });
    svg += `</svg>`;
    document.getElementById('cashflowHolder').innerHTML = svg;
  }

  /* ---------------- GPA / Health Score ---------------- */
  function computeScore(){
    const inc = monthlyIncome();
    const exp = currentMonthExpenses().reduce((s,e)=>s+e.amount,0);
    const savingsRate = inc>0 ? Math.max(0,Math.min(1,(inc-exp)/inc)) : 0;

    let inBudgetCount=0, budgetedCats=0;
    const spentByCat = expensesByCategory(currentMonthExpenses());
    CATS.forEach(c=>{
      if(budget[c]>0){ budgetedCats++; if((spentByCat[c]||0) <= budget[c]) inBudgetCount++; }
    });
    const budgetAdherence = budgetedCats>0 ? inBudgetCount/budgetedCats : 0.6;

    const subRatio = inc>0 ? Math.min(1, monthlySubTotal()/inc) : (monthlySubTotal()>0?1:0);
    const spendingControl = Math.max(0, 1 - subRatio*2.2);

    const goalProgress = goal.target>0 ? Math.max(0,Math.min(1, goal.saved/goal.target)) : 0.3;

    const weighted = savingsRate*0.35 + budgetAdherence*0.3 + spendingControl*0.15 + goalProgress*0.2;
    const score100 = Math.round(weighted*100);
    return {score100, savingsRate, budgetAdherence, spendingControl, goalProgress};
  }

  function gradeFor(pct){ // pct 0-1
    if(pct>=0.9) return {letter:'A', color:'var(--teal)'};
    if(pct>=0.8) return {letter:'B', color:'var(--teal)'};
    if(pct>=0.7) return {letter:'C', color:'var(--gold)'};
    if(pct>=0.6) return {letter:'D', color:'var(--gold)'};
    return {letter:'F', color:'var(--coral)'};
  }

  function renderTranscript(){
    const s = computeScore();
    const gpa = (s.score100/100*4).toFixed(1);
    document.getElementById('gpaNum').textContent = gpa;
    const overallGrade = gradeFor(s.score100/100);
    const letterEl = document.getElementById('gpaLetter');
    letterEl.textContent = overallGrade.letter;
    letterEl.style.borderColor = overallGrade.color; letterEl.style.color = overallGrade.color;

    const noteMap = {
      A:'Excellent standing — you\'re saving consistently and staying within budget.',
      B:'Solid standing — a few tweaks would push this into top marks.',
      C:'Passing, but there\'s room to tighten spending or boost savings.',
      D:'At risk — expenses or subscriptions may be eating into your goal.',
      F:'Needs attention — start by logging a budget and trimming one category.'
    };
    document.getElementById('gpaNote').textContent = noteMap[overallGrade.letter];

    const rows = [
      {name:'Savings Rate', desc:'Share of income kept, not spent', pct:s.savingsRate},
      {name:'Budget Adherence', desc:'Categories kept within their cap', pct:s.budgetAdherence},
      {name:'Spending Control', desc:'Recurring costs vs. income', pct:s.spendingControl},
      {name:'Goal Progress', desc:'Progress toward '+ (goal.name||'your goal'), pct:s.goalProgress}
    ];
    const holder = document.getElementById('transcriptRows');
    holder.innerHTML = '<h3>Course Breakdown</h3>';
    rows.forEach(r=>{
      const g = gradeFor(r.pct);
      const div = document.createElement('div'); div.className='trow';
      div.innerHTML = `<div><div class="name">${r.name}</div><div class="desc">${r.desc}</div></div>
        <div class="bar-track"><div class="bar-fill" style="width:${Math.round(r.pct*100)}%;background:${g.color};"></div></div>
        <div class="grade-badge" style="background:${g.color};color:#0B1119;">${g.letter}</div>`;
      holder.appendChild(div);
    });

    // stats
    const inc = monthlyIncome(); const exp = currentMonthExpenses().reduce((s,e)=>s+e.amount,0);
    document.getElementById('statIncome').textContent = fmt(inc);
    document.getElementById('statIncomeSrc').textContent = income.length + ' source' + (income.length===1?'':'s');
    document.getElementById('statExpenses').textContent = fmt(exp);
    document.getElementById('statExpenseDelta').textContent = currentMonthExpenses().length + ' logged this month';
    document.getElementById('statExpenseDelta').className = 'delta flat';
    const net = inc-exp;
    document.getElementById('statNet').textContent = (net>=0?'+':'') + fmt(net);
    document.getElementById('statNet').parentElement.querySelector('.num').style.color = net>=0 ? 'var(--teal)':'var(--coral)';
    document.getElementById('statNetNote').textContent = net>=0 ? 'Positive this month' : 'Spending more than earning';
    document.getElementById('statNetNote').className = 'delta ' + (net>=0?'up':'down');
    const gp = goal.target>0 ? Math.round((goal.saved/goal.target)*100) : 0;
    document.getElementById('statGoal').textContent = gp+'%';
    document.getElementById('statGoalNote').textContent = goal.name;
  }

  /* ---------------- Insights (rule-based) ---------------- */
  function renderInsights(){
    const s = computeScore();
    const inc = monthlyIncome(); const exp = currentMonthExpenses().reduce((s2,e)=>s2+e.amount,0);
    const card = document.getElementById('insightsCard'); card.innerHTML='';
    const items = [];

    if(inc>0 && (inc-exp)/inc < 0.1){
      items.push({type:'warn', txt:`Your savings rate is under <b>10%</b> this month. Even trimming one category by ${fmt(20)} would help.`});
    } else if(inc>0 && (inc-exp)/inc >= 0.3){
      items.push({type:'', txt:`Strong work — you're saving over <b>${Math.round(((inc-exp)/inc)*100)}%</b> of your income this month.`});
    }

    const subTotal = monthlySubTotal();
    if(inc>0 && subTotal/inc > 0.1){
      items.push({type:'warn', txt:`Subscriptions are eating <b>${Math.round((subTotal/inc)*100)}%</b> of your income (${fmt(subTotal)}/mo). Consider pausing one you use least.`});
    }

    const spentByCat = expensesByCategory(currentMonthExpenses());
    CATS.forEach(c=>{
      if(budget[c]>0 && spentByCat[c]>budget[c]){
        items.push({type:'warn', txt:`You're <b>${fmt(spentByCat[c]-budget[c])} over budget</b> in ${c} this month.`});
      }
    });

    const proj = projectGoalDate(0);
    if(proj){
      items.push({type:'tip', txt:`At your current pace, you'll hit <b>${goal.name}</b> around <b>${proj.date.toLocaleDateString(undefined,{month:'long',year:'numeric'})}</b>.`});
    } else if(goal.saved>=goal.target && goal.target>0){
      items.push({type:'', txt:`You've fully funded <b>${goal.name}</b> — nice work! Consider setting a new goal.`});
    }

    if(income.length===0){
      items.push({type:'tip', txt:'Log your part-time paycheck in the Income module to unlock accurate projections.'});
    }

    if(items.length===0){ items.push({type:'tip', txt:'Add a few income and expense entries to start generating personalized insights.'}); }

    items.forEach(it=>{
      const div = document.createElement('div'); div.className = 'insight ' + (it.type||'');
      div.innerHTML = `<div class="txt">${it.txt}</div>`;
      card.appendChild(div);
    });
  }

  /* ---------------- What-if ---------------- */
  function renderWhatif(){
    const saveEl = document.getElementById('whatifSave'), subEl = document.getElementById('whatifSub'),
          hoursEl = document.getElementById('whatifHours'), wageEl = document.getElementById('whatifWage');
    function update(){
      const extraSaveWeekly = parseFloat(saveEl.value)||0;
      const cutSub = parseFloat(subEl.value)||0;
      const extraHours = parseFloat(hoursEl.value)||0;
      const wage = parseFloat(wageEl.value)||0;
      document.getElementById('whatifSaveVal').textContent = fmt(extraSaveWeekly)+'/wk';
      document.getElementById('whatifSubVal').textContent = fmt(cutSub)+'/mo';
      document.getElementById('whatifHoursVal').textContent = extraHours+' hrs';

      const extraMonthly = extraSaveWeekly*4.33 + cutSub + extraHours*4.33*wage;
      const inc = monthlyIncome() + extraHours*4.33*wage;
      const exp = currentMonthExpenses().reduce((s,e)=>s+e.amount,0);
      const newNet = inc-exp + extraSaveWeekly*4.33 + cutSub - (extraHours*4.33*wage - extraHours*4.33*wage); // net already includes extra income once
      const monthlySavings = (inc-exp) + extraSaveWeekly*4.33 + cutSub;

      document.getElementById('wifSavings').textContent = fmt(monthlySavings);

      const remaining = Math.max(0,(goal.target||0)-(goal.saved||0));
      if(monthlySavings>0 && remaining>0){
        const months = remaining/monthlySavings;
        const d = new Date(); d.setMonth(d.getMonth()+Math.ceil(months));
        document.getElementById('wifDate').textContent = d.toLocaleDateString(undefined,{month:'short',year:'numeric'});
      } else if(remaining===0){
        document.getElementById('wifDate').textContent = 'Already met';
      } else {
        document.getElementById('wifDate').textContent = 'N/A';
      }

      // projected GPA with new savings rate substituted
      const savingsRate = inc>0 ? Math.max(0,Math.min(1,(monthlySavings)/inc)) : 0;
      const s = computeScore();
      const weighted = savingsRate*0.35 + s.budgetAdherence*0.3 + s.spendingControl*0.15 + s.goalProgress*0.2;
      document.getElementById('wifGpa').textContent = (weighted*4).toFixed(1);
    }
    [saveEl,subEl,hoursEl,wageEl].forEach(el=> el.addEventListener('input', update));
    update();
  }

  /* ---------------- Checklist ---------------- */
  const CHECKLIST_ITEMS = [
    {id:'chk1', label:'Log this month\'s income'},
    {id:'chk2', label:'Log this week\'s expenses'},
    {id:'chk3', label:'Set a budget cap for every category'},
    {id:'chk4', label:'Review subscriptions and cancel one unused'},
    {id:'chk5', label:'Add a contribution to your savings goal'},
    {id:'chk6', label:'Check your Money GPA and read the insights'}
  ];
  function renderChecklist(){
    const holder = document.getElementById('checklistHolder'); holder.innerHTML='';
    CHECKLIST_ITEMS.forEach(item=>{
      const done = !!checklist[item.id];
      const row = document.createElement('div'); row.className = 'checklist-item' + (done?' done':'');
      row.innerHTML = `<input type="checkbox" id="${item.id}" ${done?'checked':''}><label for="${item.id}">${item.label}</label>`;
      row.querySelector('input').addEventListener('change', (e)=>{
        checklist[item.id] = e.target.checked; LS.set('pfcc_checklist', checklist); renderChecklist();
      });
      holder.appendChild(row);
    });
  }

  /* ---------------- Prompt chips ---------------- */
  const PROMPTS = [
    'Explain the 50/30/20 budgeting rule for a student income',
    'How much should I keep in an emergency fund as a student?',
    'What is a realistic monthly savings rate for a part-time job?',
    'How do taxes typically work for part-time student income?',
    'What questions should I ask before opening a student credit card?',
    'How can I build credit safely as a student?'
  ];
  function renderPrompts(){
    const holder = document.getElementById('promptChips'); holder.innerHTML='';
    PROMPTS.forEach(p=>{
      const chip = document.createElement('div'); chip.className='chip'; chip.textContent = p;
      holder.appendChild(chip);
    });
  }

  /* ---------------- Calculators ---------------- */
  document.getElementById('calcGoalBtn').addEventListener('click', ()=>{
    const target = parseFloat(document.getElementById('calcTarget').value)||0;
    const saved = parseFloat(document.getElementById('calcSaved').value)||0;
    const monthly = parseFloat(document.getElementById('calcMonthly').value)||0;
    const remaining = Math.max(0,target-saved);
    const months = monthly>0 ? Math.ceil(remaining/monthly) : Infinity;
    const res = document.getElementById('calcGoalResult');
    res.innerHTML = `<div><div class="k">Months needed</div><div class="v">${isFinite(months)?months:'∞'}</div></div>
      <div><div class="k">Remaining</div><div class="v">${fmt(remaining)}</div></div>`;
  });
  document.getElementById('calcPayBtn').addEventListener('click', ()=>{
    const hours = parseFloat(document.getElementById('calcHours').value)||0;
    const rate = parseFloat(document.getElementById('calcRate').value)||0;
    const tax = parseFloat(document.getElementById('calcTax').value)||0;
    const gross = hours*rate*4.33;
    const net = gross*(1-tax/100);
    const res = document.getElementById('calcPayResult');
    res.innerHTML = `<div><div class="k">Gross / month</div><div class="v">${fmt(gross)}</div></div>
      <div><div class="k">Take-home / month</div><div class="v">${fmt(net)}</div></div>`;
  });

  /* ---------------- Form handlers ---------------- */
  function escapeHtml(str){ const d=document.createElement('div'); d.textContent=str||''; return d.innerHTML; }

  document.getElementById('addIncomeBtn').addEventListener('click', ()=>{
    const source = document.getElementById('incSource').value.trim() || 'Income';
    const amount = parseFloat(document.getElementById('incAmount').value)||0;
    const frequency = document.getElementById('incFreq').value;
    const date = document.getElementById('incDate').value || todayISO();
    if(amount<=0) return;
    income.push({id:uid(), source, amount, frequency, date});
    LS.set('pfcc_income', income);
    document.getElementById('incSource').value=''; document.getElementById('incAmount').value='';
    renderAll();
  });

  document.getElementById('addExpenseBtn').addEventListener('click', ()=>{
    const category = document.getElementById('expCategory').value;
    const amount = parseFloat(document.getElementById('expAmount').value)||0;
    const note = document.getElementById('expNote').value.trim();
    const date = document.getElementById('expDate').value || todayISO();
    if(amount<=0) return;
    expenses.push({id:uid(), category, amount, note, date});
    LS.set('pfcc_expenses', expenses);
    document.getElementById('expAmount').value=''; document.getElementById('expNote').value='';
    renderAll();
  });

  document.getElementById('addSubBtn').addEventListener('click', ()=>{
    const name = document.getElementById('subName').value.trim();
    const amount = parseFloat(document.getElementById('subAmount').value)||0;
    const cycle = document.getElementById('subCycle').value;
    if(!name || amount<=0) return;
    subs.push({id:uid(), name, amount, cycle});
    LS.set('pfcc_subscriptions', subs);
    document.getElementById('subName').value=''; document.getElementById('subAmount').value='';
    renderAll();
  });

  document.getElementById('saveGoalBtn').addEventListener('click', ()=>{
    goal.name = document.getElementById('goalName').value.trim() || goal.name;
    goal.target = parseFloat(document.getElementById('goalTarget').value)||goal.target;
    goal.targetDate = document.getElementById('goalDate').value || goal.targetDate;
    LS.set('pfcc_goal', goal);
    renderAll();
  });

  document.getElementById('addContribBtn').addEventListener('click', ()=>{
    const amt = parseFloat(document.getElementById('goalContribAmt').value)||0;
    if(amt<=0) return;
    goal.saved = (goal.saved||0) + amt;
    LS.set('pfcc_goal', goal);
    document.getElementById('goalContribAmt').value='';
    renderAll();
  });

  /* ---------------- Master render ---------------- */
  function renderAll(){
    renderTranscript();
    renderIncome();
    renderExpenses();
    renderBudget();
    renderGoal();
    renderSubs();
    renderCashflow();
    renderInsights();
  }

  renderAll();
  renderWhatif();
  renderChecklist();
  renderPrompts();
  onScroll();
})();
</script>
</body>
</html>
