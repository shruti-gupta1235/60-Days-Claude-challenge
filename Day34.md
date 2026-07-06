<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Marketing Detective</title>

<script src="https://unpkg.com/react@18/umd/react.development.js"></script>
<script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>

<style>
*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:Arial,Helvetica,sans-serif;
}

body{
background:#10131a;
color:white;
}

header{
background:#181d29;
padding:20px;
text-align:center;
font-size:28px;
font-weight:bold;
letter-spacing:2px;
box-shadow:0 0 20px rgba(0,0,0,.5);
}

.container{
width:95%;
max-width:1200px;
margin:auto;
padding:20px;
}

.card{
background:#1f2635;
padding:20px;
margin-bottom:20px;
border-radius:12px;
box-shadow:0 0 10px rgba(0,0,0,.4);
}

button{
padding:12px 25px;
border:none;
border-radius:8px;
cursor:pointer;
font-size:16px;
margin-top:15px;
background:#ff8c00;
color:white;
transition:.3s;
}

button:hover{
transform:scale(1.05);
}

.grid{
display:grid;
grid-template-columns:repeat(auto-fit,minmax(250px,1fr));
gap:15px;
margin-top:20px;
}

.evidence{
background:#2d3548;
padding:15px;
border-radius:10px;
cursor:grab;
}

select{
padding:10px;
margin-top:10px;
font-size:16px;
border-radius:8px;
width:250px;
}

.report{
background:#0d3b25;
padding:15px;
border-radius:10px;
margin-top:20px;
}

footer{
text-align:center;
padding:20px;
opacity:.6;
}
</style>
</head>

<body>

<div id="root"></div>

<script type="text/babel">

const marketingCases=[

{
company:"Glow Coffee",
industry:"Cafe",
objective:"Increase weekend visitors",
audience:"College Students",
channels:"Instagram, Posters",
budget:"$10,000",
metrics:{
reach:"250K",
ctr:"0.4%",
engagement:"2%",
conversion:"0.5%",
sales:"-8%"
},
comments:[
"Offer was confusing",
"I didn't understand the campaign"
],
social:"Very high impressions but very low engagement",
mistake:"Wrong Target Audience",
clues:[
"Campaign shown mostly to working professionals",
"Students rarely saw the ads",
"Weekend timing mismatch"
],
explanation:"The campaign targeted the wrong audience.",
improvement:"Target students near campuses with Instagram Reels."
},

{
company:"FitLife",
industry:"Fitness",
objective:"Sell yearly memberships",
audience:"Working Professionals",
channels:"Facebook Ads",
budget:"$25000",
metrics:{
reach:"500K",
ctr:"1.1%",
engagement:"4%",
conversion:"0.4%",
sales:"Flat"
},
comments:[
"Price too high",
"No free trial"
],
social:"Average engagement",
mistake:"Poor Offer",
clues:[
"No introductory discount",
"Competitors offered free trials",
"High landing page exits"
],
explanation:"Users wanted to test before purchasing.",
improvement:"Offer 7-day free trial."
},

{
company:"EcoBottle",
industry:"E-commerce",
objective:"Increase online sales",
audience:"Young Adults",
channels:"Instagram & TikTok",
budget:"18000",
metrics:{
reach:"800K",
ctr:"2.3%",
engagement:"7%",
conversion:"0.2%",
sales:"Low"
},
comments:[
"Checkout too long",
"Website slow"
],
social:"Campaign viral",
mistake:"Poor Website Experience",
clues:[
"Slow checkout",
"Cart abandonment 80%",
"Mobile optimization poor"
],
explanation:"Marketing worked but website failed.",
improvement:"Improve checkout speed."
}

];

function App(){

const[color,setColor]=React.useState("orange");
const[index,setIndex]=React.useState(0);
const solved=React.useState(false);

const data=marketingCases[index];

function nextCase(){
setIndex((index+1)%marketingCases.length);
}

const themes={
orange:"#ff8c00",
blue:"#2196f3",
green:"#2ecc71",
purple:"#8e44ad"
}

document.body.style.setProperty("--accent",themes[color]);

return(

<div>

<header style={{background:themes[color]}}>
🕵️ Marketing Detective
</header>

<div className="container">

<div className="card">

<h2>Select Theme</h2>

<select
value={color}
onChange={(e)=>setColor(e.target.value)}
>

<option value="orange">Claude Orange</option>
<option value="blue">Ocean Blue</option>
<option value="green">Emerald</option>
<option value="purple">Royal Purple</option>

</select>

</div>

<div className="card">

<h2>Case Assignment</h2>

<p><b>Company:</b> {data.company}</p>

<p><b>Industry:</b> {data.industry}</p>

<p><b>Objective:</b> {data.objective}</p>

<p><b>Audience:</b> {data.audience}</p>

<p><b>Channels:</b> {data.channels}</p>

<p><b>Budget:</b> {data.budget}</p>

<button>
Accept Investigation
</button>

</div>

<div className="card">

<h2>Investigation Board</h2>

<div className="grid">

<div className="evidence">
Reach<br/>
<b>{data.metrics.reach}</b>
</div>

<div className="evidence">
CTR<br/>
<b>{data.metrics.ctr}</b>
</div>

<div className="evidence">
Engagement<br/>
<b>{data.metrics.engagement}</b>
</div>

<div className="evidence">
Conversion<br/>
<b>{data.metrics.conversion}</b>
</div>

<div className="evidence">
Sales<br/>
<b>{data.metrics.sales}</b>
</div>

</div>

</div>

<div className="card">

<h2>Customer Comments</h2>

<ul>

{data.comments.map((c,i)=>

<li key={i}>{c}</li>

)}

</ul>

</div>

<div className="card">

<h2>Primary Marketing Mistake</h2>

<h3 style={{color:"#ff8c00"}}>

{data.mistake}

</h3>

<h2 style={{marginTop:"20px"}}>

Supporting Clues

</h2>

<ul>

{data.clues.map((c,i)=>

<li key={i}>{c}</li>

)}

</ul>

<div className="report">

<h2>Expert Verdict</h2>

<p>{data.explanation}</p>

<h3 style={{marginTop:"15px"}}>

Suggested Improvement

</h3>

<p>{data.improvement}</p>

</div>

<button onClick={nextCase}>

Replay Random Case

</button>

</div>

</div>

<footer>

Marketing Detective Demo

</footer>

</div>

)

}

ReactDOM.createRoot(document.getElementById("root")).render(<App/>);

</script>

</body>
</html>
