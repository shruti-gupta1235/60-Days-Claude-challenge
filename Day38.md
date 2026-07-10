<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Typing Speed Studio</title>

<style>
*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:Segoe UI,sans-serif;
}

body{
background:#0f172a;
color:white;
display:flex;
justify-content:center;
padding:30px;
}

.container{
width:100%;
max-width:1000px;
}

h1{
font-size:42px;
text-align:center;
margin-bottom:10px;
}

.subtitle{
text-align:center;
opacity:.8;
margin-bottom:25px;
}

.panel{
background:#1e293b;
padding:20px;
border-radius:20px;
margin-bottom:20px;
}

select,button{
padding:10px 18px;
border:none;
border-radius:10px;
font-size:16px;
margin-right:10px;
}

button{
background:#06b6d4;
color:white;
cursor:pointer;
}

.stats{
display:grid;
grid-template-columns:repeat(4,1fr);
gap:15px;
margin-top:20px;
}

.box{
background:#334155;
padding:15px;
border-radius:15px;
text-align:center;
}

#text{
margin-top:25px;
line-height:2;
font-size:22px;
background:#111827;
padding:20px;
border-radius:15px;
min-height:140px;
}

.correct{
color:#22c55e;
}

.wrong{
color:#ef4444;
background:#450a0a;
}

.current{
background:#06b6d4;
color:black;
}

textarea{
width:100%;
height:140px;
margin-top:20px;
background:#0f172a;
color:white;
border:2px solid #334155;
padding:15px;
border-radius:15px;
font-size:18px;
resize:none;
outline:none;
}

.progress{
height:12px;
background:#334155;
border-radius:20px;
overflow:hidden;
margin-top:20px;
}

.bar{
height:100%;
width:0%;
background:#22c55e;
transition:.2s;
}

@media(max-width:700px){

.stats{
grid-template-columns:repeat(2,1fr);
}

}
</style>
</head>

<body>

<div class="container">

<h1>⌨ Typing Speed Studio</h1>

<div class="subtitle">
Programming Typing Practice
</div>

<div class="panel">

<select id="time">

<option value="15">15 Seconds</option>

<option value="30" selected>30 Seconds</option>

<option value="60">60 Seconds</option>

<option value="120">120 Seconds</option>

</select>

<button onclick="restart()">
Restart
</button>

<div class="stats">

<div class="box">
<h2 id="timer">30</h2>
Time
</div>

<div class="box">
<h2 id="wpm">0</h2>
WPM
</div>

<div class="box">
<h2 id="accuracy">100%</h2>
Accuracy
</div>

<div class="box">
<h2 id="mistakes">0</h2>
Mistakes
</div>

</div>

<div class="progress">
<div class="bar" id="bar"></div>
</div>

<div id="text"></div>

<textarea
id="input"
placeholder="Start typing here..."
></textarea>

</div>

</div>

<script>

const passages=[

`function hello(){
console.log("Hello World");
}`,

`const sum=(a,b)=>{
return a+b;
};`,

`let numbers=[1,2,3];
numbers.forEach(n=>{
console.log(n);
});`,

`SELECT * FROM users
WHERE age>18
ORDER BY name;`,

`<div class="card">
<h2>Typing Studio</h2>
</div>`

];

let currentText="";
let started=false;

let timer=30;

let interval;

function randomText(){

currentText=
passages[
Math.floor(
Math.random()*passages.length
)
];

showText();

}

function showText(){

let html="";

for(let i=0;i<currentText.length;i++){

html+=`<span>${currentText[i]}</span>`;

}

document.getElementById("text").innerHTML=html;

}

function restart(){

clearInterval(interval);

started=false;

timer=parseInt(
document.getElementById("time").value
);

document.getElementById("timer").innerHTML=timer;

document.getElementById("input").value="";

document.getElementById("wpm").innerHTML=0;

document.getElementById("accuracy").innerHTML="100%";

document.getElementById("mistakes").innerHTML=0;

document.getElementById("bar").style.width="0%";

randomText();

}

document
.getElementById("input")
.addEventListener("input",function(){

if(!started){

started=true;

interval=setInterval(runTimer,1000);

}

update();

});

function runTimer(){

timer--;

document.getElementById("timer").innerHTML=timer;

let percent=
100-
(timer/
parseInt(document.getElementById("time").value))*100;

document.getElementById("bar").style.width=
percent+"%";

if(timer<=0){

clearInterval(interval);

document
.getElementById("input")
.disabled=true;

alert("Session Complete!");

}

}

function update(){

let typed=
document.getElementById("input").value;

let spans=
document.querySelectorAll("#text span");

let correct=0;

let mistakes=0;

spans.forEach((span,index)=>{

let ch=typed[index];

span.className="";

if(ch==null){

return;

}

if(ch===span.innerText){

span.classList.add("correct");

correct++;

}else{

span.classList.add("wrong");

mistakes++;

}

});

document.getElementById("mistakes").innerHTML=
mistakes;

let accuracy=
typed.length==0?
100:
Math.round(correct/typed.length*100);

document.getElementById("accuracy").innerHTML=
accuracy+"%";

let elapsed=
parseInt(document.getElementById("time").value)-timer;

if(elapsed<1)elapsed=1;

let words=correct/5;

let wpm=
Math.round(words/(elapsed/60));

document.getElementById("wpm").innerHTML=
wpm;

}

restart();

</script>

</body>
</html>
