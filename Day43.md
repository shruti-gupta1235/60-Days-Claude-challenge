<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>AI Workflow Architect &mdash; Design System Creation</title>
<style>
  :root{
    --void:#15181D; --void-2:#1C2028; --void-3:#242A34;
    --canvas:#FAFAF7; --canvas-2:#FFFFFF; --canvas-3:#EFEBE1;
    --indigo:#5B5FEF; --indigo-dim:#4448C4;
    --amber:#E8A33D; --amber-dim:#BC7F26;
    --mint:#3FCFA0; --coral:#EF6461;
    --line-dark:#2E3542; --line-light:#DCD5C4;
    --font-display:'Space Grotesk','Avenir Next','Century Gothic',system-ui,-apple-system,sans-serif;
    --font-body:'Inter','Segoe UI',system-ui,-apple-system,sans-serif;
    --font-mono:'IBM Plex Mono','SFMono-Regular',Consolas,'Liberation Mono',monospace;
    --radius:12px;
  }
  html.light{ --bg:var(--canvas); --surface:var(--canvas-2); --surface-2:var(--canvas-3);
    --fg:#1B1F27; --fg-dim:#6B7280; --border:var(--line-light); --shadow:0 6px 20px rgba(30,25,10,0.07); }
  html.dark{ --bg:var(--void); --surface:var(--void-2); --surface-2:var(--void-3);
    --fg:#EDEEF2; --fg-dim:#9198A8; --border:var(--line-dark); --shadow:0 10px 30px rgba(0,0,0,0.35); }
  *{box-sizing:border-box;}
  html{scroll-behavior:smooth;}
  body{margin:0; background:var(--bg); color:var(--fg); font-family:var(--font-body); transition:background .3s,color .3s;
    background-image:radial-gradient(var(--border) 1px, transparent 1px); background-size:26px 26px; -webkit-font-smoothing:antialiased;}
  html.light body{background-image:radial-gradient(rgba(0,0,0,0.045) 1px, transparent 1px);}
  html.dark body{background-image:radial-gradient(rgba(255,255,255,0.035) 1px, transparent 1px);}
  h1,h2,h3,.display{font-family:var(--font-display); letter-spacing:-0.01em;}
  .mono{font-family:var(--font-mono);}
  button{font-family:var(--font-body); cursor:pointer;}
  ::selection{background:var(--indigo); color:#fff;}
  a{color:inherit;}

  .topbar{position:sticky; top:0; z-index:60; background:var(--bg); border-bottom:1px solid var(--border); backdrop-filter:blur(8px);}
  .topbar-inner{max-width:1280px; margin:0 auto; padding:14px 24px; display:flex; align-items:center; justify-content:space-between;}
  .brand{display:flex; align-items:center; gap:11px;}
  .brand .mark{width:34px; height:34px; border-radius:8px; background:linear-gradient(135deg,var(--indigo),var(--mint)); display:flex; align-items:center; justify-content:center; font-family:var(--font-display); font-weight:700; color:#0B0D12; font-size:14px;}
  .brand h1{font-size:17px; margin:0; font-weight:700;}
  .brand .sub{font-size:10.5px; color:var(--fg-dim); text-transform:uppercase; letter-spacing:.13em;}
  .top-actions{display:flex; gap:8px;}
  .icon-btn{background:var(--surface); border:1px solid var(--border); color:var(--fg); border-radius:9px; width:36px; height:36px; display:flex; align-items:center; justify-content:center; font-size:15px; transition:transform .15s, border-color .15s;}
  .icon-btn:hover{transform:translateY(-1px); border-color:var(--indigo);}

  .wrap{max-width:1280px; margin:0 auto; padding:0 24px 80px;}

  /* ---------- Hero ---------- */
  .hero{padding:44px 0 30px; border-bottom:1px solid var(--border); margin-bottom:0;}
  .hero .kicker{font-size:11px; text-transform:uppercase; letter-spacing:.18em; color:var(--indigo); font-weight:700; display:flex; align-items:center; gap:8px;}
  .hero .kicker .dot{width:6px;height:6px;border-radius:50%;background:var(--mint);}
  .hero h2{font-size:34px; margin:10px 0 8px; font-weight:700; max-width:680px;}
  .hero p{color:var(--fg-dim); font-size:14.5px; max-width:620px; line-height:1.6; margin:0 0 20px;}
  .hero-meta{display:flex; gap:22px; flex-wrap:wrap;}
  .hero-meta .item{font-size:12px; color:var(--fg-dim);}
  .hero-meta .item b{color:var(--fg); font-family:var(--font-mono); display:block; font-size:17px; margin-bottom:1px;}

  /* ---------- Stage map (signature diagram) ---------- */
  .stagemap-wrap{position:sticky; top:65px; z-index:50; background:var(--bg); border-bottom:1px solid var(--border); padding:16px 0;}
  .stagemap{display:flex; align-items:flex-start; gap:0; overflow-x:auto; padding:4px 2px 6px; scrollbar-width:thin;}
  .stagemap::-webkit-scrollbar{height:4px;} .stagemap::-webkit-scrollbar-thumb{background:var(--border); border-radius:4px;}
  .stagenode{flex:0 0 auto; display:flex; align-items:center;}
  .stagenode .connector{width:36px; height:2px; background:var(--border); margin-top:19px; position:relative;}
  .stagenode .connector.fill::after{content:''; position:absolute; inset:0; background:var(--mint);}
  .stagenode:first-child .connector{display:none;}
  .stage-btn{background:none; border:none; display:flex; flex-direction:column; align-items:center; gap:6px; padding:0 10px; min-width:108px; text-align:center;}
  .stage-btn .ring{width:38px; height:38px; border-radius:50%; border:2px solid var(--border); display:flex; align-items:center; justify-content:center; font-family:var(--font-mono); font-weight:700; font-size:13px; color:var(--fg-dim); background:var(--surface); transition:all .2s ease;}
  .stage-btn.done .ring{border-color:var(--mint); background:var(--mint); color:#0B0D12;}
  .stage-btn.active .ring{border-color:var(--indigo); box-shadow:0 0 0 4px rgba(91,95,239,0.15);}
  .stage-btn .lbl{font-size:11px; font-weight:600; color:var(--fg-dim); max-width:100px; line-height:1.3;}
  .stage-btn.active .lbl, .stage-btn.done .lbl{color:var(--fg);}

  /* ---------- Sections ---------- */
  section.stage{padding:48px 0; border-bottom:1px solid var(--border); scroll-margin-top:190px;}
  .stage-head{display:flex; align-items:flex-start; justify-content:space-between; gap:20px; flex-wrap:wrap; margin-bottom:22px;}
  .stage-head-left{display:flex; gap:16px; align-items:flex-start;}
  .stage-num{font-family:var(--font-display); font-size:38px; font-weight:700; color:var(--surface-2); -webkit-text-stroke:1.5px var(--indigo); color:transparent; line-height:1;}
  .stage-head h3{font-size:24px; margin:0 0 4px; font-weight:700;}
  .stage-head .stage-desc{color:var(--fg-dim); font-size:13.5px; max-width:560px; line-height:1.55;}
  .stage-badges{display:flex; gap:8px; flex-wrap:wrap; align-items:center;}
  .badge{font-size:11px; padding:5px 11px; border-radius:20px; border:1px solid var(--border); color:var(--fg-dim); font-weight:600; display:flex; align-items:center; gap:5px;}
  .badge.time{color:var(--amber); border-color:var(--amber);}
  .mark-done{display:flex; align-items:center; gap:7px; font-size:12px; font-weight:700; padding:7px 13px; border-radius:20px; border:1px solid var(--border); background:var(--surface);}
  .mark-done.on{background:var(--mint); border-color:var(--mint); color:#0B0D12;}

  .stage-grid{display:grid; grid-template-columns:1.1fr 1fr; gap:16px;}
  @media(max-width:960px){.stage-grid{grid-template-columns:1fr;}}
  .panel{background:var(--surface); border:1px solid var(--border); border-radius:var(--radius); padding:18px 20px; box-shadow:var(--shadow);}
  .panel h4{font-size:11.5px; text-transform:uppercase; letter-spacing:.09em; color:var(--indigo); margin:0 0 12px; font-weight:700; display:flex; align-items:center; gap:7px;}
  .panel h4 .swatch{width:8px; height:8px; border-radius:2px; background:var(--indigo);}
  .panel ul{margin:0; padding-left:18px; font-size:13px; line-height:1.65;}
  .panel ul li{margin-bottom:6px;}
  .panel.full{grid-column:1/-1;}

  .tool-row{display:flex; justify-content:space-between; gap:10px; padding:11px 0; border-bottom:1px solid var(--border);}
  .tool-row:last-child{border-bottom:none;}
  .tool-row .tname{font-weight:700; font-size:13.5px;}
  .tool-row .twhy{font-size:12px; color:var(--fg-dim); margin-top:3px; line-height:1.5;}
  .alt-chip-row{display:flex; flex-wrap:wrap; gap:7px; margin-top:8px;}
  .alt-chip{font-size:11.5px; padding:5px 11px; background:var(--surface-2); border-radius:16px; border:1px solid var(--border); color:var(--fg-dim);}

  .prompt-block{background:var(--surface-2); border:1px solid var(--border); border-radius:9px; padding:12px 14px; font-family:var(--font-mono); font-size:12px; margin-bottom:9px; line-height:1.5; position:relative;}
  .prompt-block::before{content:'PROMPT'; position:absolute; top:-8px; left:10px; background:var(--indigo); color:#fff; font-family:var(--font-body); font-size:9px; font-weight:700; letter-spacing:.08em; padding:2px 7px; border-radius:5px;}

  .mistake-row{display:flex; gap:9px; font-size:13px; margin-bottom:9px; line-height:1.55;}
  .mistake-row .x{color:var(--coral); font-weight:700; flex:0 0 auto;}
  .practice-row{display:flex; gap:9px; font-size:13px; margin-bottom:9px; line-height:1.55;}
  .practice-row .ck{color:var(--mint); font-weight:700; flex:0 0 auto;}

  /* ---------- Summary ---------- */
  .summary-hero{padding:50px 0 60px;}
  .summary-grid{display:grid; grid-template-columns:repeat(4,1fr); gap:14px; margin:24px 0 28px;}
  @media(max-width:800px){.summary-grid{grid-template-columns:repeat(2,1fr);}}
  .sum-card{background:var(--surface); border:1px solid var(--border); border-radius:var(--radius); padding:18px; box-shadow:var(--shadow);}
  .sum-card .num{font-family:var(--font-mono); font-size:26px; font-weight:700; color:var(--indigo);}
  .sum-card .lbl{font-size:11px; color:var(--fg-dim); text-transform:uppercase; letter-spacing:.07em; margin-top:2px;}
  .progress-track{height:8px; background:var(--surface-2); border-radius:6px; overflow:hidden; margin-top:10px;}
  .progress-fill{height:100%; background:linear-gradient(90deg,var(--indigo),var(--mint)); border-radius:6px; transition:width .4s ease;}

  footer{text-align:center; padding:30px 20px; color:var(--fg-dim); font-size:12px; border-top:1px solid var(--border);}

  @media print{
    .topbar,.stagemap-wrap,.icon-btn,.mark-done,body::before{display:none !important;}
    body{background:#fff; color:#111;}
    section.stage{border-bottom:1px solid #ccc; page-break-inside:avoid;}
  }
</style>
</head>
<body>

<div class="topbar">
  <div class="topbar-inner">
    <div class="brand">
      <div class="mark">AI</div>
      <div>
        <h1>AI Workflow Architect</h1>
        <div class="sub">Design System Creation &middot; Solo Designer</div>
      </div>
    </div>
    <div class="top-actions">
      <button class="icon-btn" id="printBtn" title="Print workflow">&#128424;</button>
      <button class="icon-btn" id="themeBtn" title="Toggle theme">&#9789;</button>
    </div>
  </div>
</div>

<div class="wrap">
  <div class="hero">
    <div class="kicker"><span class="dot"></span>UI/UX Design &rarr; Design System Creation &rarr; Solo, From Scratch</div>
    <h2>An end-to-end workflow for building a design system alone, from first audit to a maintained v1.</h2>
    <p>Six stages, mapped start to finish. Each one lists exactly what to do, which AI tools actually help at that step and why, prompts to run, and the mistakes that cost solo designers the most time.</p>
    <div class="hero-meta">
      <div class="item"><b id="metaStages">6</b>Stages</div>
      <div class="item"><b id="metaTime">~10&ndash;14 wks</b>Estimated total time</div>
      <div class="item"><b id="metaTools">17</b>AI tools referenced</div>
    </div>
  </div>
</div>

<div class="stagemap-wrap">
  <div class="wrap">
    <div class="stagemap" id="stagemap"></div>
  </div>
</div>

<div class="wrap" id="stagesHolder"></div>

<div class="wrap">
  <section class="stage summary-hero" id="summary" style="border-bottom:none;">
    <div class="hero">
      <div class="kicker" style="border:none;"><span class="dot" style="background:var(--amber);"></span>Workflow Summary</div>
      <h2 style="font-size:28px;">Where you stand, and what this workflow adds up to.</h2>
      <p>A recap of the full path &mdash; from the first UI audit to a governed, versioned design system &mdash; and how far you've marked yourself through it.</p>
    </div>
    <div class="summary-grid">
      <div class="sum-card"><div class="num" id="sumDone">0/6</div><div class="lbl">Stages marked complete</div>
        <div class="progress-track"><div class="progress-fill" id="sumProgressBar" style="width:0%;"></div></div>
      </div>
      <div class="sum-card"><div class="num">~10&ndash;14 wks</div><div class="lbl">Total solo timeline</div></div>
      <div class="sum-card"><div class="num">17</div><div class="lbl">AI tools across the workflow</div></div>
      <div class="sum-card"><div class="num">6</div><div class="lbl">Prompt examples ready to reuse</div></div>
    </div>
    <div class="panel full">
      <h4><span class="swatch"></span>Key takeaways</h4>
      <ul>
        <li>Audit first, always &mdash; a system built without a real inventory of existing UI drifts back into inconsistency within a quarter.</li>
        <li>Tokens are the foundation everything else depends on: get primitives and semantic naming right before drawing a single component.</li>
        <li>AI tools are fastest as first-draft generators (palettes, variant states, boilerplate code, doc copy) &mdash; treat every output as a draft you refine, not a final answer.</li>
        <li>A design system isn't "done" at v1 &mdash; governance and adoption tracking is what keeps it alive past the first release.</li>
        <li>As a solo designer, your biggest lever is sequencing: skipping ahead to components before foundations is the single most common rework cause.</li>
      </ul>
    </div>
  </section>
</div>

<footer>AI Workflow Architect &middot; Progress saves locally in your browser &middot; Built as a practical starting workflow, adapt it to your product.</footer>

<script>
(function(){
  "use strict";
  const LS = {
    get(k,f){ try{ const v=localStorage.getItem(k); return v?JSON.parse(v):f; }catch(e){ return f; } },
    set(k,v){ try{ localStorage.setItem(k, JSON.stringify(v)); }catch(e){} }
  };
  const root = document.documentElement;
  function applyTheme(t){ root.classList.remove('light','dark'); root.classList.add(t); LS.set('awa_theme', t); }
  applyTheme(LS.get('awa_theme','dark'));
  document.getElementById('themeBtn').addEventListener('click', ()=> applyTheme(root.classList.contains('dark')?'light':'dark'));
  document.getElementById('printBtn').addEventListener('click', ()=> window.print());

  const STAGES = [
    {
      id:'audit', num:1, title:'Audit & Discovery', time:'3&ndash;5 days',
      desc:'Inventory every UI inconsistency in the current product and define exactly what the system needs to cover before designing anything.',
      objectives:['Understand where the current UI is inconsistent and why','Define which platforms and products the system must cover','Build an initial component inventory','Collect brand assets and any existing guidelines'],
      tasks:['Screenshot and catalog every screen of the live product','List every unique button, input, and card style currently in use','Benchmark 2&ndash;3 design systems in a similar space (structure only, not copying content)','Draft a scope doc: platforms, component types, timeline'],
      tools:[
        {name:'Claude / ChatGPT', why:'Synthesizes messy audit notes and screenshots into structured pattern lists far faster than manual tagging.'},
        {name:'Figma (native inspect + AI plugins)', why:'Lets you pull real color/spacing values straight from existing files instead of eyeballing them.'},
        {name:'Miro AI', why:'Clusters screenshots and sticky notes visually, useful for spotting repeated inconsistencies at a glance.'}
      ],
      alternatives:['Notion AI','UX Pilot','Whimsical AI'],
      prompts:['List every inconsistent color, spacing, and font-size value visible across these UI screenshots, grouped by type.','Given this list of existing UI components, group them into a proposed component taxonomy for a design system.'],
      practices:['Audit the real, shipped product &mdash; not old mockups or Dribbble shots','Timebox the audit to under a week so it doesn\'t stall the whole project','Write down every inconsistency even if it seems minor, then prioritize later'],
      mistakes:['Skipping the audit and jumping straight to designing components','Only auditing "happy path" screens and missing error/empty states','Auditing without deciding scope, so the list never ends'],
      outputs:['Audit report with categorized inconsistencies','Initial component inventory list','One-page scope document'],
      tips:'Batch every screenshot into one AI conversation and ask for a single consolidated pattern list rather than reviewing screen by screen &mdash; it cuts audit time roughly in half.'
    },
    {
      id:'foundation', num:2, title:'Foundation Definition', time:'4&ndash;6 days',
      desc:'Define the token layer &mdash; color, type, spacing, radius, elevation &mdash; that every component will be built from.',
      objectives:['Establish primitive and semantic token layers','Define an accessible color system for light and dark themes','Set a typographic scale and spacing/grid system','Get tokens into Figma Variables as the single source of truth'],
      tasks:['Generate primitive color ramps (e.g. blue-100 through blue-900)','Map primitives to semantic tokens (e.g. "action-primary" &rarr; blue-600)','Define type scale (sizes, weights, line-heights)','Define spacing scale on a consistent base unit (4pt or 8pt)','Run contrast checks on every text/background pairing'],
      tools:[
        {name:'Figma Variables', why:'Native token storage that every component and future dev handoff step depends on &mdash; the real source of truth.'},
        {name:'Stark (AI contrast checking)', why:'Flags failing WCAG contrast pairs automatically instead of manual checking token by token.'},
        {name:'Claude / ChatGPT', why:'Proposes a consistent, scalable naming taxonomy for primitive vs. semantic tokens based on your product context.'}
      ],
      alternatives:['Tokens Studio for Figma','Adobe Leonardo (color scale generator)','Coolors AI'],
      prompts:['Propose a semantic token naming convention for a design system with primitive color, spacing, and typography layers.','Generate a 9-step accessible color ramp from this base hex value, optimized for both light and dark backgrounds.'],
      practices:['Always separate primitive tokens from semantic tokens &mdash; never reference a primitive directly in a component','Document what each semantic token is "for," not just its value','Test every token combination for WCAG AA contrast before moving on'],
      mistakes:['Creating one-off color values outside the token set the moment something "needs" a tweak','Skipping the semantic layer and wiring components straight to primitives','Inconsistent naming across color, spacing, and type tokens'],
      outputs:['Complete token set in Figma Variables','Foundations documentation page (color, type, spacing, radius)','Light/dark theme mapping'],
      tips:'Use an AI tool to generate the full numeric type/spacing scale from one base value and one ratio &mdash; it keeps the whole scale mathematically consistent instead of hand-picked.'
    },
    {
      id:'components', num:3, title:'Component Library Design', time:'2&ndash;3 weeks',
      desc:'Design the reusable component set &mdash; every variant and state &mdash; entirely on top of the token foundation.',
      objectives:['Design core components with full variant and state coverage','Keep every component built from foundation tokens, with no hardcoded values','Establish consistent auto-layout and responsive behavior','Prioritize components by real product usage frequency'],
      tasks:['Rank components by how often they appear in the audited product','Design each component\'s states: default, hover, focus, disabled, error, loading','Build variants with Figma component properties, not duplicated frames','Stress-test components with real (not lorem ipsum) content lengths'],
      tools:[
        {name:'Galileo AI', why:'Generates a fast first-draft UI for a component from a text prompt, useful for breaking creative block on structure.'},
        {name:'Figma AI (native)', why:'Works inside the same file as your tokens, so generated variants can be re-wired to real token values immediately.'},
        {name:'v0.dev', why:'Produces a working coded version alongside the UI, useful for validating a component is buildable before it\'s finalized in Figma.'}
      ],
      alternatives:['Uizard','Framer AI','UX Pilot'],
      prompts:['Generate a button component with primary, secondary, and ghost variants across default, hover, focus, disabled, and loading states.','List every state a form input component needs to support, including validation and helper text.'],
      practices:['Design every state up front, even ones that feel unlikely (empty, loading, error)','Use auto-layout and component properties so variants scale instead of multiply','Keep each component\'s prop/variant surface as small as it can be'],
      mistakes:['Designing a component in isolation, then discovering it doesn\'t use existing tokens','Creating a new variant for every one-off use case instead of reusing existing ones','Skipping error, loading, or empty states until a developer asks for them later'],
      outputs:['Figma component library file with full variant sets','Component usage priority list','List of components deferred to a later version'],
      tips:'Ask an AI tool to generate a checklist of every state a component "should" have before you start designing it &mdash; it catches gaps you\'d otherwise find during dev handoff.'
    },
    {
      id:'docs', num:4, title:'Documentation & Guidelines', time:'1&ndash;2 weeks',
      desc:'Write the usage rules, accessibility notes, and do/don\'t guidance that make the system usable by someone who didn\'t build it.',
      objectives:['Document when and how to use each component','Capture accessibility requirements per component','Provide clear do/don\'t examples','Publish documentation somewhere the whole team (even future you) can reference'],
      tasks:['Write a usage section per component: purpose, when to use, when not to','Add accessibility notes (keyboard behavior, ARIA roles, focus order)','Create do/don\'t visual examples for the 5&ndash;10 most-used components','Publish to a documentation site or shared workspace'],
      tools:[
        {name:'Claude / ChatGPT', why:'Turns rough bullet notes on a component into clear, consistently structured documentation copy.'},
        {name:'Zeroheight', why:'Purpose-built for design system documentation, with AI-assisted content blocks tied directly to Figma components.'},
        {name:'Supernova', why:'Can auto-generate baseline documentation pages directly from your tokens and components, giving you a draft to edit rather than a blank page.'}
      ],
      alternatives:['Notion','Storybook Docs addon'],
      prompts:['Write a usage guideline for a Button component, covering when to use primary vs. secondary vs. ghost variants.','Draft accessibility notes for a modal dialog component, including expected keyboard and focus behavior.'],
      practices:['Write for someone who has never seen the system before','Show real product screenshots as examples, not isolated component previews','Version the docs alongside the components so nothing goes stale'],
      mistakes:['Publishing visuals with no usage rules, leaving "when to use this" ambiguous','Letting documentation drift out of sync after the first release','Writing accessibility notes as an afterthought instead of per component'],
      outputs:['Published documentation covering every foundation and component','Accessibility notes per component','Do/don\'t example set'],
      tips:'Feed an AI tool your component\'s Figma description plus a couple of bullet notes and ask for a first-draft usage section &mdash; editing is much faster than writing from a blank page.'
    },
    {
      id:'build', num:5, title:'Build & Developer Handoff', time:'2&ndash;4 weeks',
      desc:'Translate the token and component library into real, working code with a source-of-truth pipeline from design to build.',
      objectives:['Get tokens into a code-consumable format','Build components in code with full parity to Figma','Set up a living component preview (Storybook or equivalent)','Establish a visual regression safety net'],
      tasks:['Set up Style Dictionary (or similar) to transform Figma tokens into code variables','Build each component in your framework of choice with props matching the Figma variants','Set up Storybook with one story per state','Configure visual regression testing on component changes'],
      tools:[
        {name:'GitHub Copilot', why:'Speeds up scaffolding component code and tests once the first component pattern is established.'},
        {name:'Locofy / Anima', why:'Converts Figma frames into a starting-point code implementation, useful as scaffolding rather than final output.'},
        {name:'Claude / ChatGPT', why:'Strong for writing the Style Dictionary config and token transform scripts that keep design and code tokens in sync.'}
      ],
      alternatives:['Builder.io Visual Copilot','Figma Dev Mode','v0.dev'],
      prompts:['Write a Style Dictionary config that transforms these design tokens into CSS custom properties and a JS theme object.','Convert this component\'s Figma spec into a React component using these existing design tokens, including all listed states.'],
      practices:['Treat every AI-generated component as a first draft that still needs an accessibility and edge-case review','Keep tokens as the single source of truth &mdash; code should reference them, never redefine values','Set up automated visual regression testing before, not after, real usage begins'],
      mistakes:['Hardcoding pixel or hex values in code instead of referencing tokens','Skipping ARIA attributes and keyboard handling in AI-generated component code','Shipping components without any automated tests or visual regression checks'],
      outputs:['Coded component library matching Figma parity','Live Storybook instance','Token build pipeline (design tokens &rarr; code variables)'],
      tips:'Let AI generate the repetitive boilerplate and test scaffolding for each component, and spend your own review time on accessibility and edge cases &mdash; that\'s where AI output needs the most correction.'
    },
    {
      id:'governance', num:6, title:'Governance & Maintenance', time:'Ongoing, ~1 day/week',
      desc:'Define how the system evolves after v1 &mdash; versioning, contribution, adoption &mdash; so it doesn\'t decay the way the original UI did.',
      objectives:['Establish a versioning and change management policy','Define how new components or changes get proposed and reviewed','Track adoption across the real product, not just component count','Build a lightweight feedback loop with anyone else using the system'],
      tasks:['Adopt semantic versioning for the system (major/minor/patch)','Write a contribution guideline for proposing new components or changes','Set up a changelog that updates with every release','Audit real product screens periodically to measure adoption percentage'],
      tools:[
        {name:'Claude / ChatGPT', why:'Drafts governance docs, contribution templates, and changelog summaries quickly and consistently.'},
        {name:'Chromatic', why:'AI-assisted visual diffing that catches unintended component changes before they ship, protecting system integrity over time.'},
        {name:'Linear / Notion AI', why:'Keeps adoption tasks, proposed changes, and backlog items triaged without manual overhead.'}
      ],
      alternatives:['GitHub Projects','Jira'],
      prompts:['Draft a contribution guideline for a solo-maintained design system that may eventually add other designers.','Summarize these component change commits into a changelog entry in plain language.'],
      practices:['Version the system like software &mdash; breaking changes should be clearly flagged','Review changes against real usage before merging, not just visually','Measure success by adoption percentage across real screens, not by component count'],
      mistakes:['Treating v1 as "finished" with no ongoing maintenance plan','Having no clear versioning, so consumers can\'t tell what changed or why','Ignoring adoption data and assuming the system is used just because it exists'],
      outputs:['Versioning and contribution policy','Maintained changelog','Adoption tracking plan or dashboard'],
      tips:'Use AI to turn your commit or PR history into a changelog draft each release &mdash; it turns a chore you\'ll otherwise skip into a five-minute pass.'
    }
  ];

  let progress = LS.get('awa_progress', {});

  function renderStageMap(){
    const holder = document.getElementById('stagemap');
    holder.innerHTML = '';
    STAGES.forEach((s,i)=>{
      const node = document.createElement('div'); node.className='stagenode';
      const done = !!progress[s.id];
      if(i>0){
        const conn = document.createElement('div'); conn.className='connector' + (progress[STAGES[i-1].id] ? ' fill' : '');
        node.appendChild(conn);
      }
      const btn = document.createElement('button'); btn.className = 'stage-btn' + (done?' done':'');
      btn.innerHTML = `<span class="ring">${done?'&#10003;':s.num}</span><span class="lbl">${s.title}</span>`;
      btn.addEventListener('click', ()=>{ document.getElementById('stage-'+s.id).scrollIntoView({behavior:'smooth'}); });
      node.appendChild(btn);
      holder.appendChild(node);
    });
  }

  function renderStages(){
    const holder = document.getElementById('stagesHolder');
    holder.innerHTML = '';
    STAGES.forEach(s=>{
      const done = !!progress[s.id];
      const sec = document.createElement('section');
      sec.className = 'stage'; sec.id = 'stage-'+s.id;
      sec.innerHTML = `
        <div class="stage-head">
          <div class="stage-head-left">
            <div class="stage-num">${String(s.num).padStart(2,'0')}</div>
            <div>
              <h3>${s.title}</h3>
              <div class="stage-desc">${s.desc}</div>
              <div class="stage-badges" style="margin-top:10px;">
                <span class="badge time">&#9201; ${s.time}</span>
                <span class="badge">${s.tools.length} AI tools</span>
                <span class="badge">${s.prompts.length} prompts</span>
              </div>
            </div>
          </div>
          <button class="mark-done ${done?'on':''}" data-id="${s.id}">${done?'&#10003; Marked complete':'Mark stage complete'}</button>
        </div>

        <div class="stage-grid">
          <div class="panel">
            <h4><span class="swatch"></span>Objectives</h4>
            <ul>${s.objectives.map(o=>`<li>${o}</li>`).join('')}</ul>
          </div>
          <div class="panel">
            <h4><span class="swatch"></span>Tasks</h4>
            <ul>${s.tasks.map(t=>`<li>${t}</li>`).join('')}</ul>
          </div>

          <div class="panel full">
            <h4><span class="swatch"></span>Best AI tools for this stage</h4>
            ${s.tools.map(t=>`<div class="tool-row"><div><div class="tname">${t.name}</div><div class="twhy">${t.why}</div></div></div>`).join('')}
            <div class="alt-chip-row">${s.alternatives.map(a=>`<span class="alt-chip">Alt: ${a}</span>`).join('')}</div>
          </div>

          <div class="panel">
            <h4><span class="swatch"></span>Prompt examples</h4>
            ${s.prompts.map(p=>`<div class="prompt-block">${p}</div>`).join('')}
          </div>
          <div class="panel">
            <h4><span class="swatch"></span>Expected outputs</h4>
            <ul>${s.outputs.map(o=>`<li>${o}</li>`).join('')}</ul>
          </div>

          <div class="panel">
            <h4><span class="swatch"></span>Best practices</h4>
            ${s.practices.map(p=>`<div class="practice-row"><span class="ck">&#10003;</span><span>${p}</span></div>`).join('')}
          </div>
          <div class="panel">
            <h4><span class="swatch"></span>Common mistakes</h4>
            ${s.mistakes.map(m=>`<div class="mistake-row"><span class="x">&#10007;</span><span>${m}</span></div>`).join('')}
          </div>

          <div class="panel full">
            <h4><span class="swatch"></span>Efficiency tip</h4>
            <div style="font-size:13px; line-height:1.6;">${s.tips}</div>
          </div>
        </div>
      `;
      holder.appendChild(sec);
    });

    holder.querySelectorAll('.mark-done').forEach(btn=>{
      btn.addEventListener('click', ()=>{
        const id = btn.dataset.id;
        progress[id] = !progress[id];
        LS.set('awa_progress', progress);
        renderStageMap(); renderStages(); renderSummary();
        document.getElementById('stage-'+id).scrollIntoView({behavior:'smooth'});
      });
    });
  }

  function renderSummary(){
    const doneCount = STAGES.filter(s=>progress[s.id]).length;
    document.getElementById('sumDone').textContent = doneCount + '/' + STAGES.length;
    document.getElementById('sumProgressBar').style.width = (doneCount/STAGES.length*100) + '%';
  }

  renderStageMap();
  renderStages();
  renderSummary();
})();
</script>
</body>
</html>
