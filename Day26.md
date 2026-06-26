<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>PA Workflow Simulator</title>
<style>
body{font-family:Arial;background:#eaf4ff;color:#000;text-align:center}
.lane{width:30%;min-height:250px;border:2px solid #0077cc;
display:inline-block;vertical-align:top;margin:10px;padding:10px}
.card{background:#2196F3;color:#fff;padding:10px;margin:10px;
cursor:grab}
button{padding:10px;margin:10px}
</style>
</head>
<body>

<h2>Prior Authorization Workflow Simulator</h2>

<p>Progress: <span id="progress">Patient</span></p>
<p>Days: <span id="days">0</span></p>

<div class="lane" ondrop="drop(event)" ondragover="allowDrop(event)">
<h3>Patient</h3>
<div id="card" class="card" draggable="true"
ondragstart="drag(event)">
MRI Request
</div>
</div>

<div class="lane" ondrop="drop(event)" ondragover="allowDrop(event)">
<h3>Provider</h3>
</div>

<div class="lane" ondrop="drop(event)" ondragover="allowDrop(event)">
<h3>Payer</h3>
</div>

<button onclick="submitPA()">Submit PA</button>
<button onclick="location.reload()">Restart</button>

<h3 id="result"></h3>

<script>

function allowDrop(e){
e.preventDefault();
}

function drag(e){
e.dataTransfer.setData("text",e.target.id);
}

function drop(e){
e.preventDefault();
let data=e.dataTransfer.getData("text");
e.target.appendChild(document.getElementById(data));

document.getElementById("days").innerHTML++;

document.getElementById("progress").innerHTML=e.target.querySelector("h3").innerHTML;
}

function submitPA(){

let decision=Math.random()>0.5?"✅ Approved":"❌ Denied";

document.getElementById("result").innerHTML=
decision+"<br>Education: Prior Authorization ensures medical necessity before treatment.";

}

</script>

</body>
</html>
