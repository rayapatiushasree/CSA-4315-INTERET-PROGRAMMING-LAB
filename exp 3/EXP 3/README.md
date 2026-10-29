<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>EasyBus - Ticket Booking System</title>
<style>
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:Arial,sans-serif;background:#f4f7fb;color:#172033}
header{background:#173b70;color:white;padding:22px 5%;
display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:10px}
nav{background:white;display:flex;flex-wrap:wrap;gap:8px;padding:14px 4%;
box-shadow:0 2px 8px #ddd}
button{border:0;border-radius:6px;padding:11px 14px;cursor:pointer;font-weight:bold}
nav button{background:#eaf0ff;color:#173b70}
.primary{background:#245ce5;color:white}
.danger{background:#c62828;color:white}
.container{width:92%;max-width:1150px;margin:28px auto}
.page{display:none}.page.active{display:block}
.hero{background:linear-gradient(120deg,#173b70,#3479ed);
color:white;padding:35px;border-radius:14px;margin-bottom:22px}
.hero h2{font-size:32px;margin-bottom:12px}
.hero p{line-height:1.6}
.card{background:white;padding:21px;border-radius:12px;
box-shadow:0 3px 12px #0000000b;margin-bottom:18px}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:16px}
h2,h3{margin-bottom:14px}
input,select,textarea{width:100%;padding:12px;margin:7px 0 15px;
border:1px solid #ccd3df;border-radius:6px;font-size:15px}
label{font-weight:bold}
.bus-icon{font-size:40px;margin-bottom:12px}
.price{font-size:23px;color:#245ce5;font-weight:bold;margin:12px 0}
.muted{color:#64748b;line-height:1.6}
.seats{display:grid;grid-template-columns:repeat(4,minmax(38px,1fr));
gap:9px;margin:18px 0;max-width:360px}
.seat{padding:12px 4px;background:#d9f5e4;color:#176534}
.seat.selected{background:#245ce5;color:white}
.seat.booked{background:#ddd;color:#777;cursor:not-allowed}
.tag{display:inline-block;background:#e4efff;color:#173b70;
padding:5px 8px;border-radius:5px;margin:5px 0}
table{border-collapse:collapse;width:100%;min-width:620px;background:white}
th,td{text-align:left;padding:11px;border-bottom:1px solid #e5e7eb}
th{background:#eaf0ff}
.table-wrap{overflow-x:auto}
footer{background:#173b70;color:white;text-align:center;padding:25px;margin-top:35px}
@media(max-width:600px){.container{width:95%}.hero{padding:24px}
.hero h2{font-size:25px}header{padding:18px}}
</style>
</head>
<body>
<header>
<h1>🚌 EasyBus</h1>
<span>Your Journey, Your Way</span>
</header>

<nav>
<button onclick="show('home')">Home</button>
<button onclick="show('buses')">Available Buses</button>
<button onclick="show('account')">Login / Register</button>
<button onclick="show('mybookings')">My Bookings</button>
<button onclick="show('comments')">Comments</button>
<button onclick="show('news')">Travel News</button>
<button onclick="show('admin')">Admin Panel</button>
</nav>

<main class="container">
<section id="home" class="page active">
<div class="hero">
<h2>Book Your Bus Tickets Online</h2>
<p>Find buses, choose your seats, and manage your journeys easily.</p>
</div>
<div class="card">
<h2>Search Buses</h2>
<form onsubmit="searchBuses(event)">
<label>From</label><input id="from" placeholder="Departure city" required>
<label>To</label><input id="to" placeholder="Destination city" required>
<div class="grid">
<div><label>Journey Date</label><input id="journeyDate" type="date" required></div>
<div><label>Bus Category</label>
<select id="categoryFilter">
<option value="all">All Categories</option>
<option>Volvo AC</option><option>Sleeper Coach</option>
<option>Seater Coach</option><option>Non-AC</option>
</select></div>
</div>
<button class="primary">Search Buses</button>
</form>
<p id="searchMessage" class="muted"></p>
</div>
<h2>Popular Bus Categories</h2>
<div class="grid">
<div class="card"><div class="bus-icon">❄️</div><h3>Volvo AC</h3><p class="muted">Comfortable air-conditioned travel.</p></div>
<div class="card"><div class="bus-icon">🛏️</div><h3>Sleeper Coach</h3><p class="muted">Relax during overnight journeys.</p></div>
<div class="card"><div class="bus-icon">💺</div><h3>Seater Coach</h3><p class="muted">Convenient everyday travel.</p></div>
<div class="card"><div class="bus-icon">🚌</div><h3>Non-AC</h3><p class="muted">Budget-friendly journeys.</p></div>
</div>
</section>

<section id="buses" class="page">
<h2>Available Buses</h2>
<div id="busList" class="grid"></div>
</section>

<section id="account" class="page">
<div class="grid">
<div class="card">
<h2>User Registration</h2>
<form onsubmit="register(event)">
<label>Full Name</label><input id="regName" required>
<label>Email</label><input id="regEmail" type="email" required>
<label>Password</label><input id="regPassword" type="password" minlength="6" required>
<button class="primary">Register</button>
</form>
</div>
<div class="card">
<h2>User Login</h2>
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

<section id="booking" class="page">
<div class="card">
<h2>Select Seats</h2>
<p id="selectedBus" class="muted"></p>
<p>Green = Available · Blue = Selected · Grey = Booked</p>
<div id="seatGrid" class="seats"></div>
<p id="seatSummary">Selected seats: None</p>
<h3 id="fare">Total: ₹0</h3>
<form onsubmit="confirmBooking(event)">
<label>Passenger Name</label><input id="passengerName" required>
<label>Passenger Age</label><input id="passengerAge" type="number" min="1" max="120" required>
<label>Contact Email</label><input id="passengerEmail" type="email" required>
<button class="primary">Confirm Demo Booking</button>
</form>
<p class="muted">No real payment will be processed.</p>
</div>
</section>

<section id="mybookings" class="page">
<div class="card">
<h2>My Booking History</h2>
<p id="userWelcome" class="muted"></p>
<div class="table-wrap"><table>
<thead><tr><th>Ticket</th><th>Route</th><th>Date</th><th>Seats</th><th>Fare</th><th>Status</th><th>Action</th></tr></thead>
<tbody id="bookingRows"></tbody>
</table></div>
</div>
</section>

<section id="comments" class="page">
<div class="card">
<h2>Bus Comments and Reviews</h2>
<form onsubmit="postComment(event)">
<label>Select Bus</label><select id="commentBus"></select>
<label>Rating</label><select id="rating"><option>5</option><option>4</option><option>3</option><option>2</option><option>1</option></select>
<label>Comment</label><textarea id="commentText" required></textarea>
<button class="primary">Post Comment</button>
</form>
<div id="commentList"></div>
</div>
</section>

<section id="news" class="page">
<div class="card">
<h2>Travel News</h2>
<div id="newsList"></div>
</div>
</section>

<section id="admin" class="page">
<div class="card">
<h2>Administrator Login</h2>
<p class="muted">Demo credentials: admin@easybus.com / admin123</p>
<form onsubmit="adminLogin(event)">
<label>Email</label><input id="adminEmail" type="email" required>
<label>Password</label><input id="adminPassword" type="password" required>
<button class="primary">Login as Admin</button>
</form>
<p id="adminStatus" class="muted"></p>
</div>

<div id="adminPanel" style="display:none">
<div class="card">
<h2>Add a Bus</h2>
<form onsubmit="addBus(event)">
<label>Bus Operator / Name</label><input id="busName" required>
<label>From</label><input id="busFrom" required>
<label>To</label><input id="busTo" required>
<label>Category</label><select id="busCategory">
<option>Volvo AC</option><option>Sleeper Coach</option><option>Seater Coach</option><option>Non-AC</option>
</select>
<label>Departure Time</label><input id="busTime" type="time" required>
<label>Fare per Seat</label><input id="busPrice" type="number" min="1" required>
<label>Total Seats</label><input id="busSeats" type="number" min="1" max="60" value="20" required>
<button class="primary">Add Bus</button>
</form>
</div>

<div class="card">
<h2>Manage Buses</h2>
<div class="table-wrap"><table><thead><tr>
<th>ID</th><th>Bus</th><th>Route</th><th>Category</th><th>Date</th><th>Fare</th><th>Action</th>
</tr></thead><tbody id="adminBusRows"></tbody></table></div>
</div>

<div class="card">
<h2>Registered Users</h2>
<div class="table-wrap"><table><thead><tr><th>Name</th><th>Email</th></tr></thead>
<tbody id="adminUsers"></tbody></table></div>
</div>

<div class="card">
<h2>Booking Report</h2>
<p id="reportSummary"></p>
<button class="primary" onclick="downloadReport()">Download CSV Report</button>
</div>

<div class="card">
<h2>Manage Travel News</h2>
<form onsubmit="addNews(event)">
<label>News Title</label><input id="newsTitle" required>
<label>News Details</label><textarea id="newsBody" required></textarea>
<button class="primary">Publish News</button>
</form>
</div>
</div>
</section>
</main>

<footer><p>© 2026 EasyBus Ticket Booking System</p><p style="margin-top:8px">Student Web Development Project</p></footer>

<script>
const initialBuses=[
{id:1,name:"Express Volvo",from:"Chennai",to:"Bengaluru",category:"Volvo AC",time:"08:00",price:850,capacity:20,date:""},
{id:2,name:"Night Rider",from:"Chennai",to:"Hyderabad",category:"Sleeper Coach",time:"21:00",price:1200,capacity:20,date:""},
{id:3,name:"City Travels",from:"Bengaluru",to:"Chennai",category:"Seater Coach",time:"09:30",price:650,capacity:20,date:""},
{id:4,name:"Budget Bus",from:"Chennai",to:"Tirupati",category:"Non-AC",time:"07:00",price:350,capacity:20,date:""}
];
let buses=JSON.parse(localStorage.getItem("busList"))||initialBuses;
let users=JSON.parse(localStorage.getItem("busUsers"))||[];
let bookings=JSON.parse(localStorage.getItem("busBookings"))||[];
let comments=JSON.parse(localStorage.getItem("busComments"))||[];
let news=JSON.parse(localStorage.getItem("busNews"))||[
{title:"Welcome to EasyBus",body:"Search your route and book a demo ticket."}
];
let currentUser=null,isAdmin=false,search=null,selectedBusId=null,selectedSeats=[];

function save(){
localStorage.setItem("busList",JSON.stringify(buses));
localStorage.setItem("busUsers",JSON.stringify(users));
localStorage.setItem("busBookings",JSON.stringify(bookings));
localStorage.setItem("busComments",JSON.stringify(comments));
localStorage.setItem("busNews",JSON.stringify(news));
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
function seatsBooked(busId){
return bookings.filter(b=>b.busId===busId&&b.status==="Confirmed").flatMap(b=>b.seats);
}
function renderBuses(){
let list=buses.filter(b=>!search||
(b.from.toLowerCase()===search.from&&b.to.toLowerCase()===search.to&&
b.category===search.category||b.from.toLowerCase()===search.from&&
b.to.toLowerCase()===search.to&&search.category==="all"));
document.getElementById("busList").innerHTML=list.map(b=>{
let booked=seatsBooked(b.id).length;
return `<div class="card"><div class="bus-icon">🚌</div><h3>${esc(b.name)}</h3>
<p>${esc(b.from)} → ${esc(b.to)}</p><p>${esc(b.category)} · ${esc(b.time)}</p>
<p>Journey date: ${search?search.date:"Choose a date on Home"}</p>
<p>Available seats: ${b.capacity-booked}</p><p class="price">${money(b.price)}</p>
<button class="primary" onclick="chooseBus(${b.id})">Select Seats</button></div>`;
}).join("")||"<p>No buses found for this route.</p>";
}
function searchBuses(e){
e.preventDefault();
let from=document.getElementById("from").value.trim().toLowerCase();
let to=document.getElementById("to").value.trim().toLowerCase();
let date=document.getElementById("journeyDate").value;
if(from===to||date<today()){alert("Choose different cities and a valid future date.");return;}
search={from,to,date,category:document.getElementById("categoryFilter").value};
renderBuses();show("buses");
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
if(!u){alert("Invalid login. Register first.");return;}
currentUser={name:u.name,email:u.email};document.getElementById("loginStatus").textContent="Welcome "+u.name;
show("mybookings");
}
function logout(){currentUser=null;show("home");}
function chooseBus(id){
if(!currentUser){alert("Register and log in before booking.");show("account");return;}
if(!search){alert("Search for a route and date first.");show("home");return;}
selectedBusId=id;selectedSeats=[];renderSeats();show("booking");
}
function renderSeats(){
let bus=buses.find(b=>b.id===selectedBusId);if(!bus)return;
document.getElementById("selectedBus").textContent=bus.name+" · "+bus.from+" to "+bus.to+" · "+search.date;
let booked=seatsBooked(bus.id);
document.getElementById("seatGrid").innerHTML=Array.from({length:bus.capacity},(_,i)=>{
let n=i+1,disabled=booked.includes(n);
return `<button type="button" class="seat ${disabled?"booked":selectedSeats.includes(n)?"selected":""}" ${disabled?"disabled":""} onclick="toggleSeat(${n})">${n}</button>`;
}).join("");
document.getElementById("seatSummary").textContent="Selected seats: "+(selectedSeats.join(", ")||"None");
document.getElementById("fare").textContent="Total: "+money(selectedSeats.length*bus.price);
}
function toggleSeat(n){
if(selectedSeats.includes(n))selectedSeats=selectedSeats.filter(x=>x!==n);
else selectedSeats.push(n);
renderSeats();
}
function confirmBooking(e){
e.preventDefault();
if(!currentUser||!selectedSeats.length){alert("Log in and select at least one seat.");return;}
let bus=buses.find(b=>b.id===selectedBusId);
if(selectedSeats.some(n=>seatsBooked(bus.id).includes(n))){alert("A seat has just been booked. Please select again.");renderSeats();return;}
bookings.push({id:Date.now(),email:currentUser.email,busId:bus.id,
from:bus.from,to:bus.to,date:search.date,seats:[...selectedSeats],
name:document.getElementById("passengerName").value.trim(),
age:Number(document.getElementById("passengerAge").value),
passengerEmail:document.getElementById("passengerEmail").value.trim(),
total:selectedSeats.length*bus.price,status:"Confirmed"});
save();selectedSeats=[];alert("Demo ticket confirmed. No payment or email was sent.");show("mybookings");
}
function cancelTicket(id){
let b=bookings.find(x=>x.id===id);
if(!b||b.status!=="Confirmed")return;
if(confirm("Cancel this demo ticket?")){b.status="Cancelled";save();renderAll();}
}
function renderBookings(){
let mine=currentUser?bookings.filter(b=>b.email===currentUser.email):[];
document.getElementById("userWelcome").textContent=currentUser?"Welcome, "+currentUser.name:"Please log in to view bookings.";
document.getElementById("bookingRows").innerHTML=mine.map(b=>`
<tr><td>${b.id}</td><td>${esc(b.from)} → ${esc(b.to)}</td><td>${b.date}</td>
<td>${b.seats.join(", ")}</td><td>${money(b.total)}</td><td>${b.status}</td>
<td>${b.status==="Confirmed"?`<button class="danger" onclick="cancelTicket(${b.id})">Cancel</button>`:"—"}</td></tr>
`).join("")||'<tr><td colspan="7">No bookings yet.</td></tr>';
}
function adminLogin(e){
e.preventDefault();
if(document.getElementById("adminEmail").value==="admin@easybus.com"&&document.getElementById("adminPassword").value==="admin123"){
isAdmin=true;document.getElementById("adminPanel").style.display="block";
document.getElementById("adminStatus").textContent="Demo admin login successful.";renderAdmin();
}else alert("Incorrect admin credentials.");
}
function addBus(e){
e.preventDefault();if(!isAdmin)return;
buses.push({id:buses.length?Math.max(...buses.map(b=>b.id))+1:1,
name:document.getElementById("busName").value.trim(),
from:document.getElementById("busFrom").value.trim(),
to:document.getElementById("busTo").value.trim(),
category:document.getElementById("busCategory").value,
time:document.getElementById("busTime").value,
price:Number(document.getElementById("busPrice").value),
capacity:Number(document.getElementById("busSeats").value),date:""});
save();e.target.reset();renderAll();alert("Bus added successfully.");
}
function editBus(id){
if(!isAdmin)return;let b=buses.find(x=>x.id===id);if(!b)return;
let price=prompt("New fare:",b.price);if(price===null)return;
let capacity=prompt("New seat capacity:",b.capacity);if(capacity===null)return;
price=Number(price);capacity=Number(capacity);
let booked=seatsBooked(id).length;
if(price<=0||!Number.isFinite(price)||!Number.isInteger(capacity)||capacity<booked||capacity<1){
alert("Invalid price or capacity below booked seat count.");return;}
b.price=price;b.capacity=capacity;save();renderAll();
}
function deleteBus(id){
if(!isAdmin)return;
if(bookings.some(b=>b.busId===id&&b.status==="Confirmed")){
alert("Cancel active bookings before deleting this bus.");return;}
if(confirm("Delete this bus?")){buses=buses.filter(b=>b.id!==id);save();renderAll();}
}
function renderAdmin(){
document.getElementById("adminBusRows").innerHTML=buses.map(b=>`
<tr><td>${b.id}</td><td>${esc(b.name)}</td><td>${esc(b.from)} → ${esc(b.to)}</td>
<td>${esc(b.category)}</td><td>${esc(b.time)}</td><td>${money(b.price)}</td>
<td><button onclick="editBus(${b.id})">Edit</button> <button class="danger" onclick="deleteBus(${b.id})">Delete</button></td></tr>`).join("");
document.getElementById("adminUsers").innerHTML=users.map(u=>`<tr><td>${esc(u.name)}</td><td>${esc(u.email)}</td></tr>`).join("")||"<tr><td colspan='2'>No registered users.</td></tr>";
document.getElementById("reportSummary").textContent="Users: "+users.length+" · Total bookings: "+bookings.length+" · Confirmed: "+bookings.filter(b=>b.status==="Confirmed").length+" · Cancelled: "+bookings.filter(b=>b.status==="Cancelled").length+" · Confirmed demo revenue: "+money(bookings.filter(b=>b.status==="Confirmed").reduce((s,b)=>s+b.total,0));
document.getElementById("commentBus").innerHTML=buses.map(b=>`<option value="${b.id}">${esc(b.name)}</option>`).join("");
}
function postComment(e){
e.preventDefault();if(!currentUser){alert("Please log in first.");show("account");return;}
comments.push({name:currentUser.name,busId:Number(document.getElementById("commentBus").value),
rating:Number(document.getElementById("rating").value),text:document.getElementById("commentText").value});
save();e.target.reset();renderComments();alert("Comment posted.");
}
function renderComments(){
document.getElementById("commentList").innerHTML=comments.map(c=>{
let b=buses.find(x=>x.id===c.busId);
return `<div class="card"><b>${esc(c.name)}</b> · ${"★".repeat(c.rating)}<p>${esc(b?b.name:"Bus")}</p><p>${esc(c.text)}</p></div>`;
}).join("")||"<p>No comments yet.</p>";
}
function addNews(e){
e.preventDefault();if(!isAdmin)return;
news.unshift({title:document.getElementById("newsTitle").value,
body:document.getElementById("newsBody").value});
save();e.target.reset();renderNews();
}
function renderNews(){
document.getElementById("newsList").innerHTML=news.map(n=>`<div class="card"><h3>${esc(n.title)}</h3><p>${esc(n.body)}</p></div>`).join("");
}
function downloadReport(){
if(!isAdmin)return;
let rows=[["Ticket ID","User Email","From","To","Date","Seats","Fare","Status"],
...bookings.map(b=>[b.id,b.email,b.from,b.to,b.date,b.seats.join(" "),b.total,b.status])];
let csv=rows.map(row=>row.map(v=>'"'+String(v).replace(/"/g,'""')+'"').join(",")).join("\r\n");
let url=URL.createObjectURL(new Blob([csv],{type:"text/csv;charset=utf-8"}));
let a=document.createElement("a");a.href=url;a.download="bus-booking-report.csv";a.click();URL.revokeObjectURL(url);
}
function renderAll(){renderBuses();renderBookings();renderAdmin();renderComments();renderNews();}
renderAll();
</script>
</body>
</
