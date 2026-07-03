<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Media Integrity Analyzer</title>

<style>
body{
margin:0;
font-family:Arial,sans-serif;
background:#111;
color:white;
transition:.4s;
}
.container{
max-width:900px;
margin:auto;
padding:20px;
}
.card{
background:#1b1b1b;
padding:20px;
margin:20px 0;
border-radius:12px;
box-shadow:0 0 15px rgba(0,255,255,.2);
}
button{
padding:10px 18px;
margin:8px;
border:none;
border-radius:8px;
cursor:pointer;
font-size:15px;
}
.orange{background:#ff7a00;color:white;}
.blue{background:#00bcd4;color:white;}
.green{background:#4caf50;color:white;}
.metric{
display:inline-block;
margin:10px;
padding:15px;
background:#222;
border-radius:10px;
min-width:150px;
text-align:center;
}
.progress{
height:8px;
background:#333;
border-radius:8px;
overflow:hidden;
margin-bottom:20px;
}
.bar{
height:8px;
width:25%;
background:#00e5ff;
transition:.4s;
}
</style>
</head>

<body>

<div class="container">

<h1>Media Integrity Analyzer</h1>

<div class="progress">
<div class="bar" id="bar"></div>
</div>

<div class="card" id="theme">

<h2>Choose Theme</h2>

<button class="orange" onclick="theme('#ff7a00')">Claude Orange</button>

<button class="blue" onclick="theme('#00bcd4')">Ocean Blue</button>

<button class="green" onclick="theme('#4caf50')">Forest Green</button>

</div>

<div class="card">

<h2>Live Media Integrity Metrics</h2>

<div class="metric">
Headline Accuracy
<h3 id="headlineScore">50%</h3>
</div>

<div class="metric">
Source Reliability
<h3 id="sourceScore">60%</h3>
</div>

<div class="metric">
Emotional Manipulation
<h3 id="emotionScore">40%</h3>
</div>

<div class="metric">
Audience Targeting
<h3 id="audienceScore">55%</h3>
</div>

</div>

<div class="card" id="challenge">

<h2>Challenge 1: Headline Detective</h2>

<p><b>What is this?</b></p>

<p>Headlines often exaggerate facts to attract attention. Learning to notice misleading words helps you become a smarter reader.</p>

<h3 id="headline">
Scientists Shocked After One Fruit "Completely Cures" Stress Overnight!
</h3>

<p>Would you click this?</p>

<button onclick="answer()">Yes</button>
<button onclick="answer()">Maybe</button>
<button onclick="answer()">No</button>

<div id="result"></div>

</div>

</div>

<script>

function theme(color){
document.querySelector(".bar").style.background=color;
}

function answer(){

document.getElementById("headlineScore").innerHTML="88%";
document.getElementById("sourceScore").innerHTML="82%";
document.getElementById("emotionScore").innerHTML="18%";
document.getElementById("audienceScore").innerHTML="74%";

document.getElementById("bar").style.width="60%";

document.getElementById("result").innerHTML=`
<hr>
<h3>Headline Analysis</h3>

<p><b>Misleading Words:</b> "Shocked", "Completely", "Overnight"</p>

<p><b>Headline Accuracy Score:</b> 88%</p>

<p><b>Fair Rewrite:</b></p>

<p>"Study suggests one fruit may help reduce stress."</p>

<p><b>Key Takeaway:</b></p>

<p>Be careful when headlines use emotional or absolute language.</p>

<hr>

<h2>Challenge 2: Emotion Detector</h2>

<p>
"Only smart people know this hidden trick. Everyone else is missing out!"
</p>

<p><b>Manipulation Technique:</b> Fear of Missing Out (FOMO)</p>

<p><b>Target Audience:</b> Curious social media users</p>

<p><b>Neutral Rewrite:</b></p>

<p>
"This article explains a useful productivity tip."
</p>

<hr>

<h2>Media Integrity Dashboard</h2>

<p><b>Overall Score:</b> 81%</p>

<p><b>You Learned:</b></p>

<ul>
<li>Question emotional headlines.</li>
<li>Look for evidence before believing claims.</li>
<li>Notice persuasive language.</li>
</ul>

<p><b>Biggest Red Flag:</b></p>

<p>Extreme words designed to trigger emotion.</p>

<button onclick="location.reload()">Replay</button>

`;

}

</script>

</body>
</html>
