<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Task Compass</title>

<style>
*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:Arial,Helvetica,sans-serif;
}

body{
background:linear-gradient(135deg,#111827,#1e293b,#312e81);
color:white;
min-height:100vh;
padding:30px;
overflow-x:hidden;
}

.container{
max-width:1100px;
margin:auto;
}

h1{
font-size:42px;
margin-bottom:5px;
}

.subtitle{
opacity:.8;
margin-bottom:25px;
}

.card{
background:rgba(255,255,255,.08);
backdrop-filter:blur(12px);
border-radius:20px;
padding:20px;
margin-bottom:20px;
border:1px solid rgba(255,255,255,.12);
animation:fade .5s ease;
}

.progress{
height:14px;
background:#222;
border-radius:20px;
overflow:hidden;
margin-bottom:25px;
}

.bar{
height:100%;
width:0%;
background:linear-gradient(90deg,#06b6d4,#22c55e);
transition:.6s;
}

.ticket{
background:#0f172a;
padding:18px;
border-radius:15px;
font-size:18px;
margin:20px 0;
border-left:6px solid #22c55e;
}

.roles{
display:flex;
flex-wrap:wrap;
gap:15px;
margin-top:20px;
}

.role{
background:#1d4ed8;
padding:12px 18px;
border-radius:14px;
cursor:grab;
user-select:none;
transition:.3s;
}

.role:hover{
transform:translateY(-4px);
}

.drop{
margin-top:25px;
border:2px dashed white;
height:90px;
border-radius:15px;
display:flex;
align-items:center;
justify-content:center;
font-size:20px;
}

button{
padding:12px 22px;
margin-top:25px;
background:#22c55e;
border:none;
border-radius:10px;
cursor:pointer;
font-size:16px;
}

button:hover{
transform:scale(1.05);
}

.hidden{
display:none;
}

#result{
margin-top:20px;
line-height:1.7;
}

.stageTitle{
font-size:26px;
margin-bottom:15px;
color:#38bdf8;
}

@keyframes fade{
from{opacity:0;transform:translateY(20px);}
to{opacity:1;transform:none;}
}

@media(max-width:700px){
.roles{
flex-direction:column;
}
}
</style>
</head>

<body>

<div class="container">

<h1>🧭 Task Compass</h1>
<div class="subtitle">
Learn how work flows through real organizations.
</div>

<div class="progress">
<div class="bar" id="bar"></div>
</div>

<div class="card">

<div class="stageTitle">
Stage 1 — Who Owns This?
</div>

<div class="ticket" id="task"></div>

<div class="roles" id="roles"></div>

<div class="drop"
ondrop="drop(event)"
ondragover="allowDrop(event)"
id="dropZone">
Drop Owner Here
</div>

<button onclick="submitAnswer()">Submit</button>

<div id="result"></div>

</div>

</div>

<script>

const tasks=[
{
task:"Customer reports payments fail only on Android devices.",
owner:"Mobile Developer",
why:"The issue is isolated to the Android application.",
assist:"QA Engineer, Backend Developer, Product Manager"
},
{
task:"Users cannot reset their password.",
owner:"Backend Developer",
why:"Authentication APIs usually handle password reset logic.",
assist:"QA Engineer, Customer Support"
},
{
task:"New feature needs better user flow.",
owner:"UX Designer",
why:"Improving user experience is primarily owned by UX.",
assist:"Frontend Developer, Product Manager"
}
];

const roles=[
"Frontend Developer",
"Backend Developer",
"Mobile Developer",
"QA Engineer",
"Product Manager",
"UX Designer",
"Customer Support",
"Engineering Manager"
];

let current=0;
let selected="";

function loadTask(){

document.getElementById("task").innerHTML=tasks[current].task;

document.getElementById("roles").innerHTML="";

roles.forEach(r=>{

let d=document.createElement("div");

d.className="role";

d.innerHTML=r;

d.draggable=true;

d.id=r;

d.ondragstart=drag;

document.getElementById("roles").appendChild(d);

});

document.getElementById("dropZone").innerHTML="Drop Owner Here";

document.getElementById("result").innerHTML="";

document.getElementById("bar").style.width=((current)/tasks.length*100)+"%";

}

function allowDrop(e){
e.preventDefault();
}

function drag(e){
selected=e.target.id;
}

function drop(e){

e.preventDefault();

document.getElementById("dropZone").innerHTML=selected;

}

function submitAnswer(){

let t=tasks[current];

document.getElementById("result").innerHTML=

"<h3>Explanation</h3>"+

"<b>Primary Owner:</b> "+t.owner+

"<br><br><b>Why:</b> "+t.why+

"<br><br><b>Supporting Roles:</b> "+t.assist;

current++;

if(current<tasks.length){

setTimeout(loadTask,3000);

}else{

document.getElementById("bar").style.width="100%";

setTimeout(()=>{

document.querySelector(".card").innerHTML=`

<h2>✅ Stage 1 Complete</h2>

<p>You practiced identifying ownership instead of simply guessing job titles.</p>

<h3>Coming Next</h3>

<ul>

<li>Stage 2 – Task Routing</li>

<li>Stage 3 – Collaboration Challenge</li>

<li>Organizational Thinking Dashboard</li>

</ul>

`;

},3000);

}

}

loadTask();

</script>

</body>
</html>
