<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Cognitive Pattern Explorer</title>

<style>
*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:Arial,Helvetica,sans-serif;
}

body{
background:#eef5f7;
color:#24323d;
}

header{
background:#4f7c82;
color:white;
padding:20px;
text-align:center;
font-size:30px;
font-weight:bold;
}

.container{
max-width:1100px;
margin:auto;
padding:20px;
}

.card{
background:white;
padding:20px;
margin:20px 0;
border-radius:12px;
box-shadow:0 5px 15px rgba(0,0,0,.1);
}

button{
padding:12px 20px;
border:none;
background:#4f7c82;
color:white;
border-radius:8px;
cursor:pointer;
margin-top:15px;
}

button:hover{
background:#3c666d;
}

.progress{
height:12px;
background:#d7e5e7;
border-radius:20px;
overflow:hidden;
margin:20px 0;
}

.bar{
height:100%;
width:20%;
background:#4f7c82;
transition:.5s;
}

.grid{
display:grid;
grid-template-columns:repeat(auto-fit,minmax(220px,1fr));
gap:15px;
}

.item{
padding:15px;
background:#ddeef0;
border-radius:10px;
cursor:grab;
}

.timeline{
display:flex;
flex-wrap:wrap;
gap:10px;
margin-top:15px;
}

.timeline div{
padding:12px;
background:#bfdfe2;
border-radius:8px;
cursor:grab;
}

.report{
background:#edf8f4;
border-left:6px solid #4f7c82;
padding:20px;
margin-top:20px;
border-radius:10px;
}

footer{
text-align:center;
padding:20px;
opacity:.7;
}
</style>
</head>

<body>

<header>

🧠 Cognitive Pattern Explorer

</header>

<div class="container">

<div class="card">

<h2>Welcome</h2>

<p>

Explore your thinking habits through interactive activities.

This experience is educational only and is not a diagnosis.

</p>

<button onclick="setMode('Calm')">

Calm Mode

</button>

<button onclick="setMode('Stress')">

Stress Mode

</button>

</div>

<div class="progress">

<div class="bar" id="bar"></div>

</div>

<div class="card">

<h2>

Chapter 1

Discover Your Thinking Style

</h2>

<p id="question">

When a problem appears unexpectedly, what is your first reaction?

</p>

<div class="grid">

<button onclick="answer('analytical')">

Analyze everything carefully

</button>

<button onclick="answer('emotion')">

Trust my feelings

</button>

<button onclick="answer('action')">

Act immediately

</button>

<button onclick="answer('balanced')">

Pause then decide

</button>

</div>

</div>

<div class="card">

<h2>

Chapter 2

Choose Your Priorities

</h2>

<p>

(Drag-and-drop placeholder)

</p>

<div class="grid">

<div class="item" draggable="true">

Logic

</div>

<div class="item" draggable="true">

Speed

</div>

<div class="item" draggable="true">

Empathy

</div>

<div class="item" draggable="true">

Planning

</div>

</div>

</div>

<div class="card">

<h2>

Chapter 3

Thinking Timeline

</h2>

<p>

Arrange your thinking process.

</p>

<div class="timeline">

<div draggable="true">

Notice Problem

</div>

<div draggable="true">

Collect Information

</div>

<div draggable="true">

Reflect

</div>

<div draggable="true">

Take Action

</div>

</div>

</div>

<div class="report">

<h2>

Reflection Journal

</h2>

<p id="journal">

Complete activities to generate your reflection.

</p>

<h3>

Thinking Tendencies

</h3>

<ul>

<li>

Analytical Thinker : <span id="a">20%</span>

</li>

<li>

Emotional Intuitive : <span id="e">20%</span>

</li>

<li>

Overthinking Loop Style : <span id="o">20%</span>

</li>

<li>

Action-First Decision Maker : <span id="ac">20%</span>

</li>

<li>

Balanced Reflective Thinker : <span id="b">20%</span>

</li>

</ul>

</div>

</div>

<footer>

Educational Self Reflection Experience

</footer>

<script>

let progress=20;

let mode="Calm";

function setMode(m){

mode=m;

alert("Mode Selected: "+m);

}

function answer(type){

progress+=20;

if(progress>100)

progress=100;

document.getElementById("bar").style.width=progress+"%";

let text="";

switch(type){

case "analytical":

text="You often explore details before making decisions. This suggests a thoughtful analytical style.";

document.getElementById("a").innerHTML="42%";

break;

case "emotion":

text="You often trust emotional signals. This suggests intuitive reflection.";

document.getElementById("e").innerHTML="43%";

break;

case "action":

text="You often move quickly toward action before lengthy analysis.";

document.getElementById("ac").innerHTML="46%";

break;

default:

text="You often balance reflection with practical action.";

document.getElementById("b").innerHTML="45%";

}

document.getElementById("journal").innerHTML=text+" Current Mode: "+mode;

}

</script>

</body>
</html>
