<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>StayEase | Hotel Booking</title>
<style>
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:Arial,sans-serif;background:#f5f7fb;color:#172033}
header{background:#123b35;color:white;padding:22px 6%;
display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:12px}
nav{display:flex;gap:8px;flex-wrap:wrap;background:white;padding:14px 5%;
box-shadow:0 2px 8px #ddd}
button{cursor:pointer;border:0;border-radius:6px;padding:11px 15px;font-weight:bold}
nav button{background:#eaf2ef;color:#123b35}
.primary{background:#16765e;color:white}
.danger{background:#b42318;color:white}
.container{width:92%;max-width:1150px;margin:28px auto}
.page{display:none}.page.active{display:block}
.hero{background:linear-gradient(120deg,#123b35,#27856b);
color:white;padding:38px;border-radius:14px;margin-bottom:24px}
.hero h2{font-size:34px;margin-bottom:12px}
.hero p{line-height:1.6}
.card{background:white;padding:22px;border-radius:12px;
box-shadow:0 3px 12px #0000000c;margin-bottom:18px}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(230px,1fr));gap:18px}
.room-icon{font-size:46px;margin-bottom:12px}
h2,h3{margin-bottom:14px}
p{line-height:1.6}
input,select,textarea{width:100%;padding:12px;margin:7px 0 15px;
border:1px solid #ccd4dc;border-radius:6px;font-size:15px}
label{font-weight:bold}
.muted{color:#667085}
.price{font-size:24px;color:#16765e;font-weight:bold;margin:12px 0}
.tag{display:inline-block;background:#e5f5ee;color:#126448;
padding:5px 8px;border-radius:5px;margin:6px 0}
table{width:100%;border-collapse:collapse;background:white;min-width:600px}
th,td{text-align:left;padding:12px;border-bottom:1px solid #e5e7eb}
th{background:#eaf2ef}
.table-wrap{overflow-x:auto}
footer{background:#123b35;color:white;text-align:center;padding:25px;margin-top:35px}
@media(max-width:600px){.hero{padding:24px}.hero h2{font-size:26px}
.container{width:94%}header{padding:20px}}
</style>
</head>
<body>

<header>
  <h1>🏨 StayEase</h1>
  <span>Your comfortable stay starts here</span>
</header>

<nav>
  <button onclick="show('home')">Home</button>
  <button onclick="show('rooms')">Rooms</button>
  <button onclick="show('account')">Login / Register</button>
  <button onclick="show('dashboard')">My Bookings</button>
  <button onclick="show('admin')">Admin</button>
  <button onclick="show('about')">About Us</button>
  <button onclick="show('contact')">Contact & FAQ</button>
  <button onclick="show('policy')">Privacy Policy</button>
</nav>

<main class="container">

<section id="home" class="page active">
  <div class="hero">
    <h2>Find your perfect stay</h2>
    <p>Explore rooms, check availability and manage your hotel bookings.</p>
  </div>

  <div class="card">
    <h2>Search Available Rooms</h2>
    <form onsubmit="searchRooms(event)">
      <label>Destination</label>
      <input id="destination" placeholder="e.g. Chennai" required>
      <div class="grid">
        <div><label>Check-in</label>
          <input id="checkin" type="date" required></div>
        <div><label>Check-out</label>
          <input id="checkout" type="date" required></div>
        <div><label>Guests</label>
          <input id="guests" type="number" min="1" max="10" value="2" required>
        </div>
      </div>
      <button class="primary">Search Rooms</button>
    </form>
    <p id="searchMessage" class="muted"></p>
  </div>

  <h2>Our Room Types</h2>
  <div id="homeRooms" class="grid"></div>
</section>

<section id="rooms" class="page">
  <h2>Available Room Types</h2>
  <p class="muted">Select your dates on the home page before booking.</p>
  <div id="roomList" class="grid"></div>
</section>

<section id="account" class="page">
  <div class="grid">
    <div class="card">
      <h2>User Registration</h2>
      <form onsubmit="register(event)">
        <label>Full Name</label>
        <input id="regName" required>
        <label>Email</label>
        <input id="regEmail" type="email" required>
        <label>Password</label>
        <input id="regPassword" type="password" minlength="6" required>
        <button class="primary">Register</button>
      </form>
    </div>
    <div class="card">
      <h2>User Login</h2>
      <form onsubmit="login(event)">
        <label>Email</label>
        <input id="loginEmail" type="email" required>
        <label>Password</label>
        <input id="loginPassword" type="password" required>
        <button class="primary">Login</button>
      </form>
      <p id="loginStatus" class="muted"></p>
      <button onclick="logout()">Logout</button>
      <hr style="margin:20px 0">
      <h3>Update Profile</h3>
      <form onsubmit="updateProfile(event)">
        <label>New Full Name</label>
        <input id="profileName" required>
        <label>New Email</label>
        <input id="profileEmail" type="email" required>
        <button class="primary">Save Profile</button>
      </form>
    </div>
  </div>
</section>

<section id="dashboard" class="page">
  <div class="card">
    <h2>My Booking History</h2>
    <p id="welcome" class="muted">Please log in to view your bookings.</p>
    <div class="table-wrap">
      <table>
        <thead><tr>
          <th>Booking ID</th><th>Room</th><th>Destination</th>
          <th>Dates</th><th>Amount</th><th>Status</th><th>Action</th>
        </tr></thead>
        <tbody id="bookingRows"></tbody>
      </table>
    </div>
  </div>
</section>

<section id="admin" class="page">
  <div class="card">
    <h2>Administrator Login</h2>
    <p class="muted">Demo only: admin@stayease.com / admin123</p>
    <form onsubmit="adminLogin(event)">
      <label>Email</label><input id="adminEmail" type="email" required>
      <label>Password</label><input id="adminPassword" type="password" required>
      <button class="primary">Admin Login</button>
    </form>
    <p id="adminStatus" class="muted"></p>
  </div>

  <div id="adminPanel" style="display:none">
    <div class="card">
      <h2>Manage Room Inventory and Prices</h2>
      <div class="table-wrap">
        <table><thead><tr>
          <th>Room Type</th><th>Price/Night (₹)</th>
          <th>Total Rooms</th><th>Save</th>
        </tr></thead><tbody id="adminRooms"></tbody></table>
      </div>
    </div>
    <div class="card">
      <h2>Manage Bookings</h2>
      <div class="table-wrap">
        <table><thead><tr>
          <th>ID</th><th>Guest</th><th>Room</th>
          <th>Dates</th><th>Status</th><th>Action</th>
        </tr></thead><tbody id="adminBookings"></tbody></table>
      </div>
    </div>
    <div class="card">
      <h2>Edit Website Information</h2>
      <form onsubmit="saveInfo(event)">
        <label>About Us</label><textarea id="editAbout" rows="3"></textarea>
        <label>Contact Information</label><textarea id="editContact" rows="2"></textarea>
        <label>Customer Service FAQ</label><textarea id="editFaq" rows="3"></textarea>
        <label>Local Travel Guide</label><textarea id="editTravel" rows="3"></textarea>
        <label>Privacy Policy</label><textarea id="editPolicy" rows="3"></textarea>
        <button class="primary">Save Website Information</button>
      </form>
    </div>
  </div>
</section>

<section id="about" class="page">
  <div class="card">
    <h2>About StayEase</h2>
    <p id="aboutText">StayEase helps travellers find comfortable accommodation
    and manage their reservations conveniently.</p>
    <h3 style="margin-top:22px">Room Information</h3>
    <div id="aboutRooms" class="grid"></div>
    <h3>Local Travel Guide</h3>
    <p id="travelText">Ask the hotel team about local transport, sightseeing
    attractions and nearby restaurants before travelling.</p>
  </div>
</section>

<section id="contact" class="page">
  <div class="card">
    <h2>Contact Us</h2>
    <p id="contactText">Email: support@stayease.example</p>
    <p class="muted">Replace this sample contact information with your real hotel details.</p>
    <h2 style="margin-top:24px">Customer Service FAQ</h2>
    <p id="faqText">Check-in and check-out times depend on hotel policy.
    Contact the property for special requests and cancellation questions.</p>
    <form onsubmit="contactUs(event)">
      <label>Your Name</label><input id="contactName" required>
      <label>Email</label><input id="contactEmail" type="email" required>
      <label>Message</label><textarea id="contactMessage" required></textarea>
      <button class="primary">Submit Message</button>
    </form>
  </div>
</section>

<section id="policy" class="page">
  <div class="card">
    <h2>Privacy Policy</h2>
    <p id="policyText">A real hotel must explain how personal data is collected,
    used, protected, retained and deleted. This sample text is not a complete
    legal privacy policy. Do not submit real payment or identity information
    to this demonstration website.</p>
  </div>
</section>

</main>

<footer>
  <p>© 2026 StayEase Hotel Booking Website</p>
  <p style="margin-top:8px">Demo project for web development</p>
</footer>

<script>
const initialRooms = [
  {id:1,name:"Standard Room",price:1800,quantity:10,capacity:2,icon:"🛏️",
   amenities:"Wi-Fi, Air Conditioning, TV"},
  {id:2,name:"Deluxe Room",price:2800,quantity:8,capacity:3,icon:"🌇",
   amenities:"Wi-Fi, TV, City View, Breakfast"},
  {id:3,name:"Suite Room",price:4500,quantity:5,capacity:4,icon:"🛋️",
   amenities:"Living Area, Wi-Fi, Breakfast, King Bed"},
  {id:4,name:"Family Room",price:3800,quantity:6,capacity:5,icon:"👨‍👩‍👧‍👦",
   amenities:"Extra Beds, Wi-Fi, TV, Family Space"}
];

let rooms = JSON.parse(localStorage.getItem("hotelRooms")) || initialRooms;
let users = JSON.parse(localStorage.getItem("hotelUsers")) || [];
let bookings = JSON.parse(localStorage.getItem("hotelBookings")) || [];
let currentUser = null;
let isAdmin = false;
let search = null;

let info = JSON.parse(localStorage.getItem("hotelInfo")) || {
 about:"StayEase helps travellers find comfortable accommodation and manage reservations.",
 contact:"Email: support@stayease.example",
 faq:"Check-in and check-out depend on hotel policy. Contact the hotel for assistance.",
 travel:"Ask the hotel about local transport, attractions and nearby restaurants.",
 policy:"A real privacy policy must explain data collection, use, protection and retention."
};

function persist(){
 localStorage.setItem("hotelRooms",JSON.stringify(rooms));
 localStorage.setItem("hotelUsers",JSON.stringify(users));
 localStorage.setItem("hotelBookings",JSON.stringify(bookings));
 localStorage.setItem("hotelInfo",JSON.stringify(info));
}

function safe(s){
 return String(s).replace(/[&<>"']/g,c=>({
 "&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#39;"
 }[c]));
}

function show(id){
 document.querySelectorAll(".page").forEach(p=>p.classList.remove("active"));
 document.getElementById(id).classList.add("active");
 renderAll();
 window.scrollTo(0,0);
}

function dateValue(id){return document.getElementById(id).value}
function today(){
 let d=new Date();
 return d.getFullYear()+"-"+String(d.getMonth()+1).padStart(2,"0")+"-"+String(d.getDate()).padStart(2,"0");
}
function nights(a,b){
 return Math.round((new Date(b+"T12:00:00")-new Date(a+"T12:00:00"))/86400000);
}
function money(n){return "₹"+Number(n).toLocaleString("en-IN")}

function occupied(roomId,checkin,checkout,excludeId=null){
 return bookings.filter(b=>b.roomId===roomId&&b.status==="Confirmed"&&
 b.id!==excludeId&&b.checkin<checkout&&b.checkout>checkin).length;
}

function available(room,checkin,checkout){
 return room.quantity-occupied(room.id,checkin,checkout);
}

function renderRooms(target="roomList"){
 let filtered=rooms;
 if(search){
  filtered=rooms.filter(r=>r.capacity>=search.guests&&
   available(r,search.checkin,search.checkout)>0);
 }
 document.getElementById(target).innerHTML=filtered.map(r=>`
  <div class="card">
   <div class="room-icon">${r.icon}</div>
   <h3>${safe(r.name)}</h3>
   <p class="muted">${safe(r.amenities)}</p>
   <p>Maximum guests: ${r.capacity}</p>
   <p class="price">${money(r.price)} <small>/ night</small></p>
   ${search?`<span class="tag">${available(r,search.checkin,search.checkout)} available</span>`:
    `<span class="tag">${r.quantity} total rooms</span>`}
   <br><br><button class="primary" onclick="bookRoom(${r.id})">Book Room</button>
  </div>`).join("")||"<p>No rooms match your search.</p>";
}

function searchRooms(e){
 e.preventDefault();
 let checkin=dateValue("checkin"),checkout=dateValue("checkout");
 let guests=Number(document.getElementById("guests").value);
 if(checkin<today()||nights(checkin,checkout)<1){
  alert("Choose a valid check-in date and a check-out date at least one day later.");return;
 }
 search={destination:document.getElementById("destination").value.trim(),
  checkin,checkout,guests};
 document.getElementById("searchMessage").textContent=
  "Showing rooms for "+search.destination+" · "+nights(checkin,checkout)+" night(s).";
 renderRooms("homeRooms");renderRooms("roomList");
}

function register(e){
 e.preventDefault();
 let name=document.getElementById("regName").value.trim();
 let email=document.getElementById("regEmail").value.trim().toLowerCase();
 let password=document.getElementById("regPassword").value;
 if(users.some(u=>u.email===email)){alert("Email already registered.");return;}
 users.push({name,email,password});
 persist();e.target.reset();
 alert("Registration successful. Please log in.");
}

function login(e){
 e.preventDefault();
 let email=document.getElementById("loginEmail").value.trim().toLowerCase();
 let password=document.getElementById("loginPassword").value;
 let user=users.find(u=>u.email===email&&u.password===password);
 if(!user){alert("Invalid login. Register first if you do not have an account.");return;}
 currentUser={name:user.name,email:user.email};
 document.getElementById("loginStatus").textContent="Logged in as "+user.name;
 show("dashboard");
}

function logout(){
 currentUser=null;
 document.getElementById("loginStatus").textContent="Logged out.";
 show("home");
}

function updateProfile(e){
 e.preventDefault();
 if(!currentUser){alert("Please log in first.");return;}
 let name=document.getElementById("profileName").value.trim();
 let email=document.getElementById("profileEmail").value.trim().toLowerCase();
 if(users.some(u=>u.email===email&&u.email!==currentUser.email)){
  alert("That email is already in use.");return;
 }
 let user=users.find(u=>u.email===currentUser.email);
 if(!user)return;
 user.name=name;user.email=email;
 currentUser={name,email};
 persist();alert("Profile updated.");
}

function bookRoom(id){
 if(!currentUser){alert("Please register and log in before booking.");show("account");return;}
 if(!search){
  alert("Search by destination and dates on the Home page first.");show("home");return;
 }
 let room=rooms.find(r=>r.id===id);
 if(!room||room.capacity<search.guests||
 available(room,search.checkin,search.checkout)<1){
  alert("No rooms available for these dates.");return;
 }
 let total=nights(search.checkin,search.checkout)*room.price;
 if(!confirm("Confirm "+room.name+" for "+money(total)+"? This demo does not take payment."))return;
 bookings.push({id:Date.now(),email:currentUser.email,roomId:id,
 destination:search.destination,checkin:search.checkin,checkout:search.checkout,
 guests:search.guests,total,status:"Confirmed"});
 persist();renderAll();show("dashboard");
 alert("Demo booking confirmed. No payment was processed and no email was sent.");
}

function canChange(b){
 let arrival=new Date(b.checkin+"T00:00:00");
 let cutoff=new Date(arrival);
 cutoff.setDate(cutoff.getDate()-3);
 return today()<=cutoff.toISOString().slice(0,10);
}

function cancelBooking(id,admin=false){
 let b=bookings.find(x=>x.id===id);
 if(!b||b.status!=="Confirmed")return;
 if(!admin&&!canChange(b)){
  alert("Bookings can only be cancelled at least 3 days before arrival.");return;
 }
 if(!confirm("Cancel this booking?"))return;
 b.status="Cancelled";persist();renderAll();
}

function modifyBooking(id){
 let b=bookings.find(x=>x.id===id);
 if(!b||!canChange(b)){alert("Changes are only allowed at least 3 days before arrival.");return;}
 let ci=prompt("New check-in date (YYYY-MM-DD):",b.checkin);
 if(!ci)return;
 let co=prompt("New check-out date (YYYY-MM-DD):",b.checkout);
 if(!co)return;
 if(ci<today()||nights(ci,co)<1){alert("Invalid dates.");return;}
 let room=rooms.find(r=>r.id===b.roomId);
 if(available(room,ci,co,b.id)<1){alert("No rooms available for those dates.");return;}
 b.checkin=ci;b.checkout=co;b.total=nights(ci,co)*room.price;
 persist();renderAll();alert("Booking updated. No payment adjustment is processed in this demo.");
}

function renderBookings(){
 let mine=currentUser?bookings.filter(b=>b.email===currentUser.email):[];
 document.getElementById("welcome").textContent=currentUser?
 "Welcome, "+currentUser.name:"Please log in to view your bookings.";
 document.getElementById("bookingRows").innerHTML=mine.map(b=>{
  let r=rooms.find(x=>x.id===b.roomId);
  return `<tr><td>${b.id}</td><td>${safe(r?r.name:"Room")}</td>
  <td>${safe(b.destination)}</td><td>${b.checkin} to ${b.checkout}</td>
  <td>${money(b.total)}</td><td>${b.status}</td><td>
  ${b.status==="Confirmed"?`<button onclick="modifyBooking(${b.id})">Modify</button>
  <button class="danger" onclick="cancelBooking(${b.id})">Cancel</button>`:"—"}
  </td></tr>`;
 }).join("")||'<tr><td colspan="7">No bookings found.</td></tr>';
}

function adminLogin(e){
 e.preventDefault();
 if(document.getElementById("adminEmail").value==="admin@stayease.com"&&
 document.getElementById("adminPassword").value==="admin123"){
  isAdmin=true;
  document.getElementById("adminPanel").style.display="block";
  document.getElementById("adminStatus").textContent="Demo admin access enabled.";
  renderAdmin();
 }else alert("Incorrect demo administrator credentials.");
}

function renderAdmin(){
 document.getElementById("adminRooms").innerHTML=rooms.map(r=>`
 <tr><td>${safe(r.name)}</td><td><input type="number" min="1" id="price${r.id}" value="${r.price}"></td>
 <td><input type="number" min="1" id="qty${r.id}" value="${r.quantity}"></td>
 <td><button class="primary" onclick="saveRoom(${r.id})">Save</button></td></tr>`).join("");
 document.getElementById("adminBookings").innerHTML=bookings.map(b=>{
  let r=rooms.find(x=>x.id===b.roomId);
  return `<tr><td>${b.id}</td><td>${safe(b.email)}</td>
  <td>${safe(r?r.name:"Room")}</td><td>${b.checkin} to ${b.checkout}</td>
  <td>${b.status}</td><td>${b.status==="Confirmed"?
  `<button class="danger" onclick="cancelBooking(${b.id},true)">Cancel</button>`:"—"}</td></tr>`;
 }).join("")||'<tr><td colspan="6">No bookings yet.</td></tr>';
}

function saveRoom(id){
 if(!isAdmin)return;
 let r=rooms.find(x=>x.id===id);
 let price=Number(document.getElementById("price"+id).value);
 let qty=Number(document.getElementById("qty"+id).value);
 let maxOccupied=bookings.filter(b=>b.roomId===id&&b.status==="Confirmed")
 .reduce((max,b)=>Math.max(max,occupied(id,b.checkin,b.checkout)),0);
 if(!Number.isFinite(price)||price<=0||!Number.isInteger(qty)||qty<maxOccupied){
  alert("Invalid price or quantity below existing bookings.");return;
 }
 r.price=price;r.quantity=qty;persist();renderAll();alert("Room updated.");
}

function saveInfo(e){
 e.preventDefault();if(!isAdmin)return;
 info={
  about:document.getElementById("editAbout").value,
  contact:document.getElementById("editContact").value,
  faq:document.getElementById("editFaq").value,
  travel:document.getElementById("editTravel").value,
  policy:document.getElementById("editPolicy").value
 };
 persist();renderInfo();alert("Website information saved.");
}

function renderInfo(){
 document.getElementById("aboutText").textContent=info.about;
 document.getElementById("contactText").textContent=info.contact;
 document.getElementById("faqText").textContent=info.faq;
 document.getElementById("travelText").textContent=info.travel;
 document.getElementById("policyText").textContent=info.policy;
 document.getElementById("editAbout").value=info.about;
 document.getElementById("editContact").value=info.contact;
 document.getElementById("editFaq").value=info.faq;
 document.getElementById("editTravel").value=info.travel;
 document.getElementById("editPolicy").value=info.policy;
}

function contactUs(e){
 e.preventDefault();
 alert("Demo only: the message was not sent. A backend and email service are needed.");
 e.target.reset();
}

function renderAll(){
 renderRooms("roomList");renderRooms("homeRooms");
 renderBookings();renderAdmin();renderInfo();
}
renderAll();
</script>
</body>
</html>
