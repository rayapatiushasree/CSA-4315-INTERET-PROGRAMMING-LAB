<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>RailConnect - Train Reservation</title>
<style>
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:Arial,sans-serif;background:#f4f7fb;color:#182238}
header{background:#153d32;color:white;padding:22px 5%;
display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:10px}
nav{display:flex;gap:8px;flex-wrap:wrap;padding:14px 4%;
background:white;box-shadow:0 2px 8px #ddd}
button{border:0;border-radius:6px;padding:11px 14px;cursor:pointer;font-weight:bold}
nav button{background:#e5f2eb;color:#153d32}
.primary{background:#147d55;color:white}
.danger{background:#c62828;color:white}
.container{width:92%;max-width:1150px;margin:28px auto}
.page{display:none}.page.active{display:block}
.hero{background:linear-gradient(120deg,#153d32,#258461);color:white;
padding:36px;border-radius:14px;margin-bottom:22px}
.hero h2{font-size:32px;margin-bottom:12px}
.hero p{line-height:1.6}
.card{background:white;padding:21px;border-radius:12px;
box-shadow:0 3px 12px #0000000c;margin-bottom:18px}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:16px}
h2,h3{margin-bottom:14px}
label{font-weight:bold}
input,select,textarea{width:100%;padding:12px;margin:7px 0 15px;
border:1px solid #cbd5e1;border-radius:6px;font-size:15px}
.muted{color:#64748b;line-height:1.6}
.train-icon{font-size:40px;margin-bottom:12px}
.price{font-size:23px;font-weight:bold;color:#147d55;margin:12px 0}
.tag{display:inline-block;background:#e5f2eb;color:#153d32;padding:5px 8px;border-radius:5px;margin:5px 0}
table{width:100%;border-collapse:collapse;min-width:620px;background:white}
th,td{text-align:left;padding:11px;border-bottom:1px solid #e5e7eb}
th{background:#e5f2eb}
.table-wrap{overflow-x:auto}
.ticket{border:2px dashed #147d55;padding:22px;border-radius:10px;background:white}
footer{background:#153d32;color:white;text-align:center;padding:25px;margin-top:35px}
@media(max-width:600px){.container{width:95%}.hero{padding:24px}
.hero h2{font-size:25px}header{padding:18px}}
@media print{header,nav,footer,.no-print{display:none!important}
body{background:white}.container{width:100%;margin:0}.page{display:none!important}
#mytickets.page.active{display:block!important}.ticket{break-inside:avoid}}
</style>
</head>
<body>
<header>
<h1>🚆 RailConnect</h1>
<span>Simple Train Reservation System</span>
</header>

<nav>
<button onclick="show('home')">Home</button>
<button onclick="show('trains')">Search Trains</button>
<button onclick="show('account')">Login / Register</button>
<button onclick="show('mytickets')">My Tickets</button>
<button onclick="show('pnr')">Check PNR</button>
<button onclick="show('admin')">Admin Panel</button>
</nav>

<main class="container">
<section id="home" class="page active">
<div class="hero">
<h2>Your Journey Starts Here</h2>
<p>Search trains, explore travel classes, manage reservations,
and print your demo tickets from one place.</p>
</div>
<div class="card">
<h2>Find Trains</h2>
<form onsubmit="searchTrains(event)">
<label>Origin Station</label><input id="origin" placeholder="e.g. Chennai" required>
<label>Destination Station</label><input id="destination" placeholder="e.g. Bengaluru" required>
<div class="grid">
<div><label>Journey Date</label><input id="journeyDate" type="date" required></div>
<div><label>Travel Class</label>
<select id="searchClass">
<option value="all">All Classes</option>
<option>Sleeper</option><option>AC 3 Tier</option>
<option>AC 2 Tier</option><option>AC First Class</option>
</select></div>
</div>
<button class="primary">Search Trains</button>
</form>
<p id="searchMessage" class="muted"></p>
</div>
<div class="grid">
<div class="card"><h3>🛏️ Sleeper</h3><p class="muted">Budget-friendly overnight travel.</p></div>
<div class="card"><h3>❄️ AC 3 Tier</h3><p class="muted">Air-conditioned shared accommodation.</p></div>
<div class="card"><h3>🚆 AC 2 Tier</h3><p class="muted">Additional comfort and space.</p></div>
<div class="card"><h3>⭐ AC First Class</h3><p class="muted">Premium travel accommodation.</p></div>
</div>
</section>

<section id="trains" class="page">
<h2>Available Trains</h2>
<div id="trainList" class="grid"></div>
</section>

<section id="account" class="page">
<div class="grid">
<div class="card">
<h2>Register Passenger</h2>
<form onsubmit="register(event)">
<label>Full Name</label><input id="regName" required>
<label>Email</label><input id="regEmail" type="email" required>
<label>Password</label><input id="regPassword" type="password" minlength="6" required>
<button class="primary">Register</button>
</form>
</div>
<div class="card">
<h2>Passenger Login</h2>
<form onsubmit="login(event)">
<label>Email</label><input id="loginEmail" type="email" required>
<label>Password</label><input id="loginPassword" type="password" required>
<button class="primary">Login</button>
</form>
<p id="loginStatus" class="muted"></p>
<button onclick="logout()">Logout</button>
</div>
</div>
</section>

<section id="reservation" class="page">
<div class="card">
<h2>Passenger Details</h2>
<p id="selectedTrain" class="muted"></p>
<form onsubmit="bookTicket(event)">
<label>Passenger Name</label><input id="passengerName" required>
<label>Age</label><input id="passengerAge" type="number" min="1" max="120" required>
<label>Gender</label><select id="gender"><option>Male</option><option>Female</option><option>Other</option></select>
<label>Travel Class</label><select id="travelClass" onchange="updateFare()">
<option>Sleeper</option><option>AC 3 Tier</option>
<option>AC 2 Tier</option><option>AC First Class</option></select>
<label>Payment Method (demo)</label><select id="paymentMethod">
<option>Online Demo Payment</option><option>Offline / Pay at Counter (Demo)</option>
</select>
<p id="fareDetails" class="price"></p>
<button class="primary">Confirm Demo Reservation</button>
</form>
<p class="muted">No real payment will be processed.</p>
</div>
</section>

<section id="mytickets" class="page">
<div class="card">
<h2>My Tickets and Booking History</h2>
<p id="welcome" class="muted"></p>
<div id="ticketList"></div>
</div>
</section>

<section id="pnr" class="page">
<div class="card">
<h2>Check PNR Status</h2>
<form onsubmit="checkPNR(event)">
<label>Enter PNR Number</label><input id="pnrInput" required>
<button class="primary">Check Status</button>
</form>
<div id="pnrResult"></div>
</div>
</section>

<section id="admin" class="page">
<div class="card">
<h2>Administrator Login</h2>
<p class="muted">Demo credentials: admin@railconnect.com / admin123</p>
<form onsubmit="adminLogin(event)">
<label>Email</label><input id="adminEmail" type="email" required>
<label>Password</label><input id="adminPassword" type="password" required>
<button class="primary">Admin Login</button>
</form>
<p id="adminStatus" class="muted"></p>
</div>

<div id="adminPanel" style="display:none">
<div class="card">
<h2>Add Train</h2>
<form onsubmit="addTrain(event)">
<label>Train Name</label><input id="trainName" required>
<label>Train Number</label><input id="trainNumber" required>
<label>Origin</label><input id="trainOrigin" required>
<label>Destination</label><input id="trainDestination" required>
<label>Departure Time</label><input id="trainTime" type="time" required>
<label>Base Fare</label><input id="trainFare" type="number" min="1" required>
<label>Seat Capacity</label><input id="trainSeats" type="number" min="1" max="1000" value="50" required>
<button class="primary">Add Train</button>
</form>
</div>

<div class="card">
<h2>Manage Trains</h2>
<div class="table-wrap"><table>
<thead><tr><th>Number</th><th>Name</th><th>Route</th><th>Time</th><th>Base Fare</th><th>Action</th></tr></thead>
<tbody id="adminTrainRows"></tbody></table></div>
</div>

<div class="card">
<h2>Manage Reservations</h2>
<div class="table-wrap"><table>
<thead><tr><th>PNR</th><th>Passenger</th><th>Train</th><th>Date</th><th>Class</th><th>Status</th><th>Action</th></tr></thead>
<tbody id="adminBookingRows"></tbody></table></div>
</div>

<div class="card">
<h2>Reports</h2>
<p id="reportSummary"></p>
<button class="primary" onclick="downloadReport()">Download Booking CSV</button>
</div>
</div>
</section>
</main>
<footer><p>© 2026 RailConnect Reservation System</p><p style="margin-top:8px">Frontend demonstration project</p></footer>

<script>
const initialTrains=[
{id:1,number:"12601",name:"Chennai Express",origin:"Chennai",destination:"Bengaluru",time:"06:30",fare:450,capacity:50},
{id:2,number:"12603",name:"Southern Superfast",origin:"Chennai",destination:"Hyderabad",time:"19:00",fare:650,capacity:50},
{id:3,number:"12610",name:"City Intercity",origin:"Bengaluru",destination:"Chennai",time:"08:00",fare:400,capacity:50},
{id:4,number:"12615",name:"Coastal Express",origin:"Chennai",destination:"Tirupati",time:"07:30",fare:250,capacity:50}
];
let trains=JSON.parse(localStorage.getItem("railTrains"))||initialTrains;
let users=JSON.parse(localStorage.getItem("railUsers"))||[];
let bookings=JSON.parse(localStorage.getItem("railBookings"))||[];
let currentUser=null,isAdmin=false,search=null,selectedTrainId=null;

const multipliers={"Sleeper":1,"AC 3 Tier":1.8,"AC 2 Tier":2.6,"AC First Class":4};
function save(){
localStorage.setItem("railTrains",JSON.stringify(trains));
localStorage.setItem("railUsers",JSON.stringify(users));
localStorage.setItem("railBookings",JSON.stringify(bookings));
}
function esc(s){return String(s).replace(/[&<>"']/g,c=>({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#39;"}[c]));}
function show(id){
document.querySelectorAll(".page").forEach(p=>p.classList.remove("active"));
document.getElementById(id).classList.add("active");renderAll();window.scrollTo(0,0);
}
function today(){
let d=new Date();return d.getFullYear()+"-"+String(d.getMonth()+1).padStart(2,"0")+"-"+String(d.getDate()).padStart(2,"0");
}
function money(n){return "₹"+Number(n).toLocaleString("en-IN");}
function fare(t,c){return Math.round(t.fare*multipliers[c]);}
function activeCount(trainId,c){
return bookings.filter(b=>b.trainId===trainId&&b.travelClass===c&&b.status==="Confirmed").length;
}
function searchTrains(e){
e.preventDefault();
let origin=document.getElementById("origin").value.trim().toLowerCase();
let destination=document.getElementById("destination").value.trim().toLowerCase();
let date=document.getElementById("journeyDate").value;
if(origin===destination||date<today()){alert("Enter different stations and a valid journey date.");return;}
search={origin,destination,date,travelClass:document.getElementById("searchClass").value};
document.getElementById("searchMessage").textContent="Search date: "+date;
renderTrains();show("trains");
}
function renderTrains(){
let list=trains.filter(t=>!search||
(t.origin.toLowerCase()===search.origin&&t.destination.toLowerCase()===search.destination));
document.getElementById("trainList").innerHTML=list.map(t=>{
let c=search&&search.travelClass!=="all"?search.travelClass:"Sleeper";
let booked=activeCount(t.id,c);
return `<div class="card"><div class="train-icon">🚆</div>
<h3>${esc(t.name)}</h3><p>Train No: ${esc(t.number)}</p>
<p>${esc(t.origin)} → ${esc(t.destination)}</p><p>Departure: ${esc(t.time)}</p>
<p>Journey: ${search?search.date:"Search a date first"}</p>
<p>Class: ${esc(c)} · Demo availability: ${Math.max(0,t.capacity-booked)} seats</p>
<p class="price">${money(fare(t,c))}</p>
<button class="primary" onclick="selectTrain(${t.id})">Reserve Ticket</button></div>`;
}).join("")||"<p>No matching trains found. Try another route.</p>";
}
function register(e){
e.preventDefault();let name=document.getElementById("regName").value.trim();
let email=document.getElementById("regEmail").value.trim().toLowerCase();
let password=document.getElementById("regPassword").value;
if(users.some(u=>u.email===email)){alert("Email already registered.");return;}
users.push({name,email,password});save();e.target.reset();alert("Registration successful. Please log in.");
}
function login(e){
e.preventDefault();let email=document.getElementById("loginEmail").value.trim().toLowerCase();
let password=document.getElementById("loginPassword").value;
let u=users.find(x=>x.email===email&&x.password===password);
if(!u){alert("Invalid credentials. Register first if needed.");return;}
currentUser={name:u.name,email:u.email};
document.getElementById("loginStatus").textContent="Welcome "+u.name;
show("mytickets");
}
function logout(){currentUser=null;show("home");}
function selectTrain(id){
if(!currentUser){alert("Please register and log in before booking.");show("account");return;}
if(!search){alert("Search for trains and dates first.");show("home");return;}
selectedTrainId=id;
let t=trains.find(x=>x.id===id);
document.getElementById("selectedTrain").textContent=t.name+" · "+t.origin+" to "+t.destination+" · "+search.date;
document.getElementById("travelClass").value=search.travelClass==="all"?"Sleeper":search.travelClass;
updateFare();show("reservation");
}
function updateFare(){
let t=trains.find(x=>x.id===selectedTrainId);if(!t)return;
let c=document.getElementById("travelClass").value;
document.getElementById("fareDetails").textContent="Demo fare: "+money(fare(t,c));
}
function bookTicket(e){
e.preventDefault();if(!currentUser||!search||!selectedTrainId)return;
let t=trains.find(x=>x.id===selectedTrainId);
let c=document.getElementById("travelClass").value;
if(activeCount(t.id,c)>=t.capacity){alert("No seats available in this class.");return;}
let pnr=String(Date.now()).slice(-10);
let method=document.getElementById("paymentMethod").value;
bookings.push({pnr,email:currentUser.email,passenger:document.getElementById("passengerName").value.trim(),
age:Number(document.getElementById("passengerAge").value),gender:document.getElementById("gender").value,
trainId:t.id,origin:t.origin,destination:t.destination,date:search.date,time:t.time,
travelClass:c,fare:fare(t,c),payment:method,status:"Confirmed",refund:0});
save();alert("Demo ticket created. Your PNR is "+pnr+". No actual payment or email was processed.");
show("mytickets");
}
function cancelBooking(pnr,admin=false){
let b=bookings.find(x=>x.pnr===pnr);
if(!b||b.status!=="Confirmed")return;
if(!admin&&b.email!==currentUser?.email){alert("You can cancel only your own ticket.");return;}
if(!confirm("Request ticket cancellation?"))return;
b.status="Cancelled";
b.refund=Math.round(b.fare*0.8);
b.refundStatus=admin?"Refund approved in demo":"Refund pending admin approval";
save();renderAll();alert("Ticket cancelled in this demo. Estimated refund: "+money(b.refund)+". "+b.refundStatus+".");
}
function renderTickets(){
let mine=currentUser?bookings.filter(b=>b.email===currentUser.email):[];
document.getElementById("welcome").textContent=currentUser?"Welcome, "+currentUser.name:"Please log in to view tickets.";
document.getElementById("ticketList").innerHTML=mine.map(b=>{
let t=trains.find(x=>x.id===b.trainId);
return `<div class="ticket"><h3>🚆 RailConnect E-Ticket</h3>
<p><b>PNR:</b> ${b.pnr}</p><p><b>Passenger:</b> ${esc(b.passenger)} · Age ${b.age}</p>
<p><b>Train:</b> ${esc(t?t.name:"Train")} (${esc(t?t.number:"")})</p>
<p><b>Route:</b> ${esc(b.origin)} → ${esc(b.destination)}</p>
<p><b>Journey:</b> ${b.date} · ${esc(b.time)}</p>
<p><b>Class:</b> ${esc(b.travelClass)} · Fare ${money(b.fare)}</p>
<p><b>Status:</b> ${b.status}</p>
${b.status==="Cancelled"?`<p>${esc(b.refundStatus||"Refund pending")}: ${money(b.refund||0)}</p>`:""}
<div class="no-print" style="margin-top:12px">
<button class="primary" onclick="printTicket('${b.pnr}')">Print Ticket</button>
${b.status==="Confirmed"?`<button class="danger" onclick="cancelBooking('${b.pnr}')">Cancel Ticket</button>`:""}
</div></div><br>`;
}).join("")||"<p>No tickets found.</p>";
}
function printTicket(pnr){
let ticket=bookings.find(b=>b.pnr===pnr&&b.email===currentUser?.email);
if(!ticket)return;
document.querySelectorAll(".ticket").forEach(el=>el.style.display="none");
let all=[...document.querySelectorAll(".ticket")];
let idx=bookings.filter(b=>b.email===currentUser.email).findIndex(b=>b.pnr===pnr);
if(all[idx])all[idx].style.display="block";
window.print();
document.querySelectorAll(".ticket").forEach(el=>el.style.display="block");
}
function checkPNR(e){
e.preventDefault();let p=document.getElementById("pnrInput").value.trim();
let b=bookings.find(x=>x.pnr===p);
document.getElementById("pnrResult").innerHTML=b?
`<div class="card"><h3>PNR: ${b.pnr}</h3><p>Train: ${esc((trains.find(t=>t.id===b.trainId)||{}).name||"Train")}</p><p>Passenger: ${esc(b.passenger)}</p><p>Journey date: ${b.date}</p><p>Status: <b>${b.status}</b></p><p>Refund: ${money(b.refund||0)} (${esc(b.refundStatus||"Not applicable")})</p></div>`:"<p>No matching PNR found.</p>";
}
function adminLogin(e){
e.preventDefault();
if(document.getElementById("adminEmail").value==="admin@railconnect.com"&&document.getElementById("adminPassword").value==="admin123"){
isAdmin=true;document.getElementById("adminPanel").style.display="block";
document.getElementById("adminStatus").textContent="Demo admin login successful.";renderAdmin();
}else alert("Incorrect admin credentials.");
}
function addTrain(e){
e.preventDefault();if(!isAdmin)return;
trains.push({id:trains.length?Math.max(...trains.map(t=>t.id))+1:1,
name:document.getElementById("trainName").value.trim(),
number:document.getElementById("trainNumber").value.trim(),
origin:document.getElementById("trainOrigin").value.trim(),
destination:document.getElementById("trainDestination").value.trim(),
time:document.getElementById("trainTime").value,
fare:Number(document.getElementById("trainFare").value),
capacity:Number(document.getElementById("trainSeats").value)});
save();e.target.reset();renderAll();alert("Train added.");
}
function editTrain(id){
if(!isAdmin)return;let t=trains.find(x=>x.id===id);if(!t)return;
let fareValue=prompt("New base fare:",t.fare);if(fareValue===null)return;
let cap=prompt("New seat capacity:",t.capacity);if(cap===null)return;
fareValue=Number(fareValue);cap=Number(cap);
if(fareValue<=0||!Number.isFinite(fareValue)||!Number.isInteger(cap)||cap<activeCount(t.id,"Sleeper")||cap<1){
alert("Invalid fare or capacity.");return;}
t.fare=fareValue;t.capacity=cap;save();renderAll();
}
function deleteTrain(id){
if(!isAdmin)return;
if(bookings.some(b=>b.trainId===id&&b.status==="Confirmed")){alert("Cancel active bookings first.");return;}
if(confirm("Delete train?")){trains=trains.filter(t=>t.id!==id);save();renderAll();}
}
function renderAdmin(){
document.getElementById("adminTrainRows").innerHTML=trains.map(t=>`
<tr><td>${esc(t.number)}</td><td>${esc(t.name)}</td>
<td>${esc(t.origin)} → ${esc(t.destination)}</td><td>${esc(t.time)}</td><td>${money(t.fare)}</td>
<td><button onclick="editTrain(${t.id})">Edit</button>
<button class="danger" onclick="deleteTrain(${t.id})">Delete</button></td></tr>`).join("");
document.getElementById("adminBookingRows").innerHTML=bookings.map(b=>`
<tr><td>${b.pnr}</td><td>${esc(b.passenger)}</td><td>${esc((trains.find(t=>t.id===b.trainId)||{}).name||"Train")}</td>
<td>${b.date}</td><td>${esc(b.travelClass)}</td><td>${b.status}</td>
<td>${b.status==="Confirmed"?`<button class="danger" onclick="cancelBooking('${b.pnr}',true)">Cancel</button>`:
b.status==="Cancelled"&&b.refundStatus!=="Refund approved in demo"?`<button class="primary" onclick="approveRefund('${b.pnr}')">Approve Refund</button>`:"—"}</td></tr>`).join("")||"<tr><td colspan='7'>No bookings yet.</td></tr>";
document.getElementById("reportSummary").textContent="Trains: "+trains.length+" · Passengers: "+users.length+" · Reservations: "+bookings.length+" · Confirmed: "+bookings.filter(b=>b.status==="Confirmed").length+" · Cancelled: "+bookings.filter(b=>b.status==="Cancelled").length;
}
function approveRefund(pnr){
if(!isAdmin)return;let b=bookings.find(x=>x.pnr===pnr);if(!b||b.status!=="Cancelled")return;
b.refundStatus="Refund approved in demo";save();renderAll();alert("Demo refund approved. No money was transferred.");
}
function downloadReport(){
if(!isAdmin)return;
let rows=[["PNR","Email","Passenger","Train ID","Date","Class","Fare","Status","Refund"],
...bookings.map(b=>[b.pnr,b.email,b.passenger,b.trainId,b.date,b.travelClass,b.fare,b.status,b.refund||0])];
let csv=rows.map(r=>r.map(v=>'"'+String(v).replace(/"/g,'""')+'"').join(",")).join("\r\n");
let url=URL.createObjectURL(new Blob([csv],{type:"text/csv;charset=utf-8"}));
let a=document.createElement("a");a.href=url;a.download="train-reservation-report.csv";a.click();URL.revokeObjectURL(url);
}
function renderAll(){renderTrains();renderTickets();renderAdmin();}
renderAll();
</script>
</body>
</html>
