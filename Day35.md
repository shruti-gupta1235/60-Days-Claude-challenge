<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0">
<title>🧩 Prompt Puzzle — Master AI Prompting Through Play</title>

<script src="https://unpkg.com/react@18/umd/react.development.js"></script>
<script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>

<style>

*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:Arial,sans-serif;
}

body{
background:#0f172a;
color:white;
}

header{
background:#1e293b;
padding:20px;
text-align:center;
font-size:30px;
font-weight:bold;
box-shadow:0 5px 15px rgba(0,0,0,.4);
}

.container{
width:95%;
max-width:1200px;
margin:auto;
padding:20px;
}

.card{
background:#1e293b;
padding:20px;
margin-bottom:20px;
border-radius:12px;
}

h2{
margin-bottom:10px;
}

button{
padding:12px 20px;
border:none;
border-radius:8px;
background:#3b82f6;
color:white;
cursor:pointer;
margin-top:15px;
font-size:16px;
}

button:hover{
background:#2563eb;
}

.grid{
display:grid;
grid-template-columns:repeat(auto-fit,minmax(250px,1fr));
gap:15px;
}

.block{

background:#334155;
padding:15px;
border-radius:10px;
cursor:grab;
transition:.3s;

}

.block:hover{

transform:scale(1.04);

}

.progress{

height:14px;
background:#374151;
border-radius:20px;
overflow:hidden;
margin-top:10px;

}

.progress span{

display:block;
height:100%;
width:40%;
background:#22c55e;

}

.score{

display:flex;
gap:20px;
flex-wrap:wrap;
margin-top:15px;

}

.badge{

background:#0ea5e9;
padding:10px 15px;
border-radius:8px;

}

.report{

background:#064e3b;
padding:20px;
border-radius:12px;

}

footer{

padding:20px;
text-align:center;
opacity:.6;

}

</style>

</head>

<body>

<div id="root"></div>

<script type="text/babel">

const scenarios=[

{

title:"Landing Page",

desired:"Create a modern responsive landing page.",

blocks:[

"Use HTML",

"Use CSS",

"Responsive Layout",

"Accessible Buttons",

"Clear Headings"

],

distractors:[

"Ignore Mobile",

"No Structure",

"Random Colors"

],

weakPrompt:"Make website.",

optimizedPrompt:"Create a responsive landing page using semantic HTML and CSS with accessibility and mobile support.",

overPrompt:"Create a futuristic landing page with 100 animations, 50 sections, every framework imaginable and infinite scrolling.",

weakOutput:"Simple page with poor layout.",

goodOutput:"Responsive accessible landing page.",

principle:"Be Specific"

},

{

title:"Portfolio Website",

desired:"Personal developer portfolio.",

blocks:[

"Hero Section",

"Projects",

"Contact Form",

"Responsive",

"Modern Design"

],

distractors:[

"Broken Links",

"No CSS",

"Remove Navigation"

],

weakPrompt:"Portfolio.",

optimizedPrompt:"Create a responsive developer portfolio with hero, projects and contact section.",

overPrompt:"Add 500 animations and every JavaScript library.",

weakOutput:"Plain webpage.",

goodOutput:"Professional portfolio.",

principle:"Clear Context"

}

];

function App(){

const[current,setCurrent]=React.useState(0);

const score=92;

const data=scenarios[current];

return(

<div>

<header>

🧩 Prompt Puzzle

</header>

<div className="container">

<div className="card">

<h2>Scenario</h2>

<p>

<b>

{data.title}

</b>

</p>

<p>

{data.desired}

</p>

<div className="progress">

<span></span>

</div>

</div>

<div className="card">

<h2>

Challenge 1

Build the Prompt

</h2>

<div className="grid">

{

data.blocks.map((b,i)=>

<div className="block" key={i}>

{b}

</div>

)

}

{

data.distractors.map((b,i)=>

<div className="block" key={"d"+i}>

{b}

</div>

)

}

</div>

</div>

<div className="card">

<h2>

Challenge 2

Clean the Prompt

</h2>

<p>

<b>Weak Prompt</b>

</p>

<p>

{data.weakPrompt}

</p>

<br/>

<p>

<b>Optimized Prompt</b>

</p>

<p>

{data.optimizedPrompt}

</p>

</div>

<div className="card">

<h2>

Challenge 3

Choose the Best Prompt

</h2>

<p>

<b>Best:</b>

{data.optimizedPrompt}

</p>

<p>

<b>Over Engineered:</b>

{data.overPrompt}

</p>

</div>

<div className="card">

<h2>

Prompt Performance

</h2>

<div className="score">

<div className="badge">

Accuracy 95%

</div>

<div className="badge">

Moves 12

</div>

<div className="badge">

Time 1:45

</div>

<div className="badge">

Hints 1

</div>

<div className="badge">

Bonus +20

</div>

</div>

</div>

<div className="report">

<h2>

Performance Report

</h2>

<p>

Prompt Score: {score}/100

</p>

<p>

Rating: Excellent

</p>

<p>

Rank: Prompt Apprentice

</p>

<p>

Prompt DNA

</p>

<pre>

██████████░░░░░

</pre>

<p>

Principle Learned:

{data.principle}

</p>

<p>

Weak AI Output:

{data.weakOutput}

</p>

<p>

Optimized AI Output:

{data.goodOutput}

</p>

<p>

Next Milestone:

Reach 100 Prompt Score.

</p>

<button onClick={()=>setCurrent((current+1)%scenarios.length)}>

Replay

</button>

</div>

</div>

<footer>

Offline Prompt Puzzle Demo

</footer>

</div>

)

}

ReactDOM.createRoot(document.getElementById("root")).render(<App/>)

</script>

</body>
</html>
