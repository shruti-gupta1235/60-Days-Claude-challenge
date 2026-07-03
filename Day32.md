<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Think Like a Marketing Strategist</title>

<script src="https://unpkg.com/react@18/umd/react.development.js"></script>
<script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>

<style>
body{
margin:0;
font-family:Arial,sans-serif;
background:#0d1117;
color:white;
}

.container{
max-width:900px;
margin:auto;
padding:20px;
}

.card{
background:#161b22;
padding:20px;
border-radius:12px;
margin:20px 0;
box-shadow:0 0 10px rgba(0,255,255,.2);
}

button{
padding:10px 20px;
margin:8px;
border:none;
border-radius:8px;
cursor:pointer;
background:#00bcd4;
color:white;
font-size:16px;
}

button:hover{
background:#0097a7;
}

.progress{
height:10px;
background:#333;
border-radius:10px;
margin:20px 0;
}

.bar{
height:10px;
background:#00e5ff;
border-radius:10px;
}

h1,h2{
color:#00e5ff;
}

ul{
line-height:1.8;
}
</style>
</head>

<body>

<div id="root"></div>

<script type="text/babel">

const businesses=[
"Coffee Shop",
"Fitness Gym",
"Clothing Brand",
"Tech Startup",
"Bakery"
];

function App(){

const [step,setStep]=React.useState(0);
const [mode,setMode]=React.useState("");
const [business]=React.useState(
businesses[Math.floor(Math.random()*businesses.length)]
);

const progress=((step+1)/6)*100;

return(

<div className="container">

<h1>Think Like a Marketing Strategist</h1>

<div className="progress">
<div className="bar" style={{width:progress+"%"}}></div>
</div>

{step===0&&(

<div className="card">

<h2>Welcome</h2>

<p>
Learn how marketers think before creating campaigns.
</p>

<button onClick={()=>{setMode("Business");setStep(1)}}>🏢 Business</button>

<button onClick={()=>{setMode("Personal Brand");setStep(1)}}>🙋 Personal Brand</button>

<button onClick={()=>{setMode("Random Client");setStep(1)}}>🎲 Random Client</button>

</div>

)}

{step===1&&(

<div className="card">

<h2>{mode}</h2>

<p>
Business: <b>{business}</b>
</p>

<h3>Target Audience</h3>

<p>
Young professionals looking for quality and affordable products.
</p>

<button onClick={()=>setStep(2)}>Next</button>

</div>

)}

{step===2&&(

<div className="card">

<h2>Choose Marketing Platforms</h2>

<ul>
<li>Facebook</li>
<li>Instagram</li>
<li>LinkedIn</li>
<li>YouTube</li>
<li>X (Twitter)</li>
</ul>

<p>
Instagram and YouTube are best for visual growth.
</p>

<button onClick={()=>setStep(3)}>Continue</button>

</div>

)}

{step===3&&(

<div className="card">

<h2>Content Pillars</h2>

<ul>
<li>Educational Content</li>
<li>Behind the Scenes</li>
<li>Customer Stories</li>
<li>Product Showcase</li>
<li>Industry Tips</li>
</ul>

<p>Select your best three pillars.</p>

<button onClick={()=>setStep(4)}>Generate Roadmap</button>

</div>

)}

{step===4&&(

<div className="card">

<h2>30-Day Marketing Roadmap</h2>

<ul>
<li>Week 1 – Build Awareness</li>
<li>Week 2 – Increase Engagement</li>
<li>Week 3 – Build Trust</li>
<li>Week 4 – Drive Sales</li>
</ul>

<button onClick={()=>setStep(5)}>Next</button>

</div>

)}

{step===5&&(

<div className="card">

<h2>Growth Report</h2>

<p>Audience Understanding : Excellent</p>

<p>Platform Strategy : Strong</p>

<p>Content Strategy : Good</p>

<p>Growth Potential : High</p>

<p>Best Decision : Choosing visual platforms.</p>

<p>Biggest Mistake : Ignoring customer feedback.</p>

<h3>Marketing Lessons</h3>

<ul>
<li>Know your audience.</li>
<li>Stay consistent.</li>
<li>Create valuable content.</li>
</ul>

<h3>Claude Prompt</h3>

<p>
"Act as a marketing strategist and create a 30-day marketing plan for my business."
</p>

<button onClick={()=>window.location.reload()}>
Play Again
</button>

</div>

)}

</div>

);

}

ReactDOM.createRoot(document.getElementById("root")).render(<App/>);

</script>

</body>
</html>
