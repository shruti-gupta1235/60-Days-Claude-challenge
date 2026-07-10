<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>PDF Splitter & Merger</title>

<style>
*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:Segoe UI,Arial,sans-serif;
}

body{
background:linear-gradient(135deg,#0f172a,#1e293b);
color:#fff;
padding:25px;
min-height:100vh;
}

.container{
max-width:1100px;
margin:auto;
}

h1{
text-align:center;
font-size:40px;
margin-bottom:10px;
}

.subtitle{
text-align:center;
opacity:.8;
margin-bottom:25px;
}

.tabs{
display:flex;
gap:10px;
margin-bottom:20px;
}

.tab{
flex:1;
padding:15px;
border:none;
border-radius:12px;
cursor:pointer;
font-size:17px;
background:#1e40af;
color:#fff;
transition:.3s;
}

.tab.active{
background:#0ea5e9;
}

.card{
background:rgba(255,255,255,.08);
backdrop-filter:blur(15px);
padding:20px;
border-radius:18px;
margin-bottom:20px;
}

.drop-zone{
border:2px dashed #38bdf8;
padding:40px;
text-align:center;
border-radius:15px;
transition:.3s;
cursor:pointer;
}

.drop-zone.drag{
background:#1e3a8a;
}

input[type=file]{
display:none;
}

button{
padding:12px 20px;
border:none;
border-radius:10px;
background:#22c55e;
color:#fff;
cursor:pointer;
margin-top:15px;
}

button:hover{
transform:scale(1.03);
}

#fileList div{
padding:12px;
background:#1e293b;
border-radius:10px;
margin-top:10px;
display:flex;
justify-content:space-between;
}

.rangeBox{
margin-top:20px;
display:flex;
gap:10px;
}

.rangeBox input{
flex:1;
padding:12px;
border-radius:10px;
border:none;
}

.progress{
height:10px;
background:#334155;
border-radius:20px;
margin-top:20px;
overflow:hidden;
}

.bar{
height:100%;
width:0%;
background:#22c55e;
transition:.3s;
}

.hidden{
display:none;
}

.loading{
margin-top:15px;
font-size:15px;
color:#38bdf8;
}

.footer{
text-align:center;
margin-top:25px;
opacity:.7;
}

@media(max-width:700px){

.tabs{
flex-direction:column;
}

.rangeBox{
flex-direction:column;
}

}
</style>

</head>

<body>

<div class="container">

<h1>📄 PDF Splitter & Merger</h1>

<p class="subtitle">
Offline Demo UI
</p>

<div class="tabs">

<button class="tab active" onclick="showTab('split')">
Split PDF
</button>

<button class="tab" onclick="showTab('merge')">
Merge PDF
</button>

</div>

<div id="split" class="card">

<div class="drop-zone" id="drop">

<p>📂 Drop PDF Here</p>

<p>or Click to Upload</p>

<input type="file" id="file" accept=".pdf">

</div>

<div id="fileList"></div>

<div class="rangeBox">

<input
placeholder="Example: 1-5"
/>

<input
placeholder="Every N Pages"
/>

</div>

<button onclick="processSplit()">
Split PDF
</button>

<div class="progress">

<div class="bar" id="bar"></div>

</div>

<div class="loading" id="status"></div>

</div>

<div id="merge" class="card hidden">

<div class="drop-zone">

<p>📂 Upload Multiple PDFs</p>

<input
type="file"
multiple
accept=".pdf">

</div>

<div id="mergeList">

</div>

<button onclick="mergePDF()">
Merge PDFs
</button>

</div>

<p class="footer">
Demo Interface • PDF Processing Requires PDF.js / pdf-lib
</p>

</div>

<script>

let tabs=document.querySelectorAll(".tab");

function showTab(name){

document
.getElementById("split")
.classList.add("hidden");

document
.getElementById("merge")
.classList.add("hidden");

document
.getElementById(name)
.classList.remove("hidden");

tabs.forEach(t=>t.classList.remove("active"));

event.target.classList.add("active");

}

let drop=document.getElementById("drop");

let input=document.getElementById("file");

drop.onclick=()=>input.click();

drop.addEventListener("dragover",e=>{

e.preventDefault();

drop.classList.add("drag");

});

drop.addEventListener("dragleave",()=>{

drop.classList.remove("drag");

});

drop.addEventListener("drop",e=>{

e.preventDefault();

drop.classList.remove("drag");

loadFiles(e.dataTransfer.files);

});

input.addEventListener("change",()=>{

loadFiles(input.files);

});

function loadFiles(files){

let list=document.getElementById("fileList");

list.innerHTML="";

for(let f of files){

let div=document.createElement("div");

div.innerHTML=
`
<span>${f.name}</span>
<span>${(f.size/1024).toFixed(1)} KB</span>
`;

list.appendChild(div);

}

}

function processSplit(){

let bar=document.getElementById("bar");

let status=document.getElementById("status");

let p=0;

status.innerHTML="Processing...";

let timer=setInterval(()=>{

p+=10;

bar.style.width=p+"%";

if(p>=100){

clearInterval(timer);

status.innerHTML="✅ Demo Complete (PDF library required for actual splitting).";

}

},150);

}

function mergePDF(){

alert(
"Demo Merge Complete.\nReal merging requires PDF.js / pdf-lib."
);

}

</script>

</body>
</html>
