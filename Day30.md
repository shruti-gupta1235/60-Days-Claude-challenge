<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Supply Chain Builder</title>

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
background:#0d1117;
color:#fff;
padding:30px;
}

.container{
max-width:1100px;
margin:auto;
}

.card{
background:#161b22;
padding:25px;
border-radius:18px;
margin-bottom:20px;
box-shadow:0 0 15px rgba(0,0,0,.4);
transition:.3s;
}

.card:hover{
transform:translateY(-4px);
}

button{
padding:12px 22px;
border:none;
border-radius:12px;
margin:8px;
cursor:pointer;
font-size:15px;
background:#238636;
color:white;
transition:.3s;
}

button:hover{
background:#2ea043;
}

.metric{
margin:12px 0;
}

.progress{
height:14px;
background:#333;
border-radius:20px;
overflow:hidden;
}

.fill{
height:100%;
background:#2ea043;
transition:width .5s;
}

h1,h2{
margin-bottom:15px;
}

p{
line-height:1.6;
margin-bottom:12px;
}

.optionBox{
display:flex;
flex-wrap:wrap;
}

.metricGrid{
display:grid;
grid-template-columns:repeat(auto-fit,minmax(220px,1fr));
gap:15px;
}
</style>
</head>

<body>

<div id="root"></div>

<script type="text/babel">

const companies=[
{
industry:"Electronics",
product:"Smartphones",
countries:"USA, India, Germany",
demand:"High"
},
{
industry:"Furniture",
product:"Office Chairs",
countries:"Canada, UK",
demand:"Medium"
},
{
industry:"Food",
product:"Organic Snacks",
countries:"India, UAE",
demand:"Very High"
},
{
industry:"Fashion",
product:"Sports Shoes",
countries:"Europe & Asia",
demand:"Seasonal"
}
];

function randomCompany(){
return companies[Math.floor(Math.random()*companies.length)];
}

function App(){

const [step,setStep]=React.useState(0);

const [company,setCompany]=React.useState(randomCompany());

const [metrics,setMetrics]=React.useState({
cost:50,
speed:50,
risk:50,
customer:50,
sustainability:50
});

const update=(changes)=>{
setMetrics(prev=>({
cost:Math.min(100,Math.max(0,prev.cost+(changes.cost||0))),
speed:Math.min(100,Math.max(0,prev.speed+(changes.speed||0))),
risk:Math.min(100,Math.max(0,prev.risk+(changes.risk||0))),
customer:Math.min(100,Math.max(0,prev.customer+(changes.customer||0))),
sustainability:Math.min(100,Math.max(0,prev.sustainability+(changes.sustainability||0)))
}));
setStep(step+1);
};

const score=Math.round(
(metrics.cost+
metrics.speed+
(100-metrics.risk)+
metrics.customer+
metrics.sustainability)/5
);

function Progress(name,value){
return(
<div className="metric">
<b>{name}: {value}</b>
<div className="progress">
<div className="fill" style={{width:value+"%"}}></div>
</div>
</div>
)
}

return(

<div className="container">

<div className="card">
<h1>Supply Chain Builder</h1>

<p>
A supply chain is the complete journey of a product from raw materials to the customer.
Good supply chain decisions reduce costs, improve delivery speed,
and increase customer satisfaction.
</p>

</div>

<div className="card">

<h2>Company Profile</h2>

<p><b>Industry:</b> {company.industry}</p>
<p><b>Product:</b> {company.product}</p>
<p><b>Countries Served:</b> {company.countries}</p>
<p><b>Demand:</b> {company.demand}</p>

</div>

<div className="card">

<h2>Live Business Metrics</h2>

{Progress("Cost",metrics.cost)}
{Progress("Delivery Speed",metrics.speed)}
{Progress("Risk",metrics.risk)}
{Progress("Customer Satisfaction",metrics.customer)}
{Progress("Sustainability",metrics.sustainability)}

</div>

{
step===0 &&

<div className="card">

<h2>Step 1 - Suppliers</h2>

<p>
Suppliers provide raw materials. Having one supplier is cheaper but risky.
Multiple suppliers reduce risk but cost more.
</p>

<div className="optionBox">

<button onClick={()=>update({
cost:-10,
risk:+20
})}>
Single Supplier
</button>

<button onClick={()=>update({
cost:+10,
risk:-20,
customer:+5
})}>
Multiple Suppliers
</button>

</div>

</div>

}

{
step===1 &&

<div className="card">

<h2>Step 2 - Factory Location</h2>

<p>
Choose where your products will be manufactured.
A nearby factory improves speed but costs more.
</p>

<button onClick={()=>update({
cost:+10,
speed:+15,
customer:+10
})}>
Local Factory
</button>

<button onClick={()=>update({
cost:-15,
speed:-10,
risk:+10
})}>
Overseas Factory
</button>

</div>

}

{
step===2 &&

<div className="card">

<h2>Step 3 - Warehouse Strategy</h2>

<p>
Warehouses store products before delivery.
More warehouses improve delivery speed but increase costs.
</p>

<button onClick={()=>update({
cost:+15,
speed:+20,
customer:+15
})}>
Multiple Warehouses
</button>

<button onClick={()=>update({
cost:-10,
speed:-10
})}>
Single Warehouse
</button>

</div>

}

{
step===3 &&

<div className="card">

<h2>Step 4 - Transportation</h2>

<p>
Transportation moves products to customers.
Air is fastest but expensive.
Sea is cheapest but slow.
</p>

<button onClick={()=>update({
cost:+20,
speed:+25,
customer:+15,
sustainability:-10
})}>
Air
</button>

<button onClick={()=>update({
cost:-10,
speed:-15,
sustainability:+15
})}>
Sea
</button>

<button onClick={()=>update({
speed:+10,
cost:+5,
sustainability:+10
})}>
Rail
</button>

<button onClick={()=>update({
speed:+5,
customer:+5
})}>
Road
</button>

</div>

}

{
step===4 &&

<div className="card">

<h2>Step 5 - Inventory Strategy</h2>

<p>
Inventory means how much stock you keep.
Too little stock risks shortages.
Too much stock increases storage costs.
</p>

<button onClick={()=>update({
cost:-10,
risk:+15
})}>
Low Inventory
</button>

<button onClick={()=>update({
customer:+10,
risk:-10
})}>
Balanced Inventory
</button>

<button onClick={()=>update({
cost:+15,
customer:+15,
risk:-15
})}>
High Inventory
</button>

</div>

}

{
step===5 &&

<div className="card">

<h2>Final Supply Chain Dashboard</h2>

<h1>Overall Score : {score}/100</h1>

<h3>Strengths</h3>

<ul>
<li>Customer Satisfaction : {metrics.customer}</li>
<li>Delivery Speed : {metrics.speed}</li>
<li>Sustainability : {metrics.sustainability}</li>
</ul>

<h3>Weaknesses</h3>

<ul>
<li>Risk : {metrics.risk}</li>
<li>Cost : {metrics.cost}</li>
</ul>

<h3>Recommendations</h3>

<ul>
<li>Use multiple suppliers to reduce dependency.</li>
<li>Balance inventory instead of keeping too much or too little.</li>
<li>Choose transport based on customer needs and cost.</li>
</ul>

<button onClick={()=>{
setCompany(randomCompany());
setMetrics({
cost:50,
speed:50,
risk:50,
customer:50,
sustainability:50
});
setStep(0);
}}>
Replay
</button>

</div>

}

</div>

);

}

ReactDOM.createRoot(document.getElementById("root")).render(<App/>);

</script>

</body>
</html>
