<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0">
<title>AI Assistant Builder</title>

<style>
*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:Segoe UI,sans-serif;
}

body{
background:#0f172a;
color:#fff;
display:flex;
justify-content:center;
padding:30px;
}

.container{
width:100%;
max-width:1100px;
}

header{
text-align:center;
margin-bottom:25px;
}

h1{
font-size:42px;
}

p{
opacity:.8;
margin-top:10px;
}

.card{
background:#1e293b;
border-radius:20px;
padding:20px;
margin-bottom:20px;
box-shadow:0 0 20px rgba(0,0,0,.3);
}

.grid{
display:grid;
grid-template-columns:1fr 1fr;
gap:20px;
}

label{
display:block;
margin-top:15px;
margin-bottom:8px;
font-weight:bold;
}

input,select,textarea{
width:100%;
padding:12px;
border:none;
border-radius:12px;
background:#0f172a;
color:white;
}

textarea{
height:180px;
resize:none;
}

button{
margin-top:20px;
padding:12px 20px;
background:#06b6d4;
border:none;
border-radius:10px;
color:white;
cursor:pointer;
font-size:16px;
}

button:hover{
background:#0891b2;
}

.output{
background:#111827;
padding:20px;
border-radius:15px;
min-height:220px;
white-space:pre-wrap;
line-height:1.6;
}

.loading{
display:none;
margin-top:15px;
color:#38bdf8;
}

footer{
text-align:center;
opacity:.7;
margin-top:25px;
}

@media(max-width:800px){

.grid{
grid-template-columns:1fr;
}

}
</style>
</head>

<body>

<div class="container">

<header>

<h1>🤖 AI Assistant Builder</h1>

<p>Create and test your own AI assistant interface.</p>

</header>

<div class="grid">

<div class="card">

<label>Assistant Name</label>

<input id="name" value="Productivity Coach">

<label>Domain</label>

<select id="domain">

<option>Productivity</option>

<option>Education</option>

<option>Business</option>

<option>Healthcare</option>

<option>Programming</option>

</select>

<label>User Input</label>

<textarea id="prompt" placeholder="Ask your assistant something..."></textarea>

<button onclick="generate()">

Generate Response

</button>

<div class="loading" id="loading">

Thinking...

</div>

</div>

<div class="card">

<h2>Assistant Response</h2>

<div class="output" id="output">

Your assistant response will appear here.

</div>

</div>

</div>

<div class="card">

<h2>System Prompt</h2>

<div class="output">

You are an expert AI assistant.

Goals:
• Understand the user's request.
• Ask clarifying questions when needed.
• Provide accurate and structured answers.
• Never invent facts.
• Clearly explain reasoning.
• Be friendly and professional.
• Suggest next steps where appropriate.

</div>

</div>

<footer>

Premium AI Assistant Builder Demo

</footer>

</div>

<script>

function generate(){

let loading=document.getElementById("loading");

let output=document.getElementById("output");

loading.style.display="block";

output.innerHTML="";

setTimeout(()=>{

loading.style.display="none";

let assistant=document.getElementById("name").value;

let domain=document.getElementById("domain").value;

let question=document.getElementById("prompt").value.trim();

if(question===""){

output.innerHTML="Please enter a question.";

return;

}

output.innerHTML=

"Assistant: "+assistant+

"\n\nDomain: "+domain+

"\n\nUser Request:\n"+question+

"\n\nSuggested Response:\n"+

"This is a demo response generated locally. " +

"In the complete version, this section would call the Claude API " +

"and return a live AI-generated answer based on the system prompt.";

},1200);

}

</script>

</body>
</html>
