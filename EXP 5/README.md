
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>SkyBook - Flight Reservation System</title>
<style>
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:Arial,sans-serif;background:#f3f6fb;color:#17243b}
header{background:#102c50;color:white;padding:22px 5%;
display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:10px}
nav{display:flex;gap:8px;flex-wrap:wrap;padding:14px 4%;
background:white;box-shadow:0 2px 8px #ddd}
button{border:0;border-radius:6px;padding:11px 14px;cursor:pointer;font-weight:bold}
nav button{background:#e8efff;color:#102c50}
.primary{background:#245ce5;color:white}
.danger{background:#c62828;color:white}
.container{width:92%;max-width:1150px;margin:28px auto}
.page{display:none}.page.active{display:block}
.hero{background:linear-gradient(120deg,#102c50,#3979e8);
color:white;padding:36px;border-radius:14px;margin-bottom:22px}
.hero h2{font-size:32px;margin-bottom:12px}
.hero p{line-height:1.6}
.card{background:white;padding:21px;border-radius:12px;
box-shadow:0 3px 12px #0000000b;margin-bottom:18px}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:16px}
h2,h3{margin-bottom:14px}
label{font-weight:bold}
input,select,textarea{width:100%;padding:12px;margin:7px 0 15px;
border:1px solid #cbd5e1;border-radius:6px;font-size:15px}
.muted{color:#64748b;line-height:1.6}
.flight-icon{font-size:40px;margin-bottom:12px}
.price{font-size:23px;font-weight:bold;color:#245ce5;margin:12px 0}
.tag{display:inline-block;background:#e6f3ec;color:#176340;padding:5px 8px;border-radius:5px;margin:5px 0}
.seats{display:grid;grid-template-columns:repeat(4,minmax(38px,1fr));gap:9px;max-width:360px;margin:18px 0}
.seat{background:#d9f5e4;color:#176534;padding:12px 4px}
.seat.selected{background:#245ce5;color:white}
.seat.booked{background:#ddd;color:#777;cursor:not-allowed}
table{width:100%;border-collapse:collapse;min-width:620px;background:white}
th,td{text-align:left;padding:11px;border-bottom:1px solid #e5e7eb}
th{background:#e8efff}
.table-wrap{overflow-x:auto}
.ticket{border:2px dashed #245ce5;padding:22px;border-radius:10px;background:white}
footer{background:#102c50;color:white;text-align:center;padding:25px;margin-top:35px}
@media(max-width:600px){.container{width:95%}.hero{padding:24px}.hero h2{font-size:25px}}
@media print{header,nav,footer,.no-print{display:none!important}body{background:white}
.container{width:100%;margin:0}.page{display:none!important}
#mytickets.page.active{display:block!important}.ticket{break-inside:avoid}}
</style>
</head>
<body>
<header>
<h1>✈️ SkyBook</h1>
<span>Your Journey Begins Here</span>
</header>

<nav>
<button onclick="show('home')">Home</button>
<button onclick="show('flights')">Search Flights</button>
<button onclick="show('account')">Login / Register</button>
<button onclick="show('mytickets')">My Tickets</button>
<button onclick="show('status')">Booking Status</button>
<button onclick="show('feedback')">Feedback</button>
<button onclick="show('admin')">Admin Panel</button>
</nav>

<main class="container">
<section id="home" class="page active">
<div class="hero">
<h2>Discover the World with SkyBook</h2>
<p>Search flights, select seats, manage reservations, and print demo e-tickets.</p>
</div>
<div class="card">
<h2>Search Flights</h2>
<form onsubmit="searchFlights(event)">
<label>From</label><input id="origin" placeholder="e.g. Chennai" required>
<label>Destination</label><input id="destination" placeholder="e.g. Delhi" required>
<div class="grid">
<div><label>Departure Date</label><input id="flightDate" type="date" required></div>
<div><label>Cabin Class</label><select id="searchClass">
<option value="all">All Classes</option><option>Economy</option>
<option>Business</option><option>First Class</option></select></div>
<div><label>Passengers</label><input id="passengers" type="number" min="1" max="6" value="1" required></div>
</div>
<button class="primary">Search Flights</button>
</form>
<p id="searchMessage" class="muted"></p>
</div>
<div class="grid">
<div class="card"><h3>🪑 Economy</h3><p class="muted">Affordable everyday travel.</p></div>
<div class="card"><h3>💼 Business</h3><p class="muted">Extra comfort for your journey.</p></div>
<div class="card"><h3>⭐ First Class</h3><p class="muted">A premium cabin experience.</p></div>
</div>
</section>

<section id="flights" class="page">
<h2>Available Flights</h2>
<div id="flightList" class="grid"></div>
</section>

<section id="account" class="page">
<div class="grid">
<div class="card"><h2>Register</h2>
<form onsubmit="register(event)">
<label>Full Name</label><input id="regName" required>
<label>Email</label><input id="regEmail" type="email" required>
<label>Password</label><input id="regPassword" type="password" minlength="6" required>
<button class="primary">Register</button></form></div>
<div class="card"><h2>Login</h2>
<form onsubmit="login(event)">
<label>Email</label><input id="loginEmail" type="email" required>
<label>Password</label><input id="loginPassword" type="password" required>
<button class="primary">Login</button></form>
<p id="loginStatus" class="muted"></p>
<button onclick="logout()">Logout</button>
<hr style="margin:20px 0">
<h3>Forgot Password (Demo)</h3>
<form onsubmit="forgotPassword(event)">
<label>Registered Email</label><input id="forgotEmail" type="email" required>
<button class="primary">Reset Password</button></form>
</div></div>
</section>

<section id="booking" class="page">
<div class="card">
<h2>Choose Your Seat</h2>
<p id="selectedFlight" class="muted"></p>
<p>Green: Available · Blue: Selected · Grey: Booked</p>
<div id="seatGrid" class="seats"></div>
<p id="seatSummary">Selected seats: None</p>
<h3 id="fareTotal"></h3>
<form onsubmit="bookFlight(event)">
<label>Passenger Name</label><input id="passengerName" required>
<label>Passenger Age</label><input id="passengerAge" type="number" min="1" max="120" required>
<label>Passenger Email</label><input id="passengerEmail" type="email" required>
<label>Payment Method (Demo)</label><select id="paymentMethod">
<option>Online Demo Payment</option><option>Offline Payment (Demo)</option></select>
<button class="primary">Confirm Demo Booking</button>
</form>
<p class="muted">No real payment will be processed.</p>
</div>
</section>

<section id="mytickets" class="page">
<div class="card"><h2>My Tickets and Itineraries</h2>
<p id="welcome" class="muted"></p><div id="ticketList"></div>
</div></section>

<section id="status" class="page">
<div class="card"><h2>View Booking Status</h2>
<form onsubmit="checkStatus(event)">
<label>Booking Reference</label><input id="referenceInput" required>
<button class="primary">Check Status</button></form>
<div id="statusResult"></div>
</div></section>

<section id="feedback" class="page">
<div class="card"><h2>Customer Feedback</h2>
<form onsubmit="submitFeedback(event)">
<label>Rating</label><select id="rating"><option>5</option><option>4</option><option>3</option><option>2</option><option>1</option></select>
<label>Comments</label><textarea id="feedbackText" required></textarea>
<button class="primary">Submit Feedback</button></form>
<div id="feedbackList"></div>
</div></section>

<section id="admin" class="page">
<div class="card"><h2>Administrator Login</h2>
<p class="muted">Demo credentials: admin@skybook.com / admin123</p>
<form onsubmit="adminLogin(event)">
<label>Email</label><input id="adminEmail" type="email" required>
<label>Password</label><input id="adminPassword" type="password" required>
<button class="primary">Login as Admin</button></form>
<p id="adminStatus" class="muted"></p>
</div>

<div id="adminPanel" style="display:none">
<div class="card"><h2>Add Flight</h2>
<form onsubmit="addFlight(event)">
<label>Airline</label><input id="airline" required>
<label>Flight Number</label><input id="flightNumber" required>
<label>Origin</label><input id="flightOrigin" required>
<label>Destination</label><input id="flightDestination" required>
<label>Departure Time</label><input id="departureTime" type="time" required>
<label>Economy Fare</label><input id="economyFare" type="number" min="1" required>
<label>Seat Capacity</label><input id="capacity" type="number" min="1" max="200" value="30" required>
<button class="primary">Add Flight</button></form>
</div>

<div class="card"><h2>Manage Flights</h2>
<div class="table-wrap"><table><thead><tr>
<th>ID</th><th>Airline</th><th>Route</th><th>Time</th><th>Fare</th><th>Action</th>
</tr></thead><tbody id="adminFlightRows"></tbody></table></div>
</div>

<div class="card"><h2>Manage Reservations</h2>
<div class="table-wrap"><table><thead><tr>
<th>Reference</th><th>Passenger</th><th>Flight</th><th>Date</th><th>Class</th><th>Status</th><th>Action</th>
</tr></thead><tbody id="adminBookingRows"></tbody></table></div>
</div>

<div class="card"><h2>Reports</h2><p id="reportSummary"></p>
<button class="primary" onclick="downloadReport()">Download CSV Report</button></div>
</div>
</section>
</main>
<footer><p>© 2026 SkyBook Flight Reservation System</p><p style="margin-top:8px">Frontend demonstration project</p></footer>

<script>
const initialFlights=[
{id:1,airline:"Sky Airways",number:"SA101",origin:"Chennai",destination:"Delhi",time:"08:30",fare:4500,capacity:30},
{id:2,airline:"Blue Wings",number:"BW202",origin:"Chennai",destination:"Mumbai",time:"11:00",fare:3800,capacity:30},
{id:3,airline:"Cloud Air",number:"CA303",origin:"Bengaluru",destination:"Delhi",time:"15:00",fare:4200,capacity:30},
{id:4,airline:"Sky Airways",number:"SA404",origin:"Delhi",destination:"Chennai",time:"19:00",fare:4700,capacity:30}
];
let flights=JSON.parse(localStorage.getItem("skyFlights"))||initialFlights;
let users=JSON.parse(localStorage.getItem("skyUsers"))||[];
let bookings=JSON.parse(localStorage.getItem("skyBookings"))||[];
let feedback=JSON.parse(localStorage.getItem("skyFeedback"))||[];
let currentUser=null,isAdmin=false,search=null,selectedFlightId=null,selectedSeats=[];
const classes={"Economy":1,"Business":2.2,"First Class":3.8};

function save(){
localStorage.setItem("skyFlights",JSON.stringify(flights));
localStorage.setItem("skyUsers",JSON.stringify(users));
localStorage.setItem("skyBookings",JSON.stringify(bookings));
localStorage.setItem("skyFeedback",JSON.stringify(feedback));
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
function fare(f,c){return Math.round(f.fare*classes[c]);}
function bookedCount(id,c){return bookings.filter(b=>b.flightId===id&&b.cabin===c&&b.status==="Confirmed").reduce((n,b)=>n+b.seats.length,0);}
function searchFlights(e){
e.preventDefault();
let origin=document.getElementById("origin").value.trim().toLowerCase();
let destination=document.getElementById("destination").value.trim().toLowerCase();
let date=document.getElementById("flightDate").value;
if(origin===destination||date<today()){alert("Enter different airports and a valid future date.");return;}
search={origin,destination,date,cabin:document.getElementById("searchClass").value,
passengers:Number(document.getElementById("passengers").value)};
document.getElementById("searchMessage").textContent="Search date: "+date;
renderFlights();show("flights");
}
function renderFlights(){
let list=flights.filter(f=>!search||(f.origin.toLowerCase()===search.origin&&f.destination.toLowerCase()===search.destination));
document.getElementById("flightList").innerHTML=list.map(f=>{
let c=search&&search.cabin!=="all"?search.cabin:"Economy";
return `<div class="card"><div class="flight-icon">✈️</div><h3>${esc(f.airline)}</h3>
<p>Flight: ${esc(f.number)}</p><p>${esc(f.origin)} → ${esc(f.destination)}</p>
<p>Departure: ${esc(f.time)}</p><p>Travel date: ${search?search.date:"Search for a date"}</p>
<p>Class: ${esc(c)}</p><p>Demo seats available: ${Math.max(0,f.capacity-bookedCount(f.id,c))}</p>
<p class="price">${money(fare(f,c))} per passenger</p>
<button class="primary" onclick="selectFlight(${f.id})">Select Flight</button></div>`;
}).join("")||"<p>No flights found for this route.</p>";
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
if(!u){alert("Invalid login. Please register first.");return;}
currentUser={name:u.name,email:u.email};document.getElementById("loginStatus").textContent="Welcome "+u.name;
show("mytickets");
}
function logout(){currentUser=null;show("home");}
function forgotPassword(e){
e.preventDefault();let email=document.getElementById("forgotEmail").value.trim().toLowerCase();
alert(users.some(u=>u.email===email)?
"Demo only: password reset requires a secure backend and email verification.":
"No registered user found for that email.");
}
function selectFlight(id){
if(!currentUser){alert("Register and log in before booking.");show("account");return;}
if(!search){alert("Search flights and dates first.");show("home");return;}
selectedFlightId=id;selectedSeats=[];
let f=flights.find(x=>x.id===id);
document.getElementById("selectedFlight").textContent=f.airline+" "+f.number+" · "+f.origin+" to "+f.destination+" · "+search.date;
document.getElementById("fareTotal").textContent="Select "+search.passengers+" seat(s).";
renderSeats();show("booking");
}
function renderSeats(){
let f=flights.find(x=>x.id===selectedFlightId);if(!f)return;
let c=document.getElementById("travelClass").value;
let booked=bookings.filter(b=>b.flightId===f.id&&b.cabin===c&&b.status==="Confirmed"&&b.date===search.date).flatMap(b=>b.seats);
document.getElementById("seatGrid").innerHTML=Array.from({length:f.capacity},(_,i)=>{
let n=i+1,taken=booked.includes(n);
return `<button type="button" class="seat ${taken?"booked":selectedSeats.includes(n)?"selected":""}" ${taken?"disabled":""} onclick="toggleSeat(${n})">${n}</button>`;
}).join("");
document.getElementById("seatSummary").textContent="Selected seats: "+(selectedSeats.join(", ")||"None");
document.getElementById("fareTotal").textContent="Total: "+money(fare(f,c)*selectedSeats.length);
}
function toggleSeat(n){
if(selectedSeats.includes(n))selectedSeats=selectedSeats.filter(x=>x!==n);
else if(selectedSeats.length<search.passengers)selectedSeats.push(n);
else alert("You selected the requested number of passengers already.");
renderSeats();
}
document.getElementById("travelClass").addEventListener("change",()=>{selectedSeats=[];renderSeats();});
function bookFlight(e){
e.preventDefault();
if(!currentUser||!search||!selectedFlightId)return;
if(selectedSeats.length!==search.passengers){alert("Select one seat for each passenger.");return;}
let f=flights.find(x=>x.id===selectedFlightId),c=document.getElementById("travelClass").value;
if(selectedSeats.some(n=>bookings.filter(b=>b.flightId===f.id&&b.cabin===c&&b.status==="Confirmed"&&b.date===search.date).some(b=>b.seats.includes(n)))){
alert("A seat is no longer available. Please select again.");renderSeats();return;}
let ref="SKY"+Date.now().toString().slice(-9);
bookings.push({ref,email:currentUser.email,passenger:document.getElementById("passengerName").value.trim(),
age:Number(document.getElementById("passengerAge").value),passengerEmail:document.getElementById("passengerEmail").value.trim(),
flightId:f.id,origin:f.origin,destination:f.destination,date:search.date,time:f.time,
cabin:c,seats:[...selectedSeats],fare:fare(f,c)*selectedSeats.length,
payment:document.getElementById("paymentMethod").value,status:"Confirmed"});
save();selectedSeats=[];alert("Demo reservation created. Reference: "+ref+". No payment or email was processed.");show("mytickets");
}
function cancelFlight(ref,admin=false){
let b=bookings.find(x=>x.ref===ref);
if(!b||b.status!=="Confirmed")return;
if(!admin&&b.email!==currentUser?.email){alert("You can cancel only your own booking.");return;}
if(!confirm("Cancel this demo booking?"))return;
b.status="Cancelled";b.refund=Math.round(b.fare*.8);
b.refundStatus=admin?"Refund approved in demo":"Refund pending admin approval";
save();renderAll();alert("Demo cancellation recorded. Estimated refund: "+money(b.refund)+". No money was transferred.");
}
function renderTickets(){
let mine=currentUser?bookings.filter(b=>b.email===currentUser.email):[];
document.getElementById("welcome").textContent=currentUser?"Welcome, "+currentUser.name:"Please log in to view tickets.";
document.getElementById("ticketList").innerHTML=mine.map(b=>{
let f=flights.find(x=>x.id===b.flightId);
return `<div class="ticket"><h3>✈️ SkyBook E-Ticket</h3>
<p><b>Booking Reference:</b> ${b.ref}</p><p><b>Passenger:</b> ${esc(b.passenger)} · Age ${b.age}</p>
<p><b>Flight:</b> ${esc(f?f.airline:"Airline")} (${esc(f?f.number:"")})</p>
<p><b>Route:</b> ${esc(b.origin)} → ${esc(b.destination)}</p>
<p><b>Departure:</b> ${b.date} · ${esc(b.time)}</p><p><b>Cabin:</b> ${esc(b.cabin)} · Seats: ${b.seats.join(", ")}</p>
<p><b>Fare:</b> ${money(b.fare)} · <b>Status:</b> ${b.status}</p>
${b.status==="Cancelled"?`<p>${esc(b.refundStatus||"Refund pending")}: ${money(b.refund||0)}</p>`:""}
<div class="no-print" style="margin-top:12px"><button class="primary" onclick="printTicket('${b.ref}')">Print E-Ticket</button>
${b.status==="Confirmed"?`<button class="danger" onclick="cancelFlight('${b.ref}')">Cancel Ticket</button>`:""}</div>
</div><br>`;
}).join("")||"<p>No bookings yet.</p>";
}
function printTicket(ref){
if(!currentUser)return;
let mine=bookings.filter(b=>b.email===currentUser.email);
let index=mine.findIndex(b=>b.ref===ref);if(index<0)return;
let els=[...document.querySelectorAll("#ticketList .ticket")];
els.forEach((el,i)=>el.style.display=i===index?"block":"none");
window.print();els.forEach(el=>el.style.display="block");
}
function checkStatus(e){
e.preventDefault();let ref=document.getElementById("referenceInput").value.trim();
let b=bookings.find(x=>x.ref===ref);
document.getElementById("statusResult").innerHTML=b?
`<div class="card"><h3>Reference: ${b.ref}</h3><p>Passenger: ${esc(b.passenger)}</p><p>Flight date: ${b.date}</p><p>Status: <b>${b.status}</b></p><p>Refund: ${money(b.refund||0)} · ${esc(b.refundStatus||"Not applicable")}</p></div>`:"<p>No matching booking reference found.</p>";
}
function adminLogin(e){
e.preventDefault();
if(document.getElementById("adminEmail").value==="admin@skybook.com"&&document.getElementById("adminPassword").value==="admin123"){
isAdmin=true;document.getElementById("adminPanel").style.display="block";
document.getElementById("adminStatus").textContent="Demo admin login successful.";renderAdmin();
}else alert("Incorrect demo admin credentials.");
}
function addFlight(e){
e.preventDefault();if(!isAdmin)return;
flights.push({id:flights.length?Math.max(...flights.map(f=>f.id))+1:1,
airline:document.getElementById("airline").value.trim(),number:document.getElementById("flightNumber").value.trim(),
origin:document.getElementById("flightOrigin").value.trim(),destination:document.getElementById("flightDestination").value.trim(),
time:document.getElementById("departureTime").value,fare:Number(document.getElementById("economyFare").value),
capacity:Number(document.getElementById("capacity").value)});
save();e.target.reset();renderAll();alert("Flight added.");
}
function editFlight(id){
if(!isAdmin)return;let f=flights.find(x=>x.id===id);if(!f)return;
let price=prompt("New economy fare:",f.fare);if(price===null)return;
let cap=prompt("New seat capacity:",f.capacity);if(cap===null)return;
price=Number(price);cap=Number(cap);
let maxBooked=bookings.filter(b=>b.flightId===id&&b.status==="Confirmed").reduce((n,b)=>n+b.seats.length,0);
if(price<=0||!Number.isFinite(price)||!Number.isInteger(cap)||cap<maxBooked||cap<1){alert("Invalid fare or capacity below existing reservations.");return;}
f.fare=price;f.capacity=cap;save();renderAll();
}
function deleteFlight(id){
if(!isAdmin)return;
if(bookings.some(b=>b.flightId===id&&b.status==="Confirmed")){alert("Cancel active reservations first.");return;}
if(confirm("Delete flight?")){flights=flights.filter(f=>f.id!==id);save();renderAll();}
}
function renderAdmin(){
document.getElementById("adminFlightRows").innerHTML=flights.map(f=>`
<tr><td>${esc(f.number)}</td><td>${esc(f.airline)}</td><td>${esc(f.origin)} → ${esc(f.destination)}</td>
<td>${esc(f.time)}</td><td>${money(f.fare)}</td><td><button onclick="editFlight(${f.id})">Edit</button>
<button class="danger" onclick="deleteFlight(${f.id})">Delete</button></td></tr>`).join("");
document.getElementById("adminBookingRows").innerHTML=bookings.map(b=>`
<tr><td>${b.ref}</td><td>${esc(b.passenger)}</td><td>${esc((flights.find(f=>f.id===b.flightId)||{}).number||"Flight")}</td>
<td>${b.date}</td><td>${esc(b.cabin)}</td><td>${b.status}</td><td>${b.status==="Confirmed"?`<button class="danger" onclick="cancelFlight('${b.ref}',true)">Cancel</button>`:
b.status==="Cancelled"&&b.refundStatus!=="Refund approved in demo"?`<button class="primary" onclick="approveRefund('${b.ref}')">Approve Refund</button>`:"—"}</td></tr>`).join("")||"<tr><td colspan='7'>No bookings yet.</td></tr>";
document.getElementById("reportSummary").textContent="Flights: "+flights.length+" · Users: "+users.length+" · Bookings: "+bookings.length+" · Confirmed: "+bookings.filter(b=>b.status==="Confirmed").length+" · Cancelled: "+bookings.filter(b=>b.status==="Cancelled").length;
}
function approveRefund(ref){
if(!isAdmin)return;let b=bookings.find(x=>x.ref===ref);if(!b||b.status!=="Cancelled")return;
b.refundStatus="Refund approved in demo";save();renderAll();alert("Demo refund approved; no money was transferred.");
}
function submitFeedback(e){
e.preventDefault();if(!currentUser){alert("Please log in to leave feedback.");show("account");return;}
feedback.push({email:currentUser.email,rating:Number(document.getElementById("rating").value),
text:document.getElementById("feedbackText").value});
save();e.target.reset();renderFeedback();alert("Feedback saved in this browser.");
}
function renderFeedback(){
document.getElementById("feedbackList").innerHTML=feedback.map(f=>`<div class="card"><b>${"★".repeat(f.rating)}</b><p>${esc(f.text)}</p></div>`).join("")||"<p>No feedback yet.</p>";
}
function downloadReport(){
if(!isAdmin)return;
let rows=[["Reference","Passenger","Email","Flight","Date","Class","Fare","Status","Refund"],
...bookings.map(b=>[b.ref,b.passenger,b.email,b.flightId,b.date,b.cabin,b.fare,b.status,b.refund||0])];
let csv=rows.map(r=>r.map(v=>'"'+String(v).replace(/"/g,'""')+'"').join(",")).join("\r\n");
let url=URL.createObjectURL(new Blob([csv],{type:"text/csv;charset=utf-8"}));
let a=document.createElement("a");a.href=url;a.download="flight-booking-report.csv";a.click();URL.revokeObjectURL(url);
}
function renderAll(){renderFlights();renderTickets();renderAdmin();renderFeedback();}
renderAll();
</script>
</body>
</html>
