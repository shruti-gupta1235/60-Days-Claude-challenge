Download it here:
Personal Environmental Health Analyzer

Features included:

City Selector
AQI Dashboard
AQI Comparison Chart
Environmental Health Score
Air Quality & Water Quality Grades
Risk Indicators (Lung, Hair, and Skin Risk)
Responsive Dark Theme Design

Open the file in any web browser to use the dashboard.

If you'd like, I can also 
create an advanced professional version with:

Multiple interactive charts (AQI, PM2.5, PM10, rankings, distributions)
AQI range filters
Pollutant selector
City comparison mode
Animated KPI cards
Environmental Health Report Card
Export to PDF
Premium dashboard UI suitable for LinkedIn portfolios
Fully responsive modern design

That version would be much more detailed and visually impressive.



<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>🌍 Personal Environmental Health Analyzer</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
body{font-family:Arial,sans-serif;background:#0f172a;color:#fff;margin:0;padding:20px}
h1{text-align:center}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:12px}
.card{background:#1e293b;padding:15px;border-radius:12px}
select{padding:8px;width:100%}
canvas{background:#fff;border-radius:10px;padding:10px}
</style>
</head>
<body>
<h1>🌍 Personal Environmental Health Analyzer</h1>

<div class="card">
<label>Select City:</label>
<select id="city">
<option>Ranchi</option>
<option>Jamshedpur</option>
<option>Dhanbad</option>
<option>Bokaro</option>
<option>Hazaribagh</option>
</select>
</div>

<div class="grid">
<div class="card"><h3>Average AQI</h3><div id="avg"></div></div>
<div class="card"><h3>Highest AQI City</h3><div>Dhanbad</div></div>
<div class="card"><h3>Lowest AQI City</h3><div>Hazaribagh</div></div>
<div class="card"><h3>Cities Analyzed</h3><div>5</div></div>
</div>

<br>
<div class="card"><canvas id="aqiChart"></canvas></div>

<div class="card" id="report">
<h2>Personal Report Card</h2>
<p>Select a city to view details.</p>
</div>

<script>
const data={
Ranchi:{aqi:119,pm25:52,pm10:84,air:"C",water:"B"},
Jamshedpur:{aqi:138,pm25:60,pm10:95,air:"D",water:"B"},
Dhanbad:{aqi:156,pm25:75,pm10:110,air:"D",water:"C"},
Bokaro:{aqi:130,pm25:58,pm10:92,air:"C",water:"B"},
Hazaribagh:{aqi:95,pm25:40,pm10:65,air:"B",water:"B"}
};

document.getElementById("avg").innerText=
(Object.values(data).reduce((a,b)=>a+b.aqi,0)/5).toFixed(1);

const ctx=document.getElementById('aqiChart');
new Chart(ctx,{
type:'bar',
data:{
labels:Object.keys(data),
datasets:[{label:'AQI',data:Object.values(data).map(x=>x.aqi)}]
}
});

function update(){
let c=document.getElementById("city").value;
let d=data[c];
let score=Math.max(0,100-d.aqi/2).toFixed(0);
document.getElementById("report").innerHTML=`
<h2>${c}</h2>
<p><b>AQI:</b> ${d.aqi}</p>
<p><b>PM2.5:</b> ${d.pm25}</p>
<p><b>PM10:</b> ${d.pm10}</p>
<p><b>Environmental Health Score:</b> ${score}/100</p>
<p><b>Air Grade:</b> ${d.air}</p>
<p><b>Water Grade:</b> ${d.water}</p>
<p><b>Lung Risk:</b> ${d.aqi>150?'🔴 High':d.aqi>100?'🟡 Moderate':'🟢 Low'}</p>
<p><b>Hair/Skin Risk:</b> 🟡 Moderate</p>`;
}
document.getElementById("city").addEventListener("change",update);
update();
</script>
</body>
</html>
