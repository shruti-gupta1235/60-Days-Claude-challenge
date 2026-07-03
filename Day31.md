<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>AI Supply Chain Control Tower</title>

<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:Arial,sans-serif;}
body{
background:#08131f;
color:white;
padding:20px;
}
h1{
text-align:center;
color:#00d4ff;
margin-bottom:20px;
}
.dashboard{
display:grid;
grid-template-columns:repeat(auto-fit,minmax(180px,1fr));
gap:15px;
margin-bottom:20px;
}
.card{
background:#132433;
padding:15px;
border-radius:10px;
box-shadow:0 0 15px rgba(0,255,255,.2);
transition:.3s;
}
.card:hover{
transform:scale(1.05);
}
.value{
font-size:28px;
margin-top:8px;
color:#00ffff;
}
.controls{
display:flex;
gap:10px;
margin:20px 0;
}
button{
padding:10px 20px;
border:none;
border-radius:8px;
cursor:pointer;
font-weight:bold;
}
#start{
background:#00bcd4;
color:white;
}
#pause{
background:orange;
color:white;
}
#sound{
background:#444;
color:white;
}
#alerts{
display:grid;
gap:15px;
}
.alert{
background:#142638;
padding:15px;
border-left:6px solid red;
border-radius:10px;
animation:pulse 1s infinite;
}
.alert.high{border-color:red;}
.alert.medium{border-color:orange;}
.alert.low{border-color:green;}
.actions{
margin-top:10px;
display:flex;
flex-wrap:wrap;
gap:10px;
}
.actions button{
background:#2196f3;
color:white;
}
.log{
margin-top:20px;
background:#111;
padding:10px;
height:180px;
overflow:auto;
border-radius:10px;
}
@keyframes pulse{
50%{box-shadow:0 0 20px red;}
}
#result{
display:none;
margin-top:20px;
padding:20px;
background:#102030;
border-radius:10px;
}
</style>

</head>
<body>

<h1>AI Supply Chain Control Tower</h1>

<div class="dashboard">

<div class="card">
Service Level
<div class="value" id="service">95%</div>
</div>

<div class="card">
Customer Satisfaction
<div class="value" id="customer">90%</div>
</div>

<div class="card">
Inventory Health
<div class="value" id="inventory">88%</div>
</div>

<div class="card">
Transport Efficiency
<div class="value" id="transport">92%</div>
</div>

<div class="card">
Operating Cost
<div class="value" id="cost">$120K</div>
</div>

<div class="card">
Revenue Protected
<div class="value" id="revenue">$2M</div>
</div>

<div class="card">
Score
<div class="value" id="score">0</div>
</div>

<div class="card">
Time
<div class="value" id="time">180</div>
</div>

</div>

<div class="controls">
<button id="start">Start</button>
<button id="pause">Pause</button>
<button id="sound">Sound ON</button>
</div>

<h2>Operational Alerts</h2>

<div id="alerts"></div>

<h2 style="margin-top:20px;">Event Log</h2>

<div class="log" id="log"></div>

<div id="result">
<h2>Simulation Complete</h2>
<p id="finalScore"></p>
<p id="grade"></p>
<button onclick="location.reload()">Play Again</button>
</div>

<script>

let score=0;
let timer=180;
let interval;
let paused=false;

const alertTypes=[
{
title:"🚨 Port Congestion",
priority:"high",
impact:"Shipment delays"
},
{
title:"🚨 Supplier Delay",
priority:"high",
impact:"Production risk"
},
{
title:"🚨 Truck Breakdown",
priority:"medium",
impact:"Late delivery"
},
{
title:"🚨 Warehouse Out of Stock",
priority:"high",
impact:"Lost sales"
},
{
title:"🚨 Customs Inspection",
priority:"medium",
impact:"Import delay"
},
{
title:"🚨 Demand Spike",
priority:"medium",
impact:"Inventory pressure"
},
{
title:"🚨 Factory Machine Failure",
priority:"high",
impact:"Production stop"
},
{
title:"🚨 Weather Disruption",
priority:"medium",
impact:"Transport delay"
},
{
title:"🚨 Wrong Inventory Count",
priority:"low",
impact:"Planning issue"
},
{
title:"🚨 Damaged Shipment",
priority:"medium",
impact:"Customer complaints"
}
];

function randomAlert(){

let a=alertTypes[Math.floor(Math.random()*alertTypes.length)];

let div=document.createElement("div");

div.className="alert "+a.priority;

div.innerHTML=`
<h3>${a.title}</h3>
<p>${a.impact}</p>

<div class="actions">
<button onclick="resolve(this,true)">Best Action</button>
<button onclick="resolve(this,false)">Ignore</button>
</div>
`;

document.getElementById("alerts").prepend(div);

}

function resolve(btn,good){

let card=btn.closest(".alert");

card.remove();

if(good){

score+=10;

log("Correct decision.");

}
else{

score-=5;

log("Wrong decision.");

}

document.getElementById("score").innerHTML=score;

}

function log(text){

let box=document.getElementById("log");

box.innerHTML+="<p>"+text+"</p>";

box.scrollTop=box.scrollHeight;

}

function startGame(){

interval=setInterval(()=>{

if(paused)return;

timer--;

document.getElementById("time").innerHTML=timer;

if(Math.random()>0.4){

randomAlert();

}

if(timer<=0){

clearInterval(interval);

endGame();

}

},1000);

}

function endGame(){

document.getElementById("alerts").innerHTML="";

document.getElementById("result").style.display="block";

document.getElementById("finalScore").innerHTML="Final Score : "+score;

let grade="D";

if(score>120)grade="A+";
else if(score>90)grade="A";
else if(score>60)grade="B";
else if(score>30)grade="C";

document.getElementById("grade").innerHTML="Performance Grade : "+grade;

}

document.getElementById("start").onclick=startGame;

document.getElementById("pause").onclick=()=>{

paused=!paused;

document.getElementById("pause").innerHTML=paused?"Resume":"Pause";

};

document.getElementById("sound").onclick=()=>{

let b=document.getElementById("sound");

b.innerHTML=b.innerHTML=="Sound ON"?"Sound OFF":"Sound ON";

};

</script>

</body>
</html>
