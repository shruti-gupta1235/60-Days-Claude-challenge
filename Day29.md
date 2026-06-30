<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Operation Lifeline: Supply Chain Crisis Lab</title>
<script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
<style>
  :root{
    --bg-0:#0b0e14;
    --bg-1:#11151f;
    --bg-2:#161c29;
    --bg-3:#1d2433;
    --border:#2a3346;
    --text-0:#eef1f8;
    --text-1:#aab3c5;
    --text-2:#6e7892;
    --accent:#5b8cff;
    --accent-2:#8b5bff;
    --good:#36d399;
    --warn:#fbbd23;
    --bad:#f87272;
    --shadow: 0 10px 30px rgba(0,0,0,0.45);
  }
  *{box-sizing:border-box;}
  html,body{margin:0;padding:0;}
  body{
    background: radial-gradient(circle at 20% 0%, #141b2b 0%, var(--bg-0) 55%) fixed;
    color:var(--text-0);
    font-family: 'Segoe UI', 'Inter', system-ui, -apple-system, sans-serif;
    min-height:100vh;
  }
  #root{min-height:100vh;}
  .app{
    max-width:1100px;
    margin:0 auto;
    padding:28px 20px 80px;
  }
  .topbar{
    display:flex;
    align-items:center;
    justify-content:space-between;
    margin-bottom:24px;
  }
  .brand{
    display:flex;
    align-items:center;
    gap:10px;
    font-weight:700;
    letter-spacing:0.3px;
    color:var(--text-0);
  }
  .brand .dot{
    width:10px;height:10px;border-radius:50%;
    background:linear-gradient(135deg,var(--accent),var(--accent-2));
    box-shadow:0 0 12px var(--accent);
  }
  .step-pill{
    font-size:12px;
    color:var(--text-2);
    border:1px solid var(--border);
    padding:6px 12px;
    border-radius:999px;
    background:var(--bg-2);
  }
  .card{
    background:linear-gradient(180deg, var(--bg-2), var(--bg-1));
    border:1px solid var(--border);
    border-radius:18px;
    padding:24px;
    box-shadow:var(--shadow);
    transition:transform .25s ease, box-shadow .25s ease, border-color .25s ease;
  }
  .card:hover{
    border-color:#3a4665;
  }
  .grid{
    display:grid;
    gap:16px;
  }
  .grid-2{grid-template-columns:repeat(2,1fr);}
  .grid-3{grid-template-columns:repeat(3,1fr);}
  .grid-4{grid-template-columns:repeat(4,1fr);}
  @media (max-width:760px){
    .grid-2, .grid-3, .grid-4{grid-template-columns:1fr;}
  }
  h1{font-size:38px;margin:0 0 10px;font-weight:800;letter-spacing:-0.5px;}
  h2{font-size:26px;margin:0 0 8px;font-weight:800;letter-spacing:-0.3px;}
  h3{font-size:18px;margin:0 0 6px;font-weight:700;}
  p{line-height:1.55;color:var(--text-1);margin:0 0 12px;}
  .subtitle{color:var(--text-1);font-size:17px;margin-bottom:28px;}
  .badge{
    display:inline-block;
    font-size:12px;
    font-weight:700;
    padding:4px 10px;
    border-radius:999px;
    letter-spacing:0.3px;
    text-transform:uppercase;
  }
  .badge-accent{background:rgba(91,140,255,0.15);color:#9bb8ff;border:1px solid rgba(91,140,255,0.35);}
  .badge-warn{background:rgba(251,189,35,0.15);color:#ffd874;border:1px solid rgba(251,189,35,0.35);}
  .badge-bad{background:rgba(248,114,114,0.15);color:#ffaaaa;border:1px solid rgba(248,114,114,0.35);}
  .badge-good{background:rgba(54,211,153,0.15);color:#7df0c2;border:1px solid rgba(54,211,153,0.35);}

  button{
    font-family:inherit;
    cursor:pointer;
    border:none;
    outline:none;
  }
  .btn{
    background:linear-gradient(135deg, var(--accent), var(--accent-2));
    color:white;
    font-weight:700;
    font-size:15px;
    padding:14px 26px;
    border-radius:12px;
    transition:transform .15s ease, box-shadow .15s ease, opacity .15s ease;
    box-shadow:0 8px 20px rgba(91,140,255,0.25);
  }
  .btn:hover{transform:translateY(-2px);box-shadow:0 12px 26px rgba(91,140,255,0.35);}
  .btn:active{transform:translateY(0px);}
  .btn:disabled{opacity:0.4;cursor:not-allowed;transform:none;box-shadow:none;}
  .btn-ghost{
    background:var(--bg-3);
    color:var(--text-0);
    border:1px solid var(--border);
    font-weight:600;
    padding:12px 22px;
    border-radius:12px;
    transition:all .15s ease;
  }
  .btn-ghost:hover{border-color:var(--accent);color:#bcd0ff;}

  .option{
    text-align:left;
    width:100%;
    background:var(--bg-3);
    border:1px solid var(--border);
    border-radius:14px;
    padding:16px 18px;
    color:var(--text-0);
    margin-bottom:12px;
    transition:all .18s ease;
    display:block;
  }
  .option:hover{border-color:var(--accent);background:#202a3f;transform:translateX(2px);}
  .option.selected{
    border-color:var(--accent);
    background:linear-gradient(135deg, rgba(91,140,255,0.18), rgba(139,91,255,0.12));
    box-shadow:0 0 0 1px var(--accent) inset;
  }
  .option.disabled{opacity:0.4;cursor:not-allowed;}
  .option-title{font-weight:700;margin-bottom:4px;font-size:15.5px;}
  .option-desc{font-size:13.5px;color:var(--text-2);margin:0;line-height:1.45;}

  .why-box{
    background:rgba(91,140,255,0.08);
    border:1px dashed rgba(91,140,255,0.4);
    border-radius:12px;
    padding:14px 16px;
    font-size:13.5px;
    color:#aebde6;
    margin:14px 0;
  }
  .why-box b{color:#cdd9ff;}

  .metric-row{
    display:flex;
    align-items:center;
    gap:14px;
    margin-bottom:14px;
  }
  .metric-label{
    width:160px;
    font-size:13.5px;
    color:var(--text-1);
    font-weight:600;
    flex-shrink:0;
  }
  .metric-track{
    flex:1;
    height:10px;
    background:var(--bg-3);
    border-radius:999px;
    overflow:hidden;
    border:1px solid var(--border);
  }
  .metric-fill{
    height:100%;
    border-radius:999px;
    transition:width 1s cubic-bezier(.22,.9,.3,1);
  }
  .metric-value{
    width:56px;
    text-align:right;
    font-size:13px;
    font-weight:700;
    color:var(--text-0);
    flex-shrink:0;
  }

  .footer-nav{
    display:flex;
    justify-content:space-between;
    align-items:center;
    margin-top:24px;
  }

  .small{font-size:13px;color:var(--text-2);}
  .stat-card{
    background:var(--bg-3);
    border:1px solid var(--border);
    border-radius:14px;
    padding:16px;
  }
  .stat-card .stat-label{font-size:12px;color:var(--text-2);text-transform:uppercase;letter-spacing:0.5px;margin-bottom:6px;}
  .stat-card .stat-value{font-size:22px;font-weight:800;color:var(--text-0);}

  .score-ring{
    width:170px;height:170px;
    border-radius:50%;
    display:flex;align-items:center;justify-content:center;
    margin:0 auto 18px;
    position:relative;
  }
  .score-ring .inner{
    width:130px;height:130px;
    border-radius:50%;
    background:var(--bg-1);
    display:flex;flex-direction:column;align-items:center;justify-content:center;
  }
  .score-ring .num{font-size:36px;font-weight:800;}
  .score-ring .lbl{font-size:11px;color:var(--text-2);text-transform:uppercase;letter-spacing:1px;}

  .tag-list{display:flex;flex-wrap:wrap;gap:8px;margin-top:10px;}
  .chip{
    background:var(--bg-3);
    border:1px solid var(--border);
    padding:6px 12px;
    border-radius:999px;
    font-size:12.5px;
    color:var(--text-1);
  }

  .fade-in{animation:fadeIn .4s ease;}
  @keyframes fadeIn{
    from{opacity:0; transform:translateY(8px);}
    to{opacity:1; transform:translateY(0);}
  }

  .crisis-banner{
    border-radius:16px;
    padding:20px 22px;
    margin-bottom:20px;
    background:linear-gradient(135deg, rgba(248,114,114,0.14), rgba(251,189,35,0.08));
    border:1px solid rgba(248,114,114,0.35);
  }

  ::-webkit-scrollbar{width:10px;}
  ::-webkit-scrollbar-track{background:var(--bg-0);}
  ::-webkit-scrollbar-thumb{background:#2a3346;border-radius:10px;}

  .feedback-block{
    border-left:3px solid var(--accent);
    padding:10px 16px;
    background:rgba(91,140,255,0.06);
    border-radius:0 10px 10px 0;
    margin-bottom:14px;
  }
  .feedback-block.good{border-color:var(--good);background:rgba(54,211,153,0.06);}
  .feedback-block.bad{border-color:var(--bad);background:rgba(248,114,114,0.06);}
</style>
</head>
<body>
<div id="root"></div>

<script type="text/babel">
const { useState, useEffect, useRef } = React;

/* ============================= DATA ============================= */

const INDUSTRIES = [
  "Consumer Electronics", "Automotive Parts", "Fashion Apparel", "Pharmaceuticals",
  "Packaged Foods", "Home Furniture", "Industrial Equipment", "Toys & Games",
  "Sporting Goods", "Cosmetics & Personal Care"
];

const COUNTRIES_POOL = [
  "Vietnam", "China", "India", "Mexico", "Germany", "Poland", "Brazil",
  "Turkey", "Indonesia", "South Korea", "USA", "Bangladesh", "Thailand", "Italy"
];

const COMPANY_PREFIXES = ["Northstar", "Atlas", "Vertex", "Summit", "Beacon", "Cobalt", "Meridian", "Pinnacle", "Harbor", "Quantum", "Foundry", "Lumen", "Cascade", "Ironwood"];
const COMPANY_SUFFIXES = ["Industries", "Group", "Holdings", "Global", "& Co.", "Manufacturing", "Logistics", "Brands", "Supply Co.", "Works"];

function randInt(min, max){ return Math.floor(Math.random() * (max - min + 1)) + min; }
function pick(arr){ return arr[randInt(0, arr.length - 1)]; }
function pickN(arr, n){
  const copy = [...arr];
  const out = [];
  for(let i=0;i<n && copy.length>0;i++){
    const idx = randInt(0, copy.length-1);
    out.push(copy[idx]);
    copy.splice(idx,1);
  }
  return out;
}
function clamp(v, min=0, max=100){ return Math.max(min, Math.min(max, v)); }

function generateCompany(){
  const name = `${pick(COMPANY_PREFIXES)} ${pick(COMPANY_SUFFIXES)}`;
  const industry = pick(INDUSTRIES);
  const revenue = randInt(80, 950); // millions
  const factories = randInt(2, 9);
  const warehouses = randInt(3, 14);
  const suppliers = randInt(10, 60);
  const inventoryDays = randInt(12, 55);
  const leadTime = randInt(7, 45);
  const countries = pickN(COUNTRIES_POOL, randInt(3,5));
  return { name, industry, revenue, factories, warehouses, suppliers, inventoryDays, leadTime, countries };
}

const CRISES = [
  {
    id: "fire",
    title: "Factory Fire",
    icon: "🔥",
    desc: "A major fire has broken out at one of your primary manufacturing plants overnight. Initial reports suggest production there will be offline for weeks.",
    impact: "Production capacity drops sharply. You risk missing customer orders unless you reroute manufacturing fast.",
    urgency: "Critical",
  },
  {
    id: "bankruptcy",
    title: "Supplier Bankruptcy",
    icon: "📉",
    desc: "Your second-largest raw material supplier has unexpectedly filed for bankruptcy, freezing all pending shipments.",
    impact: "A key input is suddenly unavailable. Without quick action, multiple product lines could stall.",
    urgency: "High",
  },
  {
    id: "port",
    title: "Port Strike",
    icon: "🚢",
    desc: "Dockworkers at one of your main shipping ports have gone on strike, halting container movement indefinitely.",
    impact: "Inbound and outbound shipments are stuck. Delivery timelines for thousands of orders are now at risk.",
    urgency: "High",
  },
  {
    id: "cyber",
    title: "Cyberattack",
    icon: "💻",
    desc: "A ransomware attack has locked your warehouse management and order-tracking systems.",
    impact: "Visibility into inventory and shipments has gone dark. Operations are flying blind until systems are restored.",
    urgency: "Critical",
  },
  {
    id: "flood",
    title: "Regional Flooding",
    icon: "🌊",
    desc: "Severe flooding has damaged roads and rail lines near one of your key distribution hubs.",
    impact: "Ground transportation in the region is crippled, threatening on-time delivery to a major market.",
    urgency: "Moderate",
  },
  {
    id: "shortage",
    title: "Raw Material Shortage",
    icon: "⚠️",
    desc: "A sudden global shortage of a critical raw material is driving prices up and squeezing availability.",
    impact: "Production costs are climbing fast, and you may not be able to source enough material to meet demand.",
    urgency: "High",
  },
  {
    id: "conflict",
    title: "Political Conflict",
    icon: "🛑",
    desc: "Escalating political tension in a country where you operate has triggered new export restrictions overnight.",
    impact: "Cross-border shipments are delayed by new inspections and tariffs, threatening your supply continuity.",
    urgency: "Critical",
  },
  {
    id: "delay",
    title: "Shipping Delay",
    icon: "🛳️",
    desc: "A backlog at sea has pushed average container transit times up by several weeks across your network.",
    impact: "Inventory replenishment is slowing down, raising the risk of stockouts during peak demand.",
    urgency: "Moderate",
  },
];

/* Crisis response actions: 6 offered, player picks 3.
   Each action affects: cost, inventory, profit, deliverySpeed, satisfaction (deltas -20..+20) */
const ACTION_POOL = [
  {
    id: "air_freight",
    title: "Switch to Air Freight",
    desc: "Move critical shipments by air instead of sea or rail to save time, at a much higher cost.",
    why: "Air freight can cut delivery time dramatically, but it can be 4-6x more expensive than ocean freight — a classic speed-vs-cost tradeoff.",
    effects: { cost: -16, inventory: 6, profit: -10, deliverySpeed: 18, satisfaction: 12 }
  },
  {
    id: "alt_supplier",
    title: "Activate Backup Supplier",
    desc: "Engage a pre-vetted secondary supplier to fill the gap, even though their pricing is less favorable.",
    why: "Having backup suppliers is a core resilience strategy. It costs more short-term but prevents total supply failure.",
    effects: { cost: -10, inventory: 14, profit: -4, deliverySpeed: 8, satisfaction: 8 }
  },
  {
    id: "reallocate",
    title: "Reallocate Inventory Across Warehouses",
    desc: "Shift existing stock from lower-priority regions to the affected market to plug the gap quickly.",
    why: "Reallocation is fast and cheap, but it borrows risk from other regions — satisfaction may dip elsewhere.",
    effects: { cost: -2, inventory: 10, profit: 2, deliverySpeed: 10, satisfaction: 4 }
  },
  {
    id: "freeze_promos",
    title: "Pause Promotions & Discounts",
    desc: "Temporarily halt marketing promotions to reduce sudden demand spikes while supply is constrained.",
    why: "Lowering demand pressure protects service levels, but it also reduces near-term revenue and may upset customers expecting deals.",
    effects: { cost: 4, inventory: 6, profit: -2, deliverySpeed: 2, satisfaction: -6 }
  },
  {
    id: "transparent_comms",
    title: "Proactively Communicate with Customers",
    desc: "Send customers early, honest updates about possible delays instead of staying silent.",
    why: "Research consistently shows that transparency during disruptions preserves trust even when delivery slips — silence is what damages loyalty most.",
    effects: { cost: -1, inventory: 0, profit: 0, deliverySpeed: 0, satisfaction: 16 }
  },
  {
    id: "overtime",
    title: "Authorize Overtime at Remaining Plants",
    desc: "Pay premium wages to run unaffected factories at higher capacity to cover the shortfall.",
    why: "Overtime ramps up output fast without new contracts, but labor costs rise and overworked lines can hurt quality over time.",
    effects: { cost: -8, inventory: 9, profit: -5, deliverySpeed: 7, satisfaction: 3 }
  },
  {
    id: "renegotiate_terms",
    title: "Renegotiate Payment Terms with Suppliers",
    desc: "Ask existing suppliers for extended payment terms to preserve cash during the disruption.",
    why: "Protecting cash flow during a crisis matters as much as protecting product flow — but pushing suppliers too hard can strain relationships.",
    effects: { cost: 10, inventory: -2, profit: 6, deliverySpeed: -2, satisfaction: -2 }
  },
  {
    id: "do_nothing",
    title: "Wait and Monitor the Situation",
    desc: "Hold off on major decisions for a few days to gather more information before committing resources.",
    why: "Sometimes patience avoids costly overreactions — but in fast-moving crises, waiting too long can let small problems compound.",
    effects: { cost: 2, inventory: -10, profit: -2, deliverySpeed: -10, satisfaction: -10 }
  },
];

/* Negotiation: 4 rounds, each with 3 choices affecting trust, price, leadTime */
const NEGOTIATION_ROUNDS = [
  {
    round: 1,
    title: "Opening Position",
    context: "You're on a call with your supplier's account director. They're aware you're in a tough spot and have opened with a firm pricing stance.",
    why: "How you open a negotiation sets the tone for the entire relationship — aggressive openers can win short-term price but cost long-term trust.",
    choices: [
      { id:"a", label:"Push hard for your original contract price", desc:"Insist on the price you agreed to before the crisis, citing your contract.", effects:{trust:-8, price:10, leadTime:-2} },
      { id:"b", label:"Propose a fair compromise", desc:"Offer to split the cost increase, acknowledging their constraints too.", effects:{trust:8, price:-4, leadTime:2} },
      { id:"c", label:"Accept their price to move fast", desc:"Agree quickly so you can secure supply without further delay.", effects:{trust:4, price:-14, leadTime:6} },
    ]
  },
  {
    round: 2,
    title: "Volume Commitment",
    context: "The supplier asks if you'll commit to a larger long-term order volume in exchange for priority treatment during the shortage.",
    why: "Volume commitments can unlock priority access, but they also reduce your future flexibility if conditions change again.",
    choices: [
      { id:"a", label:"Commit to a 12-month volume guarantee", desc:"Lock in a long-term deal for guaranteed priority capacity.", effects:{trust:10, price:-6, leadTime:8} },
      { id:"b", label:"Offer a smaller 3-month trial commitment", desc:"Test the relationship before committing long-term.", effects:{trust:4, price:-2, leadTime:3} },
      { id:"c", label:"Decline any volume commitment", desc:"Keep full flexibility to switch suppliers later if needed.", effects:{trust:-6, price:4, leadTime:-4} },
    ]
  },
  {
    round: 3,
    title: "Quality & Inspection Terms",
    context: "With production ramping fast, the supplier suggests relaxing some quality inspection steps to speed up shipments.",
    why: "Cutting inspection steps can speed delivery, but quality issues discovered later are often far more expensive than the time saved now.",
    choices: [
      { id:"a", label:"Agree to reduced inspections temporarily", desc:"Prioritize speed; resume full inspections once stable.", effects:{trust:2, price:2, leadTime:10} },
      { id:"b", label:"Keep full inspections, accept the delay", desc:"Protect product quality even if it costs you time.", effects:{trust:6, price:-2, leadTime:-6} },
      { id:"c", label:"Propose a risk-based spot-check system", desc:"A balanced middle ground that keeps some oversight without full slowdown.", effects:{trust:8, price:0, leadTime:2} },
    ]
  },
  {
    round: 4,
    title: "Final Terms",
    context: "It's time to finalize the agreement. The supplier wants to know how you'll structure payment given the urgency.",
    why: "Final terms determine how the relationship feels after the crisis ends — generous terms build goodwill for future negotiations.",
    choices: [
      { id:"a", label:"Offer faster payment for a small discount", desc:"Pay sooner than usual in exchange for a price break.", effects:{trust:6, price:-8, leadTime:0} },
      { id:"b", label:"Stick to standard 60-day payment terms", desc:"Keep your normal payment cycle unchanged.", effects:{trust:0, price:0, leadTime:0} },
      { id:"c", label:"Ask for extended 90-day terms", desc:"Protect your cash flow by delaying payment further.", effects:{trust:-5, price:5, leadTime:3} },
    ]
  },
];

/* CEO Boardroom: 5 questions, each scored 0-20 pts based on choice quality */
const CEO_QUESTIONS = [
  {
    q: "The board asks how you'll explain this crisis to investors on tomorrow's earnings call.",
    why: "How leaders frame a crisis publicly shapes investor confidence as much as the financial numbers themselves.",
    options: [
      { label:"Be transparent about the disruption and outline a clear recovery plan", score:20 },
      { label:"Downplay the issue to avoid spooking investors", score:4 },
      { label:"Blame external factors entirely and avoid discussing internal response", score:8 },
      { label:"Postpone the call until more information is available", score:10 },
    ]
  },
  {
    q: "Your COO recommends laying off temporary warehouse staff to cut costs immediately.",
    why: "Crisis cost-cutting can backfire if it removes the exact labor capacity you'll need for recovery.",
    options: [
      { label:"Reduce hours instead of full layoffs to preserve trained staff", score:18 },
      { label:"Approve the layoffs to protect margins immediately", score:6 },
      { label:"Keep all staff and absorb the cost short-term", score:14 },
      { label:"Delay the decision for two weeks to assess demand", score:12 },
    ]
  },
  {
    q: "A key customer threatens to cancel their contract unless you guarantee on-time delivery this month.",
    why: "Overpromising under pressure can damage trust even more than an honest, realistic commitment.",
    options: [
      { label:"Give an honest, realistic delivery estimate with regular updates", score:20 },
      { label:"Promise on-time delivery to keep the contract, regardless of risk", score:4 },
      { label:"Offer a partial shipment now and the rest later, clearly communicated", score:16 },
      { label:"Avoid responding until the situation is fully resolved", score:5 },
    ]
  },
  {
    q: "Your CFO suggests drawing on the company's emergency credit line to fund crisis response.",
    why: "Strong leaders balance short-term liquidity needs against long-term financial health.",
    options: [
      { label:"Use a modest, targeted draw tied to specific recovery actions", score:18 },
      { label:"Draw the maximum available immediately as a safety buffer", score:8 },
      { label:"Avoid using the credit line and cut spending elsewhere instead", score:10 },
      { label:"Delay the decision until the crisis fully plays out", score:6 },
    ]
  },
  {
    q: "After the immediate crisis stabilizes, the leadership team debates next steps.",
    why: "The best leaders treat a crisis as a lesson, not just an event to survive.",
    options: [
      { label:"Conduct a full post-crisis review and invest in resilience for next time", score:20 },
      { label:"Move on quickly without a formal review to save time", score:5 },
      { label:"Review only the financial impact, skip the operational lessons", score:10 },
      { label:"Assign blame to the department most affected", score:2 },
    ]
  },
];

/* AI Strategy investments: pick 2 of 5 */
const AI_INVESTMENTS = [
  {
    id:"forecasting",
    title:"Demand Forecasting AI",
    desc:"Uses historical and real-time data to predict demand shifts before they happen.",
    impact:"Reduces stockouts and overstock by anticipating demand swings earlier than manual planning.",
    effects:{ resilience:14, cost:6, satisfaction:6 }
  },
  {
    id:"inventory_opt",
    title:"Inventory Optimization AI",
    desc:"Continuously recalculates optimal stock levels across every warehouse.",
    impact:"Frees up working capital by trimming excess inventory while protecting service levels.",
    effects:{ resilience:10, cost:12, satisfaction:4 }
  },
  {
    id:"supplier_risk",
    title:"Supplier Risk Monitoring AI",
    desc:"Scans news, financial filings, and shipping data to flag at-risk suppliers early.",
    impact:"Gives you days or weeks of early warning before disruptions like the one you just faced.",
    effects:{ resilience:18, cost:-2, satisfaction:2 }
  },
  {
    id:"warehouse_vision",
    title:"Warehouse Vision AI",
    desc:"Computer vision systems that track inventory accuracy and automate quality checks.",
    impact:"Improves picking accuracy and reduces shipping errors that hurt customer satisfaction.",
    effects:{ resilience:6, cost:4, satisfaction:14 }
  },
  {
    id:"procurement_copilot",
    title:"Procurement Copilot",
    desc:"An AI assistant that drafts supplier negotiations and flags contract risks automatically.",
    impact:"Speeds up sourcing decisions and helps negotiators spot unfavorable terms before signing.",
    effects:{ resilience:10, cost:8, satisfaction:4 }
  },
];

/* ============================= SMALL COMPONENTS ============================= */

function Badge({children, kind="accent"}){
  return <span className={`badge badge-${kind}`}>{children}</span>;
}

function MetricBar({label, value, color}){
  const [display, setDisplay] = useState(0);
  useEffect(()=>{
    const t = setTimeout(()=> setDisplay(clamp(value)), 80);
    return ()=> clearTimeout(t);
  }, [value]);
  return (
    <div className="metric-row">
      <div className="metric-label">{label}</div>
      <div className="metric-track">
        <div className="metric-fill" style={{width:`${display}%`, background:color}}></div>
      </div>
      <div className="metric-value">{Math.round(value)}</div>
    </div>
  );
}

function WhyBox({children}){
  return <div className="why-box"><b>Why this matters:</b> {children}</div>;
}

function StepPill({step, total}){
  return <div className="step-pill">Step {step} of {total}</div>;
}

function NavFooter({onBack, onNext, nextLabel="Continue", nextDisabled=false, hideBack=false}){
  return (
    <div className="footer-nav">
      {!hideBack ? <button className="btn-ghost" onClick={onBack}>← Back</button> : <div></div>}
      <button className="btn" onClick={onNext} disabled={nextDisabled}>{nextLabel}</button>
    </div>
  );
}

/* ============================= SCREENS ============================= */

function Welcome({onStart}){
  return (
    <div className="card fade-in" style={{textAlign:"center", padding:"56px 32px"}}>
      <div style={{fontSize:46, marginBottom:10}}>🌐</div>
      <h1>Operation Lifeline</h1>
      <div className="subtitle">Supply Chain Crisis Lab</div>
      <p style={{maxWidth:620, margin:"0 auto 28px"}}>
        Step into the shoes of a supply chain executive. A random company, a real-world style crisis,
        and a series of high-stakes decisions are waiting. No experience required — every decision comes
        with plain-language context and a "why this matters" explanation so you'll learn as you play.
      </p>
      <button className="btn" onClick={onStart} style={{fontSize:16, padding:"16px 34px"}}>Start Simulation →</button>
      <p className="small" style={{marginTop:22}}>Takes about 8–10 minutes • Every playthrough is randomized</p>
    </div>
  );
}

function CompanyProfile({company, onNext}){
  return (
    <div className="fade-in">
      <StepPill step={1} total={6} />
      <h2 style={{marginTop:14}}>Your Company</h2>
      <p>Before the crisis hits, get familiar with the business you're now responsible for. These numbers will shape how much room you have to maneuver.</p>
      <div className="card" style={{marginBottom:18}}>
        <h3>{company.name}</h3>
        <Badge kind="accent">{company.industry}</Badge>
        <div className="grid grid-4" style={{marginTop:18}}>
          <div className="stat-card">
            <div className="stat-label">Annual Revenue</div>
            <div className="stat-value">${company.revenue}M</div>
          </div>
          <div className="stat-card">
            <div className="stat-label">Factories</div>
            <div className="stat-value">{company.factories}</div>
          </div>
          <div className="stat-card">
            <div className="stat-label">Warehouses</div>
            <div className="stat-value">{company.warehouses}</div>
          </div>
          <div className="stat-card">
            <div className="stat-label">Active Suppliers</div>
            <div className="stat-value">{company.suppliers}</div>
          </div>
          <div className="stat-card">
            <div className="stat-label">Inventory on Hand</div>
            <div className="stat-value">{company.inventoryDays} days</div>
          </div>
          <div className="stat-card">
            <div className="stat-label">Avg. Lead Time</div>
            <div className="stat-value">{company.leadTime} days</div>
          </div>
        </div>
        <div style={{marginTop:16}}>
          <div className="small" style={{marginBottom:6}}>Operating Countries</div>
          <div className="tag-list">
            {company.countries.map(c => <span className="chip" key={c}>{c}</span>)}
          </div>
        </div>
      </div>
      <WhyBox>
        Companies with more inventory days and diversified countries have more buffer to absorb shocks.
        Keep these numbers in mind — they hint at how vulnerable or resilient your starting position is.
      </WhyBox>
      <NavFooter onNext={onNext} nextLabel="See the Crisis →" hideBack={true} />
    </div>
  );
}

function CrisisIntro({crisis, onNext}){
  const urgencyKind = crisis.urgency === "Critical" ? "bad" : crisis.urgency === "High" ? "warn" : "accent";
  return (
    <div className="fade-in">
      <StepPill step={2} total={6} />
      <h2 style={{marginTop:14}}>Crisis Alert</h2>
      <div className="crisis-banner">
        <div style={{display:"flex", alignItems:"center", gap:12, marginBottom:10}}>
          <div style={{fontSize:32}}>{crisis.icon}</div>
          <div>
            <h3 style={{margin:0}}>{crisis.title}</h3>
            <Badge kind={urgencyKind}>{crisis.urgency} Urgency</Badge>
          </div>
        </div>
        <p style={{color:"var(--text-0)"}}>{crisis.desc}</p>
      </div>
      <div className="card">
        <h3>Business Impact</h3>
        <p>{crisis.impact}</p>
      </div>
      <WhyBox>
        Every crisis hits a different part of the supply chain — cost, speed, inventory, or trust.
        Pay attention to where this one is hitting hardest; it should guide which responses you choose next.
      </WhyBox>
      <NavFooter onNext={onNext} nextLabel="Enter the War Room →" hideBack={true} />
    </div>
  );
}

function WarRoom({actions, selected, onToggle, metrics, onNext}){
  const maxReached = selected.length >= 3;
  return (
    <div className="fade-in">
      <StepPill step={3} total={6} />
      <h2 style={{marginTop:14}}>War Room: Choose Your Response</h2>
      <p>You have six possible actions. Pick exactly <b>three</b> — the combination you choose will shape your company's cost, inventory, profit, delivery speed, and customer satisfaction.</p>
      <div className="grid grid-2">
        {actions.map(a => {
          const isSelected = selected.includes(a.id);
          const disabled = !isSelected && maxReached;
          return (
            <button
              key={a.id}
              className={`option ${isSelected ? "selected" : ""} ${disabled ? "disabled" : ""}`}
              onClick={()=> !disabled && onToggle(a.id)}
              disabled={disabled}
            >
              <div className="option-title">{isSelected ? "✓ " : ""}{a.title}</div>
              <p className="option-desc">{a.desc}</p>
            </button>
          );
        })}
      </div>
      <p className="small" style={{margin:"4px 0 18px"}}>{selected.length} / 3 selected</p>

      {selected.length > 0 && (
        <div className="card" style={{marginBottom:18}}>
          <h3>Why these matter</h3>
          {selected.map(id => {
            const a = actions.find(x=>x.id===id);
            return <p key={id} style={{marginBottom:8}}><b style={{color:"var(--text-0)"}}>{a.title}:</b> {a.why}</p>;
          })}
        </div>
      )}

      <div className="card">
        <h3>Live Business Impact</h3>
        <MetricBar label="Cost Pressure" value={metrics.cost} color="var(--bad)" />
        <MetricBar label="Inventory Health" value={metrics.inventory} color="var(--good)" />
        <MetricBar label="Profit Margin" value={metrics.profit} color="var(--accent)" />
        <MetricBar label="Delivery Speed" value={metrics.deliverySpeed} color="var(--accent-2)" />
        <MetricBar label="Customer Satisfaction" value={metrics.satisfaction} color="var(--warn)" />
      </div>

      <NavFooter onNext={onNext} nextLabel="Proceed to Negotiation →" nextDisabled={selected.length !== 3} hideBack={true} />
    </div>
  );
}

function Negotiation({roundIndex, negState, onChoose, onNext, onBackToRound}){
  const round = NEGOTIATION_ROUNDS[roundIndex];
  const chosen = negState.choices[roundIndex];
  return (
    <div className="fade-in">
      <StepPill step={4} total={6} />
      <h2 style={{marginTop:14}}>Supplier Negotiation — Round {round.round} of 4</h2>
      <div className="card" style={{marginBottom:18}}>
        <h3>{round.title}</h3>
        <p>{round.context}</p>
        <div className="grid grid-3" style={{marginTop:6}}>
          <div className="stat-card"><div className="stat-label">Trust</div><div className="stat-value">{negState.trust}</div></div>
          <div className="stat-card"><div className="stat-label">Price Index</div><div className="stat-value">{negState.price}</div></div>
          <div className="stat-card"><div className="stat-label">Lead Time Index</div><div className="stat-value">{negState.leadTime}</div></div>
        </div>
      </div>

      {round.choices.map(c => (
        <button
          key={c.id}
          className={`option ${chosen===c.id ? "selected" : ""}`}
          onClick={()=> onChoose(roundIndex, c.id)}
        >
          <div className="option-title">{c.label}</div>
          <p className="option-desc">{c.desc}</p>
        </button>
      ))}

      <WhyBox>{round.why}</WhyBox>

      <NavFooter
        onBack={()=> onBackToRound(roundIndex-1)}
        onNext={onNext}
        nextLabel={roundIndex === NEGOTIATION_ROUNDS.length - 1 ? "Finish Negotiation →" : "Next Round →"}
        nextDisabled={!chosen}
        hideBack={roundIndex===0}
      />
    </div>
  );
}

function NegotiationSummary({negState, onNext}){
  const score = clamp(Math.round((negState.trust*0.5) + ((100-negState.price)*0.25) + ((100-negState.leadTime)*0.25)));
  return (
    <div className="fade-in">
      <StepPill step={4} total={6} />
      <h2 style={{marginTop:14}}>Negotiation Complete</h2>
      <div className="card" style={{marginBottom:18, textAlign:"center"}}>
        <div className="score-ring" style={{background:`conic-gradient(var(--accent) ${score*3.6}deg, var(--bg-3) 0deg)`}}>
          <div className="inner">
            <div className="num">{score}</div>
            <div className="lbl">Negotiation Score</div>
          </div>
        </div>
        <div className="grid grid-3">
          <div className="stat-card"><div className="stat-label">Final Trust</div><div className="stat-value">{negState.trust}</div></div>
          <div className="stat-card"><div className="stat-label">Final Price Index</div><div className="stat-value">{negState.price}</div></div>
          <div className="stat-card"><div className="stat-label">Final Lead Time Index</div><div className="stat-value">{negState.leadTime}</div></div>
        </div>
      </div>
      <WhyBox>
        Higher trust usually pays off beyond this single deal — suppliers prioritize loyal, fair partners
        when the next shortage hits. Price and lead time matter today; trust matters for years.
      </WhyBox>
      <NavFooter onNext={onNext} nextLabel="Enter the Boardroom →" hideBack={true} />
    </div>
  );
}

function Boardroom({qIndex, answers, onAnswer, onNext, onBack}){
  const item = CEO_QUESTIONS[qIndex];
  const chosen = answers[qIndex];
  return (
    <div className="fade-in">
      <StepPill step={5} total={6} />
      <h2 style={{marginTop:14}}>CEO Boardroom — Question {qIndex+1} of {CEO_QUESTIONS.length}</h2>
      <div className="card" style={{marginBottom:18}}>
        <p style={{color:"var(--text-0)", fontSize:16}}>{item.q}</p>
      </div>
      {item.options.map((o, i) => (
        <button
          key={i}
          className={`option ${chosen===i ? "selected" : ""}`}
          onClick={()=> onAnswer(qIndex, i)}
        >
          <div className="option-title">{o.label}</div>
        </button>
      ))}
      <WhyBox>{item.why}</WhyBox>
      <NavFooter
        onBack={()=> onBack(qIndex-1)}
        onNext={onNext}
        nextLabel={qIndex === CEO_QUESTIONS.length - 1 ? "See Boardroom Results →" : "Next Question →"}
        nextDisabled={chosen===undefined}
        hideBack={qIndex===0}
      />
    </div>
  );
}

function BoardroomSummary({answers, onNext}){
  const total = answers.reduce((sum, ansIdx, i)=> sum + CEO_QUESTIONS[i].options[ansIdx].score, 0);
  const max = CEO_QUESTIONS.length * 20;
  const pct = Math.round((total/max)*100);
  return (
    <div className="fade-in">
      <StepPill step={5} total={6} />
      <h2 style={{marginTop:14}}>Boardroom Results</h2>
      <div className="card" style={{marginBottom:18, textAlign:"center"}}>
        <div className="score-ring" style={{background:`conic-gradient(var(--accent-2) ${pct*3.6}deg, var(--bg-3) 0deg)`}}>
          <div className="inner">
            <div className="num">{pct}</div>
            <div className="lbl">Leadership Score</div>
          </div>
        </div>
        <p>Your decisions reflected {pct >= 80 ? "strong, balanced executive judgment" : pct >= 55 ? "reasonable but inconsistent leadership instincts" : "a reactive leadership style under pressure"}.</p>
      </div>
      <WhyBox>
        Great crisis leadership isn't about having all the answers — it's about transparency, balanced
        risk-taking, and treating every disruption as a chance to build long-term resilience.
      </WhyBox>
      <NavFooter onNext={onNext} nextLabel="Plan Your AI Strategy →" hideBack={true} />
    </div>
  );
}

function AIStrategy({selected, onToggle, onNext}){
  const maxReached = selected.length >= 2;
  return (
    <div className="fade-in">
      <StepPill step={6} total={6} />
      <h2 style={{marginTop:14}}>AI Strategy: Future-Proof the Supply Chain</h2>
      <p>The crisis is stabilizing. Now it's time to invest in technology that reduces the odds — and impact — of the next one. Choose <b>two</b> AI investments.</p>
      <div className="grid grid-2">
        {AI_INVESTMENTS.map(inv => {
          const isSelected = selected.includes(inv.id);
          const disabled = !isSelected && maxReached;
          return (
            <button
              key={inv.id}
              className={`option ${isSelected ? "selected" : ""} ${disabled ? "disabled" : ""}`}
              onClick={()=> !disabled && onToggle(inv.id)}
              disabled={disabled}
            >
              <div className="option-title">{isSelected ? "✓ " : ""}{inv.title}</div>
              <p className="option-desc">{inv.desc}</p>
            </button>
          );
        })}
      </div>
      <p className="small" style={{margin:"4px 0 18px"}}>{selected.length} / 2 selected</p>
      {selected.length > 0 && (
        <div className="card" style={{marginBottom:18}}>
          <h3>Expected Impact</h3>
          {selected.map(id => {
            const inv = AI_INVESTMENTS.find(x=>x.id===id);
            return <p key={id} style={{marginBottom:8}}><b style={{color:"var(--text-0)"}}>{inv.title}:</b> {inv.impact}</p>;
          })}
        </div>
      )}
      <WhyBox>
        AI tools work best when they target your biggest weak point. Think back to what made this crisis
        so painful — forecasting, monitoring, inventory, or sourcing — and invest accordingly.
      </WhyBox>
      <NavFooter onNext={onNext} nextLabel="View Final Dashboard →" nextDisabled={selected.length !== 2} hideBack={true} />
    </div>
  );
}

function FinalDashboard({state, onReplay}){
  const { company, crisis, actions, warMetrics, negState, ceoAnswers, aiSelected } = state;

  // Leadership
  const leadershipRaw = ceoAnswers.reduce((sum, idx, i)=> sum + CEO_QUESTIONS[i].options[idx].score, 0);
  const leadership = Math.round((leadershipRaw / (CEO_QUESTIONS.length*20)) * 100);

  // Negotiation
  const negotiation = clamp(Math.round((negState.trust*0.5) + ((100-negState.price)*0.25) + ((100-negState.leadTime)*0.25)));

  // Resilience: based on AI investments + good crisis-action choices
  const aiResilience = aiSelected.reduce((sum, id)=> sum + AI_INVESTMENTS.find(a=>a.id===id).effects.resilience, 0);
  const resilience = clamp(Math.round(40 + aiResilience + (warMetrics.inventory - 50)*0.3));

  // Cost control: based on war room cost metric and negotiation price
  const costControl = clamp(Math.round((warMetrics.cost*0.5) + ((100-negState.price)*0.3) + (warMetrics.profit*0.2)));

  // Risk Management: combination of inventory health, delivery speed, negotiation trust
  const riskManagement = clamp(Math.round((warMetrics.inventory*0.4) + (warMetrics.deliverySpeed*0.3) + (negState.trust*0.3)));

  // Customer satisfaction: war room satisfaction + AI satisfaction boosts
  const aiSat = aiSelected.reduce((sum, id)=> sum + AI_INVESTMENTS.find(a=>a.id===id).effects.satisfaction, 0);
  const customerSatisfaction = clamp(Math.round(warMetrics.satisfaction*0.7 + aiSat));

  const overall = clamp(Math.round(
    leadership*0.2 + negotiation*0.15 + resilience*0.2 + costControl*0.15 + riskManagement*0.15 + customerSatisfaction*0.15
  ));

  const overallColor = overall >= 75 ? "var(--good)" : overall >= 50 ? "var(--warn)" : "var(--bad)";

  // Biggest mistake / best decision — pick lowest/highest scoring action effect heuristically
  const chosenActions = actions.filter(a => state.selectedActions.includes(a.id));
  const worstAction = chosenActions.reduce((worst, a)=>{
    const sum = a.effects.cost + a.effects.inventory + a.effects.profit + a.effects.deliverySpeed + a.effects.satisfaction;
    const worstSum = worst ? (worst.effects.cost+worst.effects.inventory+worst.effects.profit+worst.effects.deliverySpeed+worst.effects.satisfaction) : Infinity;
    return sum < worstSum ? a : worst;
  }, null);
  const bestAction = chosenActions.reduce((best, a)=>{
    const sum = a.effects.cost + a.effects.inventory + a.effects.profit + a.effects.deliverySpeed + a.effects.satisfaction;
    const bestSum = best ? (best.effects.cost+best.effects.inventory+best.effects.profit+best.effects.deliverySpeed+best.effects.satisfaction) : -Infinity;
    return sum > bestSum ? a : best;
  }, null);

  let feedbackTone = overall >= 75 ? "good" : overall >= 50 ? "" : "bad";
  let feedbackText = overall >= 75
    ? "You navigated this crisis with strong judgment — balancing cost, speed, and relationships well. This is the kind of decision-making that builds long-term resilience."
    : overall >= 50
    ? "You made solid progress through a tough situation, though a few tradeoffs cost you more than necessary. With sharper prioritization, you could turn good outcomes into great ones."
    : "This crisis exposed some real gaps in your response — costly choices and missed opportunities to build trust added up. The good news: every mistake here is a lesson you can apply next time.";

  const recommendation = resilience < 55
    ? "Prioritize supplier risk monitoring and diversification — your biggest vulnerability was reacting after the disruption hit instead of seeing it coming."
    : costControl < 55
    ? "Tighten cost discipline during crises by leaning more on negotiation and reallocation before reaching for expensive options like air freight."
    : "Keep investing in early-warning systems and transparent communication — they were your strongest assets this round.";

  return (
    <div className="fade-in">
      <h2 style={{marginTop:4}}>Final Executive Dashboard</h2>
      <p>Here's how {company.name} performed managing the {crisis.title.toLowerCase()}.</p>

      <div className="card" style={{textAlign:"center", marginBottom:20}}>
        <div className="score-ring" style={{background:`conic-gradient(${overallColor} ${overall*3.6}deg, var(--bg-3) 0deg)`}}>
          <div className="inner">
            <div className="num">{overall}</div>
            <div className="lbl">Overall Score</div>
          </div>
        </div>
        <Badge kind={overall>=75?"good":overall>=50?"warn":"bad"}>
          {overall>=75 ? "Crisis Mastered" : overall>=50 ? "Stabilized, With Gaps" : "Survived, Barely"}
        </Badge>
      </div>

      <div className="card" style={{marginBottom:20}}>
        <h3>Performance Breakdown</h3>
        <MetricBar label="Leadership" value={leadership} color="var(--accent-2)" />
        <MetricBar label="Negotiation" value={negotiation} color="var(--accent)" />
        <MetricBar label="Resilience" value={resilience} color="var(--good)" />
        <MetricBar label="Cost Control" value={costControl} color="var(--warn)" />
        <MetricBar label="Risk Management" value={riskManagement} color="#36b6d3" />
        <MetricBar label="Customer Satisfaction" value={customerSatisfaction} color="#f8a572" />
      </div>

      <div className="card" style={{marginBottom:20}}>
        <h3>Personalized Feedback</h3>
        <div className={`feedback-block ${feedbackTone}`}>{feedbackText}</div>
        {bestAction && (
          <div className="feedback-block good">
            <b>Best Decision:</b> Choosing "{bestAction.title}" gave you strong returns across multiple metrics with manageable downside.
          </div>
        )}
        {worstAction && (
          <div className="feedback-block bad">
            <b>Biggest Mistake:</b> "{worstAction.title}" came with tradeoffs that outweighed its benefits in this scenario.
          </div>
        )}
        <div className="feedback-block">
          <b>Expert Recommendation:</b> {recommendation}
        </div>
      </div>

      <div className="card" style={{marginBottom:24}}>
        <h3>Lessons Learned</h3>
        <p>• Crises rarely have a perfect answer — the goal is balancing speed, cost, and trust rather than maximizing any single metric.</p>
        <p>• Transparent communication, both with customers and suppliers, consistently pays off more than it costs.</p>
        <p>• Investing in early-warning technology before a crisis is almost always cheaper than reacting after one hits.</p>
      </div>

      <div style={{textAlign:"center"}}>
        <button className="btn" onClick={onReplay} style={{fontSize:15, padding:"14px 30px"}}>↻ Replay With a New Scenario</button>
      </div>
    </div>
  );
}

/* ============================= APP ROOT ============================= */

const SCREENS = {
  WELCOME: "welcome",
  COMPANY: "company",
  CRISIS: "crisis",
  WARROOM: "warroom",
  NEGOTIATION: "negotiation",
  NEG_SUMMARY: "neg_summary",
  BOARDROOM: "boardroom",
  BOARD_SUMMARY: "board_summary",
  AI: "ai",
  DASHBOARD: "dashboard",
};

function freshState(){
  const company = generateCompany();
  const crisis = pick(CRISES);
  const actions = pickN(ACTION_POOL, 6);
  return {
    company,
    crisis,
    actions,
    selectedActions: [],
    warMetrics: { cost:50, inventory:50, profit:50, deliverySpeed:50, satisfaction:50 },
    negRoundIndex: 0,
    negState: { trust:50, price:50, leadTime:50, choices:{} },
    ceoIndex: 0,
    ceoAnswers: [],
    aiSelected: [],
  };
}

function App(){
  const [screen, setScreen] = useState(SCREENS.WELCOME);
  const [state, setState] = useState(freshState());

  function restart(){
    setState(freshState());
    setScreen(SCREENS.COMPANY);
  }

  /* ---- War room logic ---- */
  function toggleAction(id){
    setState(prev => {
      let selected = prev.selectedActions.includes(id)
        ? prev.selectedActions.filter(x=>x!==id)
        : [...prev.selectedActions, id];
      if(selected.length > 3) return prev;

      const base = { cost:50, inventory:50, profit:50, deliverySpeed:50, satisfaction:50 };
      selected.forEach(actId => {
        const a = prev.actions.find(x=>x.id===actId);
        base.cost += a.effects.cost;
        base.inventory += a.effects.inventory;
        base.profit += a.effects.profit;
        base.deliverySpeed += a.effects.deliverySpeed;
        base.satisfaction += a.effects.satisfaction;
      });
      Object.keys(base).forEach(k => base[k] = clamp(base[k]));

      return { ...prev, selectedActions: selected, warMetrics: base };
    });
  }

  /* ---- Negotiation logic ---- */
  function chooseNeg(roundIdx, choiceId){
    setState(prev => {
      const round = NEGOTIATION_ROUNDS[roundIdx];
      const choice = round.choices.find(c=>c.id===choiceId);
      const prevChoiceId = prev.negState.choices[roundIdx];

      // recompute from scratch each time to allow changing answers
      const newChoices = { ...prev.negState.choices, [roundIdx]: choiceId };
      const base = { trust:50, price:50, leadTime:50 };
      NEGOTIATION_ROUNDS.forEach((r, i) => {
        const cid = newChoices[i];
        if(cid){
          const c = r.choices.find(x=>x.id===cid);
          base.trust += c.effects.trust;
          base.price += c.effects.price;
          base.leadTime += c.effects.leadTime;
        }
      });
      Object.keys(base).forEach(k => base[k] = clamp(base[k]));

      return { ...prev, negState: { ...base, choices: newChoices } };
    });
  }

  function negNext(){
    setState(prev => {
      if(prev.negRoundIndex < NEGOTIATION_ROUNDS.length - 1){
        return { ...prev, negRoundIndex: prev.negRoundIndex + 1 };
      }
      return prev;
    });
    if(state.negRoundIndex >= NEGOTIATION_ROUNDS.length - 1){
      setScreen(SCREENS.NEG_SUMMARY);
    }
  }

  function negBack(idx){
    if(idx < 0) return;
    setState(prev => ({ ...prev, negRoundIndex: idx }));
  }

  /* ---- Boardroom logic ---- */
  function answerCeo(qIdx, optIdx){
    setState(prev => {
      const newAnswers = [...prev.ceoAnswers];
      newAnswers[qIdx] = optIdx;
      return { ...prev, ceoAnswers: newAnswers };
    });
  }

  function ceoNext(){
    if(state.ceoIndex < CEO_QUESTIONS.length - 1){
      setState(prev => ({ ...prev, ceoIndex: prev.ceoIndex + 1 }));
    } else {
      setScreen(SCREENS.BOARD_SUMMARY);
    }
  }

  function ceoBack(idx){
    if(idx < 0) return;
    setState(prev => ({ ...prev, ceoIndex: idx }));
  }

  /* ---- AI strategy logic ---- */
  function toggleAI(id){
    setState(prev => {
      let selected = prev.aiSelected.includes(id)
        ? prev.aiSelected.filter(x=>x!==id)
        : [...prev.aiSelected, id];
      if(selected.length > 2) return prev;
      return { ...prev, aiSelected: selected };
    });
  }

  return (
    <div className="app">
      <div className="topbar">
        <div className="brand"><span className="dot"></span> Operation Lifeline</div>
        {screen !== SCREENS.WELCOME && screen !== SCREENS.DASHBOARD && (
          <div className="small">{state.company.name} • {state.crisis.title}</div>
        )}
      </div>

      {screen === SCREENS.WELCOME && (
        <Welcome onStart={restart} />
      )}

      {screen === SCREENS.COMPANY && (
        <CompanyProfile company={state.company} onNext={()=> setScreen(SCREENS.CRISIS)} />
      )}

      {screen === SCREENS.CRISIS && (
        <CrisisIntro crisis={state.crisis} onNext={()=> setScreen(SCREENS.WARROOM)} />
      )}

      {screen === SCREENS.WARROOM && (
        <WarRoom
          actions={state.actions}
          selected={state.selectedActions}
          onToggle={toggleAction}
          metrics={state.warMetrics}
          onNext={()=> setScreen(SCREENS.NEGOTIATION)}
        />
      )}

      {screen === SCREENS.NEGOTIATION && (
        <Negotiation
          roundIndex={state.negRoundIndex}
          negState={state.negState}
          onChoose={chooseNeg}
          onNext={negNext}
          onBackToRound={negBack}
        />
      )}

      {screen === SCREENS.NEG_SUMMARY && (
        <NegotiationSummary negState={state.negState} onNext={()=> setScreen(SCREENS.BOARDROOM)} />
      )}

      {screen === SCREENS.BOARDROOM && (
        <Boardroom
          qIndex={state.ceoIndex}
          answers={state.ceoAnswers}
          onAnswer={answerCeo}
          onNext={ceoNext}
          onBack={ceoBack}
        />
      )}

      {screen === SCREENS.BOARD_SUMMARY && (
        <BoardroomSummary answers={state.ceoAnswers} onNext={()=> setScreen(SCREENS.AI)} />
      )}

      {screen === SCREENS.AI && (
        <AIStrategy
          selected={state.aiSelected}
          onToggle={toggleAI}
          onNext={()=> setScreen(SCREENS.DASHBOARD)}
        />
      )}

      {screen === SCREENS.DASHBOARD && (
        <FinalDashboard state={state} onReplay={restart} />
      )}
    </div>
  );
}

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(<App />);
</script>
</body>
</html>
