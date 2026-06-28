<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Prior Authorization Story Simulator</title>
<script src="https://cdn.tailwindcss.com"></script>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=Lora:ital,wght@0,500;1,400&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #F0F4F8;
    --surface: #FFFFFF;
    --surface-alt: #EBF4FF;
    --border: #D1E3F8;
    --accent-blue: #2563EB;
    --accent-teal: #0D9488;
    --accent-amber: #D97706;
    --accent-red: #DC2626;
    --accent-green: #16A34A;
    --text-primary: #1E293B;
    --text-secondary: #475569;
    --text-muted: #94A3B8;
    --rahul-bg: #DBEAFE;
    --rahul-border: #93C5FD;
    --priya-bg: #CCFBF1;
    --priya-border: #5EEAD4;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    font-family: 'Inter', sans-serif;
    background: var(--bg);
    color: var(--text-primary);
    min-height: 100vh;
  }

  .app-header {
    background: linear-gradient(135deg, #1E40AF 0%, #0F766E 100%);
    padding: 16px 24px;
    display: flex;
    align-items: center;
    gap: 12px;
    box-shadow: 0 2px 12px rgba(0,0,0,0.15);
  }

  .app-header h1 {
    font-family: 'Lora', serif;
    font-size: 1.25rem;
    font-weight: 500;
    color: #fff;
    letter-spacing: -0.02em;
  }

  .app-header p {
    font-size: 0.75rem;
    color: rgba(255,255,255,0.7);
    margin-top: 1px;
  }

  .progress-area {
    background: var(--surface);
    border-bottom: 1px solid var(--border);
    padding: 12px 24px;
  }

  .progress-labels {
    display: flex;
    justify-content: space-between;
    margin-bottom: 6px;
    font-size: 0.7rem;
    font-weight: 600;
    color: var(--text-secondary);
    text-transform: uppercase;
    letter-spacing: 0.06em;
  }

  .progress-track {
    height: 6px;
    background: #E2E8F0;
    border-radius: 99px;
    overflow: hidden;
  }

  .progress-fill {
    height: 100%;
    background: linear-gradient(90deg, #2563EB, #0D9488);
    border-radius: 99px;
    transition: width 0.6s cubic-bezier(0.4, 0, 0.2, 1);
    width: 0%;
  }

  .scene-chips {
    display: flex;
    gap: 4px;
    margin-top: 8px;
    flex-wrap: wrap;
  }

  .scene-chip {
    font-size: 0.65rem;
    font-weight: 500;
    padding: 2px 8px;
    border-radius: 99px;
    background: #F1F5F9;
    color: var(--text-muted);
    border: 1px solid #E2E8F0;
    transition: all 0.3s;
  }

  .scene-chip.active {
    background: #DBEAFE;
    color: var(--accent-blue);
    border-color: #93C5FD;
  }

  .scene-chip.done {
    background: #DCFCE7;
    color: var(--accent-green);
    border-color: #86EFAC;
  }

  .chat-feed {
    flex: 1;
    padding: 20px 24px;
    display: flex;
    flex-direction: column;
    gap: 14px;
    overflow-y: auto;
    max-height: calc(100vh - 240px);
  }

  /* Rahul — left */
  .bubble-rahul {
    display: flex;
    align-items: flex-start;
    gap: 10px;
    max-width: 72%;
    align-self: flex-start;
  }

  .bubble-rahul .avatar {
    width: 36px;
    height: 36px;
    border-radius: 50%;
    background: var(--rahul-bg);
    border: 2px solid var(--rahul-border);
    display: flex;align-items:center;justify-content:center;
    font-size: 18px;
    flex-shrink: 0;
  }

  .bubble-rahul .msg {
    background: var(--rahul-bg);
    border: 1px solid var(--rahul-border);
    border-radius: 4px 18px 18px 18px;
    padding: 10px 14px;
    font-size: 0.875rem;
    line-height: 1.55;
    color: var(--text-primary);
  }

  .bubble-rahul .name {
    font-size: 0.65rem;
    font-weight: 700;
    color: var(--accent-blue);
    text-transform: uppercase;
    letter-spacing: 0.08em;
    margin-bottom: 4px;
  }

  /* Priya — right */
  .bubble-priya {
    display: flex;
    align-items: flex-start;
    gap: 10px;
    max-width: 72%;
    align-self: flex-end;
    flex-direction: row-reverse;
  }

  .bubble-priya .avatar {
    width: 36px;
    height: 36px;
    border-radius: 50%;
    background: var(--priya-bg);
    border: 2px solid var(--priya-border);
    display: flex;align-items:center;justify-content:center;
    font-size: 18px;
    flex-shrink: 0;
  }

  .bubble-priya .msg {
    background: var(--priya-bg);
    border: 1px solid var(--priya-border);
    border-radius: 18px 4px 18px 18px;
    padding: 10px 14px;
    font-size: 0.875rem;
    line-height: 1.55;
    color: var(--text-primary);
  }

  .bubble-priya .name {
    font-size: 0.65rem;
    font-weight: 700;
    color: var(--accent-teal);
    text-transform: uppercase;
    letter-spacing: 0.08em;
    margin-bottom: 4px;
    text-align: right;
  }

  /* Narrator — centered italic */
  .narrator {
    text-align: center;
    font-family: 'Lora', serif;
    font-style: italic;
    font-size: 0.82rem;
    color: var(--text-secondary);
    padding: 4px 12px;
    position: relative;
  }

  .narrator::before, .narrator::after {
    content: '';
    display: inline-block;
    width: 32px;
    height: 1px;
    background: var(--text-muted);
    vertical-align: middle;
    margin: 0 10px;
  }

  /* Scene title */
  .scene-title {
    text-align: center;
    background: linear-gradient(135deg, #EFF6FF, #F0FDFA);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 10px 16px;
    margin: 6px 0;
  }

  .scene-title .label {
    font-size: 0.65rem;
    font-weight: 700;
    color: var(--accent-blue);
    text-transform: uppercase;
    letter-spacing: 0.1em;
  }

  .scene-title .title {
    font-family: 'Lora', serif;
    font-size: 1rem;
    font-weight: 500;
    color: var(--text-primary);
    margin-top: 2px;
  }

  /* Info card for stats/citations */
  .info-card {
    background: #FFFBEB;
    border: 1px solid #FDE68A;
    border-left: 4px solid var(--accent-amber);
    border-radius: 8px;
    padding: 10px 14px;
    font-size: 0.8rem;
    color: #78350F;
    line-height: 1.5;
    margin: 0 8px;
  }

  .info-card.red {
    background: #FEF2F2;
    border-color: #FECACA;
    border-left-color: var(--accent-red);
    color: #7F1D1D;
  }

  .info-card.green {
    background: #F0FDF4;
    border-color: #BBF7D0;
    border-left-color: var(--accent-green);
    color: #14532D;
  }

  .info-card.blue {
    background: #EFF6FF;
    border-color: #BFDBFE;
    border-left-color: var(--accent-blue);
    color: #1E3A5F;
  }

  /* Flow diagram */
  .flow-diagram {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    background: #F8FAFC;
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 12px 16px;
    margin: 0 8px;
    flex-wrap: wrap;
  }

  .flow-node {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 6px 12px;
    font-size: 0.75rem;
    font-weight: 600;
    color: var(--text-primary);
    text-align: center;
  }

  .flow-arrow {
    color: var(--accent-blue);
    font-size: 1rem;
    font-weight: bold;
  }

  /* Choices */
  .choices-area {
    border-top: 1px solid var(--border);
    padding: 16px 24px;
    background: var(--surface);
  }

  .choices-label {
    font-size: 0.7rem;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.08em;
    color: var(--text-muted);
    margin-bottom: 10px;
  }

  .choices-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 10px;
  }

  .choice-btn {
    background: var(--surface);
    border: 1.5px solid var(--border);
    border-radius: 10px;
    padding: 12px 14px;
    text-align: left;
    cursor: pointer;
    font-size: 0.82rem;
    line-height: 1.45;
    color: var(--text-primary);
    font-family: 'Inter', sans-serif;
    transition: all 0.2s;
    position: relative;
    overflow: hidden;
  }

  .choice-btn::before {
    content: '';
    position: absolute;
    inset: 0;
    background: linear-gradient(135deg, #EFF6FF, #F0FDFA);
    opacity: 0;
    transition: opacity 0.2s;
  }

  .choice-btn:hover:not(:disabled) {
    border-color: var(--accent-blue);
    transform: translateY(-1px);
    box-shadow: 0 4px 12px rgba(37,99,235,0.12);
  }

  .choice-btn:hover:not(:disabled)::before {
    opacity: 1;
  }

  .choice-btn:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }

  .choice-btn .choice-icon {
    font-size: 1rem;
    margin-bottom: 4px;
    display: block;
    position: relative;
  }

  .choice-btn .choice-text {
    position: relative;
    font-weight: 500;
  }

  /* Restart button */
  .restart-btn {
    display: block;
    margin: 0 auto;
    background: linear-gradient(135deg, #2563EB, #0D9488);
    color: white;
    border: none;
    border-radius: 10px;
    padding: 12px 28px;
    font-size: 0.9rem;
    font-weight: 600;
    font-family: 'Inter', sans-serif;
    cursor: pointer;
    transition: all 0.2s;
    margin-top: 8px;
  }

  .restart-btn:hover {
    transform: translateY(-2px);
    box-shadow: 0 6px 20px rgba(37,99,235,0.3);
  }

  /* Typing indicator */
  .typing {
    display: flex;
    gap: 4px;
    padding: 10px 14px;
    background: #F1F5F9;
    border-radius: 18px;
    width: fit-content;
    align-self: flex-start;
  }

  .typing span {
    width: 6px;
    height: 6px;
    background: var(--text-muted);
    border-radius: 50%;
    animation: bounce 1.2s infinite;
  }

  .typing span:nth-child(2) { animation-delay: 0.2s; }
  .typing span:nth-child(3) { animation-delay: 0.4s; }

  @keyframes bounce {
    0%, 60%, 100% { transform: translateY(0); }
    30% { transform: translateY(-6px); }
  }

  @media (max-width: 600px) {
    .choices-grid { grid-template-columns: 1fr; }
    .bubble-rahul, .bubble-priya { max-width: 90%; }
    .chat-feed { max-height: calc(100vh - 280px); }
  }

  .tag {
    display: inline-block;
    font-size: 0.65rem;
    font-weight: 700;
    padding: 1px 7px;
    border-radius: 99px;
    margin-right: 4px;
    text-transform: uppercase;
    letter-spacing: 0.06em;
  }

  .tag-denied { background: #FEE2E2; color: #B91C1C; }
  .tag-approved { background: #DCFCE7; color: #15803D; }
  .tag-pending { background: #FEF9C3; color: #92400E; }
</style>
</head>
<body>

<header class="app-header">
  <div style="font-size:28px">🏥</div>
  <div>
    <h1>Prior Authorization Story Simulator</h1>
    <p>Follow Rahul's journey through the U.S. healthcare system — guided by Priya</p>
  </div>
</header>

<div class="progress-area">
  <div class="progress-labels">
    <span id="scene-label">Scene 1 of 8</span>
    <span id="pct-label">0%</span>
  </div>
  <div class="progress-track">
    <div class="progress-fill" id="progress-fill"></div>
  </div>
  <div class="scene-chips" id="scene-chips"></div>
</div>

<div class="chat-feed" id="chat-feed"></div>

<div class="choices-area" id="choices-area">
  <div class="choices-label">What would you like to do?</div>
  <div class="choices-grid" id="choices-grid"></div>
</div>

<script>
// ─── Story Data ───────────────────────────────────────────────────────────────

const SCENES = [
  {
    id: 1,
    title: "Doctor Visit",
    emoji: "🩺",
    intro: "City Medical Center — Examination Room 3",
    messages: [
      { type: "narrator", text: "Rahul, 34, has been experiencing joint pain and stiffness for months. Today he finally visits Dr. Patel at City Medical Center." },
      { type: "rahul", text: "Dr. Patel, my knuckles have been swollen every morning for weeks. Some days I can barely open a jar." },
      { type: "narrator", text: "Dr. Patel reviews Rahul's blood work, imaging results, and symptom history." },
      { type: "narrator", text: "Dr. Patel says: 'Rahul, based on your RF factor, anti-CCP antibodies, and the pattern of joint involvement — I'm diagnosing you with Rheumatoid Arthritis.'" },
      { type: "rahul", text: "Rheumatoid Arthritis? That sounds serious. What does that mean for me?" },
      { type: "narrator", text: "Dr. Patel explains RA is an autoimmune condition that causes the immune system to attack the joints. Without treatment, it can lead to permanent joint damage." },
      { type: "narrator", text: "Dr. Patel says: 'I'm prescribing Humira — it's a biologic medication that targets the specific protein causing inflammation. It's one of the most effective treatments for moderate-to-severe RA.'" },
      { type: "rahul", text: "Okay. So I just pick it up at the pharmacy?" },
      { type: "priya", text: "Hi Rahul! I'm Priya — a healthcare operations specialist. I'm here to help you understand what happens next. Humira is a specialty biologic drug. Before the pharmacy can dispense it, your insurance company needs to give something called a Prior Authorization. Your journey is just beginning! 😊" },
    ],
    choices: [
      { icon: "🤔", text: "Ask Priya: 'What exactly is prior authorization?'", tag: "curious" },
      { icon: "😟", text: "Say: 'That sounds like it'll take forever. What if I just pay out of pocket?'", tag: "worried" },
    ]
  },
  {
    id: 2,
    title: "Insurance Roadblock",
    emoji: "📋",
    intro: "Dr. Patel's Office — Administrative Suite",
    messages: [
      { type: "narrator", text: "The next morning, Dr. Patel's administrative team begins the Prior Authorization process on Rahul's behalf." },
      { type: "narrator", text: "Unlike filling a regular prescription, Humira requires formal review by the insurance company before it can be approved." },
      { type: "priya", text: "Here's how the PA request flows — notice there's no pharmacy involved at this stage. It goes straight from the provider to the payer:" },
      {
        type: "flow",
        nodes: ["Dr. Patel's Office\n(Provider)", "PA Request →", "StarCare Health\n(Payer)"],
      },
      { type: "rahul", text: "Wait — StarCare Health is my insurance company? Why do they have to approve my doctor's prescription?" },
      { type: "priya", text: "Great question! StarCare Health — which we're using as an illustrative example throughout this story — is the payer. They pay the claims. Because Humira can cost $25,000–$50,000 per year, insurers require clinical justification before agreeing to cover it." },
      { type: "narrator", text: "Dr. Patel's office submits a PA request packet to StarCare Health. This includes Rahul's diagnosis codes, lab results, and treatment history." },
      { type: "rahul", text: "So my doctor is doing all this work just to get the insurance to say yes?" },
      { type: "priya", text: "Exactly. And here's an important detail: once a PA is approved, it's saved on file permanently for that medication. Rahul won't need to re-submit a PA for Humira as long as the conditions of approval are met. It's a one-time gate — but that gate can be tough to get through." },
      {
        type: "infocard",
        variant: "blue",
        text: "📌 Key fact: PA approvals are kept on file. An approved PA for a specific medication removes the barrier for future refills under the same plan — unless the plan changes or the PA expires."
      },
    ],
    choices: [
      { icon: "📊", text: "Ask: 'What does StarCare Health actually check when they review my request?'", tag: "detail" },
      { icon: "⏱️", text: "Ask: 'How long does this review process usually take?'", tag: "timing" },
    ]
  },
  {
    id: 3,
    title: "What Is PA?",
    emoji: "💡",
    intro: "Priya Explains — Plain Language Breakdown",
    messages: [
      { type: "priya", text: "Let me break down Prior Authorization in plain language. Think of it like a gate between your doctor's recommendation and your insurance coverage." },
      { type: "narrator", text: "Priya opens a simple explanation." },
      { type: "priya", text: "PA is a process where your insurance company requires your doctor to prove, in advance, that a specific treatment is medically necessary before they'll agree to pay for it." },
      { type: "rahul", text: "Why does this exist? Isn't that just bureaucracy?" },
      { type: "priya", text: "It feels that way — but there's a real reason. For some conditions, there's a clinical protocol called step therapy. The idea is to try a less expensive, well-established medication first before jumping to a costly biologic like Humira." },
      { type: "rahul", text: "That kind of makes sense. But what if the cheaper medication doesn't work?" },
      { type: "priya", text: "That's exactly where it gets important. If you've already tried and failed step therapy, your doctor documents that — and the PA is meant to confirm it. But here's the problem: for aggressive diagnoses like Rheumatoid Arthritis, every week of delay matters." },
      {
        type: "infocard",
        variant: "amber",
        text: "⚠️ Step therapy isn't just bureaucracy — for aggressive diagnoses like RA, treatment delays can affect disease progression. Untreated inflammation causes irreversible joint damage over time."
      },
      {
        type: "infocard",
        variant: "red",
        text: "📊 AMA 2023 PA Survey: PA causes treatment delays in the majority of cases. Many physicians report that PA requirements lead patients to abandon treatment altogether."
      },
      { type: "priya", text: "The system was designed to prevent unnecessary spending. But when it slows down medically appropriate treatment, the human cost is real. That's the tension at the heart of PA." },
      { type: "rahul", text: "I had no idea this was so complicated. I thought my insurance just... covered things my doctor prescribed." },
      { type: "priya", text: "Most people think that! You're learning something that millions of Americans encounter — often at the worst possible time." },
    ],
    choices: [
      { icon: "🔍", text: "Ask: 'Walk me through exactly what reviewers look at in my file.'", tag: "detail" },
      { icon: "💊", text: "Ask: 'What is step therapy, and did I have to do it?'", tag: "step-therapy" },
    ]
  },
  {
    id: 4,
    title: "Insurance Review",
    emoji: "🔎",
    intro: "StarCare Health — Clinical Review Department",
    messages: [
      { type: "narrator", text: "Inside StarCare Health's clinical review department, a nurse reviewer pulls up Rahul's PA request. Here's what they check — and why each item matters." },
      { type: "priya", text: "StarCare Health's reviewers go through a standard checklist. Let me walk you through each step:" },
      { type: "priya", text: "① Eligibility Check — First, they verify Rahul is an active member of StarCare Health's plan and that Humira is covered under his specific plan tier. If someone's plan lapsed, the request gets rejected before it's even reviewed." },
      { type: "rahul", text: "Okay, I'm definitely still on my plan. What's next?" },
      { type: "priya", text: "② Clinical Documentation — They review the supporting documents Dr. Patel submitted: Rahul's lab results (RF factor, anti-CCP antibodies), physician notes, imaging results, and the formal diagnosis. This is the medical evidence that justifies Humira." },
      { type: "priya", text: "③ ICD-10 Diagnosis Code Match — Every diagnosis has a code in a system called ICD-10. For Rahul, that's M05.79 — Rheumatoid arthritis with rheumatoid factor. The code on Dr. Patel's PA request must match the code that StarCare Health's policy allows Humira for. A single mismatch can trigger a denial." },
      { type: "rahul", text: "A code mismatch? My doctor just types the wrong number and I lose coverage?" },
      { type: "priya", text: "Unfortunately, yes — it happens more than you'd think. That's why medical coders in physician offices are so important." },
      { type: "priya", text: "④ Step Therapy History — Finally, they check whether Rahul has tried and documented previous RA medications. StarCare Health's policy for Humira requires proof of trying at least one conventional DMARD, like Methotrexate, first — unless there's a documented contraindication." },
      {
        type: "infocard",
        variant: "blue",
        text: "📋 DMARD = Disease-Modifying Antirheumatic Drug. These are typically the first-line treatments for RA. Biologics like Humira are step-therapy 'step two' for many insurance plans."
      },
      { type: "rahul", text: "Did Dr. Patel include all of that in the paperwork?" },
      { type: "narrator", text: "StarCare Health's reviewer finishes the checklist. There's a pause." },
    ],
    choices: [
      { icon: "😰", text: "Ask anxiously: 'So what did they decide?'", tag: "anxious" },
      { icon: "📝", text: "Ask: 'What happens if something is missing from the documentation?'", tag: "practical" },
    ]
  },
  {
    id: 5,
    title: "Denial",
    emoji: "❌",
    intro: "Rahul's Phone — StarCare Health Notification",
    messages: [
      { type: "narrator", text: "Three business days later, Rahul receives a letter — and a notification in his StarCare Health app." },
      {
        type: "infocard",
        variant: "red",
        text: "❌ PRIOR AUTHORIZATION DENIED\n\nMember: Rahul Sharma  |  Medication: Humira (adalimumab)\nReason: Insufficient step therapy documentation. Records provided do not include documented trial and failure of a conventional DMARD prior to biologic initiation.\n\nThis is not a permanent denial. You have the right to appeal."
      },
      { type: "rahul", text: "I've been DENIED? I have a real diagnosis! My joints are getting worse every week and they're saying no?!" },
      { type: "priya", text: "I know this feels devastating, Rahul. But I need you to hear this clearly: a denial is not the end. It happens to many people, and it's often successfully reversed on appeal. The reason here is specific — missing step therapy documentation." },
      { type: "rahul", text: "So it's just paperwork? They're denying my medication over paperwork?" },
      { type: "priya", text: "In a sense, yes. Dr. Patel may have already tried conventional treatment with you, but if that wasn't formally documented in the PA submission, StarCare Health has no way to verify it. The system requires evidence, not just good intentions." },
      {
        type: "infocard",
        variant: "amber",
        text: "⏱️ System Cost: PA denials cost physician offices an average of 2+ staff hours to resolve. That's time nurses and administrative staff spend on appeals instead of direct patient care."
      },
      { type: "priya", text: "Here's the important distinction: Denial ≠ Permanent. What we need to do now is file a formal appeal. That's where you and Dr. Patel fight back — with documentation." },
      { type: "rahul", text: "Okay. Tell me what we need to do." },
    ],
    choices: [
      { icon: "⚔️", text: "Say: 'Let's fight this. Walk me through the appeal process.'", tag: "fight" },
      { icon: "😔", text: "Ask: 'Is there any way to get the medication while we wait for the appeal?'", tag: "bridge" },
    ]
  },
  {
    id: 6,
    title: "Appeal",
    emoji: "📄",
    intro: "Dr. Patel's Office — Building the Case",
    messages: [
      { type: "narrator", text: "Rahul calls Dr. Patel's office. The administrative team springs into action to prepare a formal appeal." },
      { type: "priya", text: "Here's what goes into a strong appeal. Think of it as building a case file:" },
      { type: "priya", text: "① Gather All Documentation — Dr. Patel pulls Rahul's complete treatment history, including the Methotrexate trial from 8 months ago that wasn't included in the original submission. This is the missing piece." },
      { type: "rahul", text: "Wait — I did try Methotrexate! I was on it for 4 months and it barely helped. Why wasn't that in the original request?" },
      { type: "narrator", text: "Dr. Patel's office acknowledges the gap. The records were in a different system and weren't pulled into the initial PA packet. This is a common administrative gap." },
      { type: "priya", text: "② Letter of Medical Necessity (LMN) — Dr. Patel writes a formal letter explaining, in clinical detail, why Rahul specifically needs Humira and not a lower-cost alternative. This letter is the heart of the appeal." },
      { type: "priya", text: "A strong LMN includes: the specific diagnosis with ICD-10 code, lab values supporting severity, documented history of prior treatments and their failure, and clinical reasoning for why Humira is the appropriate next step." },
      { type: "rahul", text: "This sounds like Dr. Patel has to write an essay just to defend her own prescription." },
      { type: "priya", text: "That's a fair way to put it. And it takes time the clinical staff could spend with other patients. But it's the process — and Dr. Patel knows it well." },
      { type: "priya", text: "③ File the Formal Appeal — The complete packet — updated records, LMN, original denial letter, and Rahul's member information — gets submitted to StarCare Health's appeals department. The clock starts: insurers typically have 30 days to respond to a standard appeal, or 72 hours for urgent cases." },
      { type: "rahul", text: "Can we request urgent review? My joints are getting worse." },
      { type: "priya", text: "Yes — and Dr. Patel includes documentation of ongoing disease progression, which supports an expedited review request. That's exactly the right move." },
      { type: "narrator", text: "The appeal packet is submitted. Now comes the waiting." },
    ],
    choices: [
      { icon: "🙏", text: "Think: 'I just have to hope they get it right this time.'", tag: "hopeful" },
      { icon: "📞", text: "Ask: 'Can Rahul or Dr. Patel speak directly to someone at StarCare Health?'", tag: "peer-review" },
    ]
  },
  {
    id: 7,
    title: "Approval",
    emoji: "✅",
    intro: "11 Days Later — The Decision Arrives",
    messages: [
      { type: "narrator", text: "Eleven days after the appeal was filed, Dr. Patel's office receives a fax and Rahul gets a notification." },
      {
        type: "infocard",
        variant: "green",
        text: "✅ PRIOR AUTHORIZATION APPROVED\n\nMember: Rahul Sharma  |  Medication: Humira (adalimumab)\nPA Reference Number: SCH-2024-RA-88412\nAuthorization Period: 12 months\n\nThis PA is saved on file. No repeat submission required for Humira refills within the authorization period. The pharmacy has been notified."
      },
      { type: "rahul", text: "It's approved! Finally! I can't believe how long that took just to get permission to use the medication my doctor recommended." },
      { type: "priya", text: "This moment is real, Rahul. The PA reference number — SCH-2024-RA-88412 — is now on file with StarCare Health. For the next 12 months, every Humira refill will reference this approval. No re-submitting. No starting over." },
      { type: "narrator", text: "Dr. Patel's nurse calls Rahul to explain what happens next. A specialty pharmacy will contact him within 24 hours to coordinate delivery of Humira, along with instructions for self-injection and a nurse support line." },
      { type: "rahul", text: "So what did we learn from all of this? I went from excited about a treatment to terrified about a denial to relieved about an approval — all in a few weeks." },
      { type: "priya", text: "And that experience is the story of millions of Americans. You've just navigated one of the most common and frustrating aspects of U.S. healthcare. The fact that you persisted — and Dr. Patel's team was thorough — made the difference." },
      { type: "priya", text: "Many people give up after a denial. Some can't afford to wait and go without treatment. That's the human stakes behind what looks like an administrative process." },
      { type: "rahul", text: "I feel like I need to share this with people. Most of my friends have no idea how this works." },
      { type: "priya", text: "That's exactly why I'm here. Let's review the key takeaways from both sides of the experience." },
    ],
    choices: [
      { icon: "📖", text: "Ask: 'What should every patient know about PA before it happens to them?'", tag: "patient-side" },
      { icon: "🏥", text: "Ask: 'How do health systems track whether this process is working?'", tag: "system-side" },
    ]
  },
  {
    id: 8,
    title: "Takeaways",
    emoji: "🎓",
    intro: "Key Lessons — Two Perspectives",
    messages: [
      { type: "narrator", text: "Priya takes a moment to summarize Rahul's journey — from both a patient perspective and a health systems perspective." },
      { type: "priya", text: "Let's start with what Rahul's experience teaches us from the patient side:" },
      {
        type: "infocard",
        variant: "blue",
        text: "👦 PATIENT PERSPECTIVE — What Rahul Learned:\n\n• PA is standard for specialty/biologic medications — expect it, don't be surprised\n• A denial is not a final answer. Most are appealed successfully with complete documentation\n• Your doctor's office fights alongside you — but they need your full treatment history\n• Step therapy documentation is critical: keep records of every medication you've tried\n• Ask your insurer upfront: 'Does this medication require PA?' — before you're expecting a prescription\n• Request expedited review if your condition is worsening"
      },
      { type: "rahul", text: "And what about the bigger picture? How does the healthcare system view all of this?" },
      { type: "priya", text: "From a health systems and operations view, PA is tracked as a performance metric. Payers, providers, and regulators all look at three numbers:" },
      {
        type: "infocard",
        variant: "green",
        text: "🏥 SYSTEM PERSPECTIVE — How Health Systems Measure PA:\n\n• Denial Rate — What % of PA requests are denied on first submission? High rates signal documentation gaps or overly restrictive criteria\n• Appeal Rate — What % of denials are contested? High appeal rates signal that initial denials may be clinically inappropriate\n• Resolution Time — How long does the full cycle take? Long times = patients waiting for treatment, staff hours wasted\n\nHealth systems, payers, and quality accreditors use these metrics to evaluate whether PA programs are balancing cost control with patient access."
      },
      {
        type: "infocard",
        variant: "amber",
        text: "🔭 The Future of PA: Electronic Prior Authorization (ePA) is expanding. This automates parts of the submission and review process, reducing delays. CMS has introduced rules requiring payers to respond faster and improve transparency. The goal: keep clinical oversight without the weeks-long wait."
      },
      { type: "rahul", text: "So this system is being reformed?" },
      { type: "priya", text: "Slowly. Healthcare moves slowly. But yes — the experience you went through is exactly what's driving policy change. And now you understand it well enough to advocate for yourself — and others." },
      { type: "narrator", text: "Rahul starts Humira two days later. Three months in, his morning stiffness is significantly better. He still keeps his PA reference number saved in his phone." },
      { type: "priya", text: "Thank you for going through this journey, Rahul. And thank you for learning alongside us. 🏥" },
    ],
    choices: [
      { icon: "🔄", text: "Restart the story and explore the other dialogue paths", tag: "restart" },
      { icon: "📚", text: "Explore again — there's another set of responses waiting", tag: "restart" },
    ],
    isEnd: true
  },
];

// ─── Alternate responses per choice ──────────────────────────────────────────
const ALT_RESPONSES = {
  "1-1": [ // Scene 1, Choice A (curious)
    { type: "rahul", text: "Priya, can you explain what 'prior authorization' actually means? Is this a normal thing?" },
    { type: "priya", text: "Very normal — and very important to understand. Prior authorization is essentially your insurance company's pre-approval process. Before they agree to pay for certain medications or procedures, they need your doctor to prove it's medically necessary. It's like getting a go-ahead before spending." },
  ],
  "1-2": [ // Scene 1, Choice B (worried)
    { type: "rahul", text: "What if I just skip the insurance process and pay out of pocket?" },
    { type: "priya", text: "I completely understand that impulse! But Humira's list price is roughly $6,000–$8,000 per injection — with dosing every 2 weeks, that's over $150,000 a year. Most people genuinely can't bypass this process. Understanding it is the path forward." },
  ],
  "2-1": [ // Scene 2, choice A (detail)
    { type: "rahul", text: "What exactly does StarCare Health check when they look at my request?" },
    { type: "priya", text: "Good question — we'll go deep on this in the next scene. They review your eligibility, your diagnosis codes, your clinical documentation, and critically — your step therapy history. Each one is a gate that needs to be cleared." },
  ],
  "2-2": [ // Scene 2, Choice B (timing)
    { type: "rahul", text: "How long does StarCare Health usually take to review a PA request?" },
    { type: "priya", text: "For standard reviews, payers typically have 14 days by law. For urgent cases, it's 72 hours. In reality, most decisions come back in 3–5 business days. But any delay means Rahul is waiting for treatment. Time is not neutral here — it matters clinically." },
  ],
  "3-1": [ // Scene 3, Choice A (detail)
    { type: "rahul", text: "Can you walk me through exactly what the reviewer sees in my file?" },
    { type: "priya", text: "Absolutely. The reviewer opens a structured checklist: eligibility status, the diagnosis code your doctor submitted, clinical notes supporting the diagnosis, and a history of prior treatments. Each item either clears or raises a flag. One flag can trigger a denial." },
  ],
  "3-2": [ // Scene 3, Choice B (step therapy)
    { type: "rahul", text: "What exactly is step therapy, and did I go through it?" },
    { type: "priya", text: "Step therapy means starting with a lower-cost 'step one' medication before approving a more expensive 'step two' option. For RA, that often means trying Methotrexate before Humira. If you've already done that — and your doctor documented it — that clears a major PA hurdle. Did you try anything before Humira was prescribed?" },
    { type: "rahul", text: "Actually yes — Dr. Patel had me on Methotrexate about 8 months ago. I didn't know that mattered for the insurance paperwork." },
    { type: "priya", text: "It matters enormously. That history needs to be in your PA submission. We'll see why in just a moment." },
  ],
  "4-1": [ // Scene 4, Choice A (anxious)
    { type: "rahul", text: "Okay... so what did they decide? I'm nervous." },
    { type: "narrator", text: "StarCare Health completes the review. The checklist shows three green items — and one red flag." },
  ],
  "4-2": [ // Scene 4, Choice B (practical)
    { type: "rahul", text: "What happens if something's missing from the documentation?" },
    { type: "priya", text: "The most likely outcome is a denial — not because the treatment is wrong, but because the paperwork doesn't prove it. It's one of the most frustrating things about the PA system: a clinically appropriate prescription can be denied for an administrative gap. And that's exactly what's about to happen." },
  ],
  "5-1": [ // Scene 5, Choice A (fight)
    { type: "rahul", text: "Let's fight this. I'm not giving up — walk me through exactly what we need to do to appeal." },
    { type: "priya", text: "That's the right attitude. Appeals require a structured packet: updated records, a formal Letter of Medical Necessity, and a clear response to the specific denial reason. Dr. Patel's team knows this process. Let's get moving." },
  ],
  "5-2": [ // Scene 5, Choice B (bridge)
    { type: "rahul", text: "Is there anything I can do to get Humira while we wait on the appeal?" },
    { type: "priya", text: "There are a few options to explore. AbbVie, the maker of Humira, has a patient assistance program called myAbbVie Assist. Dr. Patel can also request samples to bridge the gap. And some specialty pharmacies work with providers to facilitate access during appeal periods. It's worth asking." },
  ],
  "6-1": [ // Scene 6, Choice A (hopeful)
    { type: "rahul", text: "I've done everything I can. I just have to hope they get it right this time." },
    { type: "priya", text: "Hope is valid — and it's backed by evidence. When appeals include complete step therapy documentation and a strong Letter of Medical Necessity, success rates improve significantly. Dr. Patel's team has done this before. The packet is thorough." },
  ],
  "6-2": [ // Scene 6, Choice B (peer-review)
    { type: "rahul", text: "Can Dr. Patel actually speak directly to someone at StarCare Health to make the case?" },
    { type: "priya", text: "Yes — this is called a peer-to-peer review. Dr. Patel can request to speak directly with the StarCare Health medical director reviewing the case. Physician-to-physician conversations can be very effective, especially for complex cases. Dr. Patel puts in that request alongside the written appeal." },
  ],
  "7-1": [ // Scene 7, Choice A (patient side)
    { type: "rahul", text: "What should every patient know about PA before they face it?" },
    { type: "priya", text: "The single most important thing: don't wait until you're denied to ask questions. When your doctor prescribes anything specialty-level, immediately ask: 'Will this need prior authorization, and what will my insurance need to see?' Getting ahead of it saves weeks." },
  ],
  "7-2": [ // Scene 7, Choice B (system side)
    { type: "rahul", text: "How do health systems actually track whether the PA process is working?" },
    { type: "priya", text: "Three metrics matter most: denial rate tells you if requests are being completed correctly; appeal rate signals whether initial denials were clinically justified; and resolution time tracks how long patients wait. Payers use these internally, and accrediting bodies like NCQA review them to assess plan quality." },
  ],
  "8-1": [ // Scene 8, end choice A
    { type: "narrator", text: "You've reached the end of Rahul's journey. Use the restart button below to explore the alternative dialogue paths." },
  ],
  "8-2": [ // Scene 8, end choice B
    { type: "narrator", text: "Rahul's story has multiple paths. Click restart to explore what happens when you choose differently at each decision point." },
  ],
};

// ─── App State ────────────────────────────────────────────────────────────────
let currentScene = 0;
let isAnimating = false;

// ─── DOM Refs ─────────────────────────────────────────────────────────────────
const chatFeed = document.getElementById('chat-feed');
const choicesArea = document.getElementById('choices-area');
const choicesGrid = document.getElementById('choices-grid');
const progressFill = document.getElementById('progress-fill');
const sceneLabel = document.getElementById('scene-label');
const pctLabel = document.getElementById('pct-label');
const sceneChips = document.getElementById('scene-chips');

// ─── Build scene chips ────────────────────────────────────────────────────────
function buildChips() {
  sceneChips.innerHTML = '';
  SCENES.forEach((s, i) => {
    const chip = document.createElement('div');
    chip.className = 'scene-chip';
    chip.id = `chip-${i}`;
    chip.textContent = `${s.emoji} ${s.title}`;
    sceneChips.appendChild(chip);
  });
}

function updateChips(idx) {
  SCENES.forEach((_, i) => {
    const chip = document.getElementById(`chip-${i}`);
    if (!chip) return;
    chip.classList.remove('active', 'done');
    if (i < idx) chip.classList.add('done');
    else if (i === idx) chip.classList.add('active');
  });
}

// ─── Progress ─────────────────────────────────────────────────────────────────
function updateProgress(sceneIdx) {
  const pct = Math.round((sceneIdx / SCENES.length) * 100);
  progressFill.style.width = pct + '%';
  sceneLabel.textContent = `Scene ${sceneIdx + 1} of ${SCENES.length}`;
  pctLabel.textContent = pct + '%';
  updateChips(sceneIdx);
}

// ─── Message Builders ─────────────────────────────────────────────────────────
function buildRahul(text) {
  const wrap = document.createElement('div');
  wrap.className = 'bubble-rahul';
  wrap.innerHTML = `
    <div class="avatar">👦</div>
    <div>
      <div class="name">Rahul</div>
      <div class="msg">${text}</div>
    </div>
  `;
  return wrap;
}

function buildPriya(text) {
  const wrap = document.createElement('div');
  wrap.className = 'bubble-priya';
  wrap.innerHTML = `
    <div class="avatar">👧</div>
    <div>
      <div class="name">Priya — Healthcare Ops Specialist</div>
      <div class="msg">${text}</div>
    </div>
  `;
  return wrap;
}

function buildNarrator(text) {
  const el = document.createElement('div');
  el.className = 'narrator';
  el.textContent = text;
  return el;
}

function buildSceneTitle(scene) {
  const el = document.createElement('div');
  el.className = 'scene-title';
  el.innerHTML = `
    <div class="label">Scene ${scene.id} of ${SCENES.length}</div>
    <div class="title">${scene.emoji} ${scene.title}</div>
  `;
  return el;
}

function buildInfoCard(text, variant = 'amber') {
  const el = document.createElement('div');
  el.className = `info-card ${variant}`;
  el.style.whiteSpace = 'pre-line';
  el.textContent = text;
  return el;
}

function buildFlow(nodes) {
  const el = document.createElement('div');
  el.className = 'flow-diagram';
  nodes.forEach((node, i) => {
    if (node.endsWith('→')) {
      const arrow = document.createElement('div');
      arrow.className = 'flow-arrow';
      arrow.textContent = '→';
      el.appendChild(arrow);
    } else {
      const n = document.createElement('div');
      n.className = 'flow-node';
      n.style.whiteSpace = 'pre-line';
      n.textContent = node;
      el.appendChild(n);
    }
  });
  return el;
}

function buildNarratorIntro(text) {
  const el = document.createElement('div');
  el.className = 'narrator';
  el.style.fontStyle = 'normal';
  el.style.fontSize = '0.75rem';
  el.style.color = 'var(--accent-blue)';
  el.style.fontWeight = '600';
  el.style.letterSpacing = '0.08em';
  el.style.textTransform = 'uppercase';
  el.textContent = '📍 ' + text;
  return el;
}

function buildTyping() {
  const el = document.createElement('div');
  el.className = 'typing';
  el.innerHTML = '<span></span><span></span><span></span>';
  return el;
}

function buildElement(msg) {
  switch (msg.type) {
    case 'rahul': return buildRahul(msg.text);
    case 'priya': return buildPriya(msg.text);
    case 'narrator': return buildNarrator(msg.text);
    case 'infocard': return buildInfoCard(msg.text, msg.variant || 'amber');
    case 'flow': return buildFlow(msg.nodes);
    default: return null;
  }
}

// ─── Animate messages in sequence ────────────────────────────────────────────
async function appendMessagesAnimated(messages, onDone) {
  for (let i = 0; i < messages.length; i++) {
    const msg = messages[i];
    const el = buildElement(msg);
    if (!el) continue;

    // Show typing indicator briefly for chat bubbles
    if (msg.type === 'rahul' || msg.type === 'priya') {
      const typing = buildTyping();
      typing.style.alignSelf = msg.type === 'priya' ? 'flex-end' : 'flex-start';
      chatFeed.appendChild(typing);
      chatFeed.scrollTop = chatFeed.scrollHeight;
      await delay(600 + Math.random() * 400);
      chatFeed.removeChild(typing);
    } else {
      await delay(300);
    }

    el.style.opacity = '0';
    el.style.transform = 'translateY(8px)';
    el.style.transition = 'opacity 0.3s, transform 0.3s';
    chatFeed.appendChild(el);

    // Force reflow
    el.getBoundingClientRect();
    el.style.opacity = '1';
    el.style.transform = 'translateY(0)';
    chatFeed.scrollTop = chatFeed.scrollHeight;

    if (msg.type === 'rahul' || msg.type === 'priya') {
      const len = msg.text.length;
      await delay(Math.min(len * 18, 1800));
    } else {
      await delay(500);
    }
  }
  onDone();
}

function delay(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

// ─── Render choices ───────────────────────────────────────────────────────────
function renderChoices(scene, sceneIdx) {
  choicesGrid.innerHTML = '';

  if (scene.isEnd) {
    const btn = document.createElement('button');
    btn.className = 'restart-btn';
    btn.innerHTML = '🔄 Restart Story';
    btn.addEventListener('click', restartStory);
    choicesGrid.style.gridTemplateColumns = '1fr';
    choicesGrid.appendChild(btn);
    return;
  }

  choicesGrid.style.gridTemplateColumns = '1fr 1fr';

  scene.choices.forEach((choice, ci) => {
    const btn = document.createElement('button');
    btn.className = 'choice-btn';
    btn.innerHTML = `
      <span class="choice-icon">${choice.icon}</span>
      <span class="choice-text">${choice.text}</span>
    `;
    btn.addEventListener('click', () => handleChoice(sceneIdx, ci, btn));
    choicesGrid.appendChild(btn);
  });
}

// ─── Handle choice selection ──────────────────────────────────────────────────
async function handleChoice(sceneIdx, choiceIdx, btn) {
  if (isAnimating) return;
  isAnimating = true;

  // Disable all buttons
  choicesGrid.querySelectorAll('button').forEach(b => b.disabled = true);

  const key = `${sceneIdx + 1}-${choiceIdx + 1}`;
  const altMessages = ALT_RESPONSES[key] || [];

  if (altMessages.length) {
    await new Promise(resolve => {
      appendMessagesAnimated(altMessages, resolve);
    });
  }

  // Move to next scene
  const nextIdx = sceneIdx + 1;
  if (nextIdx < SCENES.length) {
    await delay(600);
    await renderScene(nextIdx);
  } else {
    isAnimating = false;
  }
}

// ─── Render a scene ───────────────────────────────────────────────────────────
async function renderScene(idx) {
  isAnimating = true;
  currentScene = idx;
  updateProgress(idx);

  const scene = SCENES[idx];

  // Scene title
  const titleEl = buildSceneTitle(scene);
  titleEl.style.opacity = '0';
  titleEl.style.transition = 'opacity 0.4s';
  chatFeed.appendChild(titleEl);
  chatFeed.scrollTop = chatFeed.scrollHeight;
  await delay(50);
  titleEl.style.opacity = '1';
  await delay(400);

  // Location narrator
  if (scene.intro) {
    const loc = buildNarratorIntro(scene.intro);
    loc.style.opacity = '0';
    loc.style.transition = 'opacity 0.3s';
    chatFeed.appendChild(loc);
    await delay(50);
    loc.style.opacity = '1';
    chatFeed.scrollTop = chatFeed.scrollHeight;
    await delay(400);
  }

  await new Promise(resolve => {
    appendMessagesAnimated(scene.messages, resolve);
  });

  renderChoices(scene, idx);
  isAnimating = false;
}

// ─── Restart ──────────────────────────────────────────────────────────────────
function restartStory() {
  chatFeed.innerHTML = '';
  choicesGrid.innerHTML = '';
  choicesGrid.style.gridTemplateColumns = '1fr 1fr';
  currentScene = 0;
  isAnimating = false;
  updateProgress(0);
  renderScene(0);
}

// ─── Init ─────────────────────────────────────────────────────────────────────
buildChips();
renderScene(0);
</script>
</body>
</html>
