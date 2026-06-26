<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>AI Shark Tank Simulator</title>
<style>
body{font-family:Arial;background:#111;color:#fff;text-align:center;padding:20px}
input,textarea{width:80%;padding:8px;margin:5px}
button{padding:10px 20px;background:#00b894;color:#fff;border:none;cursor:pointer}
.card{background:#222;padding:15px;margin:15px;border-radius:10px}
</style>
</head>
<body>

<h1>🦈 AI Shark Tank Simulator</h1>

<input id="name" placeholder="Startup Name"><br>
<textarea id="problem" placeholder="Problem Statement"></textarea><br>
<textarea id="solution" placeholder="Solution"></textarea><br>
<input id="revenue" placeholder="Revenue Model"><br>
<input id="audience" placeholder="Target Audience"><br>
<input id="funding" placeholder="Funding Ask"><br>

<button onclick="start()">Start Pitch</button>

<div id="output"></div>

<script>
function start(){

let startup=document.getElementById("name").value;

let judges=[
"🦈 VC: How big is your market?",
"🦈 Founder: Why will customers choose you?",
"🦈 Customer: Why should I use this product?",
"🦈 Angel: When will you become profitable?"
];

let score=Math.floor(Math.random()*21)+80;

let decision;

if(score>=90)
decision="🟢 INVEST";
else if(score>=80)
decision="🟡 COME BACK LATER";
else
decision="🔴 REJECT";

document.getElementById("output").innerHTML=`
<div class="card">
<h2>${startup}</h2>

<h3>Investor Questions</h3>

<p>${judges[0]}</p>
<p>${judges[1]}</p>
<p>${judges[2]}</p>
<p>${judges[3]}</p>

<h3>Score: ${score}/100</h3>

<p><b>Market:</b> ${score-2}</p>
<p><b>Innovation:</b> ${score-5}</p>
<p><b>Business:</b> ${score-4}</p>
<p><b>Execution:</b> ${score-3}</p>

<h2>${decision}</h2>

<p><b>Suggested Valuation:</b> ₹5 Crore</p>

<p><b>Funding:</b> ₹50 Lakhs</p>

</div>
`;

}
</script>

</body>
</html>
