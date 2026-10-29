<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Library Management System</title>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: Arial, sans-serif;
}

body {
    background: #f4f6fb;
    color: #222;
}

header {
    background: #172554;
    color: white;
    padding: 22px 6%;
    display: flex;
    justify-content: space-between;
    align-items: center;
    flex-wrap: wrap;
    gap: 12px;
}

header h1 { font-size: 24px; }

nav {
    background: white;
    padding: 15px 5%;
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    box-shadow: 0 2px 8px #ddd;
}

nav button {
    background: transparent;
    color: #172554;
    border: none;
    padding: 10px;
    cursor: pointer;
    font-weight: bold;
}

nav button:hover { background: #e8edff; }

.container {
    width: 90%;
    max-width: 1200px;
    margin: 28px auto;
}

.page { display: none; }
.page.active { display: block; }

.hero {
    background: linear-gradient(120deg, #172554, #2563eb);
    color: white;
    padding: 35px;
    border-radius: 14px;
    margin-bottom: 25px;
}

.hero h2 { font-size: 30px; margin-bottom: 12px; }
.hero p { line-height: 1.6; }

input, select, textarea {
    width: 100%;
    padding: 12px;
    margin: 8px 0 15px;
    border: 1px solid #ccd2df;
    border-radius: 7px;
    font-size: 15px;
}

button.primary {
    background: #2563eb;
    color: white;
    border: none;
    border-radius: 7px;
    padding: 12px 18px;
    cursor: pointer;
}

button.primary:hover { background: #1d4ed8; }

button.danger {
    background: #dc2626;
    color: white;
    border: none;
    border-radius: 5px;
    padding: 8px 10px;
    cursor: pointer;
}

.stats, .book-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(210px, 1fr));
    gap: 18px;
    margin: 22px 0;
}

.card {
    background: white;
    padding: 22px;
    border-radius: 12px;
    box-shadow: 0 3px 12px #0000000d;
}

.stat-number {
    font-size: 28px;
    color: #2563eb;
    font-weight: bold;
    margin-top: 10px;
}

.book-card h3 { margin: 12px 0; }
.book-icon { font-size: 45px; }
.muted { color: #64748b; line-height: 1.6; }
form { max-width: 650px; }
h2, h3 { margin-bottom: 15px; }

table {
    width: 100%;
    border-collapse: collapse;
    background: white;
    margin-top: 15px;
}

th, td {
    text-align: left;
    padding: 12px;
    border-bottom: 1px solid #e5e7eb;
}

th { background: #e8edff; }

.table-wrap { overflow-x: auto; }

footer {
    text-align: center;
    padding: 25px;
    background: #172554;
    color: white;
    margin-top: 40px;
}

@media (max-width: 600px) {
    header { padding: 20px; }
    .container { width: 94%; }
    .hero { padding: 24px; }
    .hero h2 { font-size: 24px; }
}
</style>
</head>

<body>

<header>
    <h1>📚 Smart Library</h1>
    <span>Library Management System</span>
</header>

<nav>
    <button onclick="showPage('home')">Home</button>
    <button onclick="showPage('books')">Book Catalog</button>
    <button onclick="showPage('login')">Login / Register</button>
    <button onclick="showPage('student')">Student Dashboard</button>
    <button onclick="showPage('materials')">Study Materials</button>
    <button onclick="showPage('admin')">Admin Dashboard</button>
    <button onclick="showPage('contact')">Contact</button>
</nav>

<main class="container">

<!-- HOME -->
<section id="home" class="page active">
    <div class="hero">
        <h2>Welcome to Smart Library</h2>
        <p>Discover books, manage borrowing records, check fines,
        and access academic study materials in one place.</p>
        <br>
        <button class="primary" onclick="showPage('books')">
            Explore Books
        </button>
    </div>

    <div class="stats">
        <div class="card">
            <h3>Total Books</h3>
            <div class="stat-number" id="totalBooks">0</div>
        </div>
        <div class="card">
            <h3>Available Copies</h3>
            <div class="stat-number" id="availableBooks">0</div>
        </div>
        <div class="card">
            <h3>Borrowed Books</h3>
            <div class="stat-number" id="borrowedBooks">0</div>
        </div>
    </div>

    <div class="card">
        <h2>Why use Smart Library?</h2>
        <p class="muted">
            Search books quickly, view availability, track issue and
            return dates, check overdue fines, and download study materials.
        </p>
    </div>
</section>

<!-- BOOK CATALOG -->
<section id="books" class="page">
    <h2>Book Catalog</h2>
    <p class="muted">Search by title, author, or category.</p>

    <input id="searchInput" placeholder="Search books..."
           oninput="renderBooks()">

    <div id="bookList" class="book-grid"></div>
</section>

<!-- LOGIN AND REGISTER -->
<section id="login" class="page">
    <div class="card">
        <h2>Login</h2>
        <p class="muted">Demo student and librarian access.</p>

        <form onsubmit="loginUser(event)">
            <label>Email</label>
            <input id="email" type="email" required
                   placeholder="Enter email">

            <label>Password</label>
            <input id="password" type="password" required
                   placeholder="Enter password">

            <label>Login as</label>
            <select id="role">
                <option value="student">Student</option>
                <option value="admin">Librarian Admin</option>
            </select>

            <button class="primary" type="submit">Login</button>
        </form>

        <hr style="margin:25px 0">

        <h2>Student Registration</h2>
        <form onsubmit="registerUser(event)">
            <label>Full Name</label>
            <input id="regName" required placeholder="Enter your name">

            <label>Email</label>
            <input id="regEmail" type="email" required
                   placeholder="Enter your email">

            <label>Password</label>
            <input id="regPassword" type="password" minlength="6"
                   required placeholder="At least 6 characters">

            <button class="primary" type="submit">Register</button>
        </form>
    </div>
</section>

<!-- STUDENT DASHBOARD -->
<section id="student" class="page">
    <h2>Student Dashboard</h2>
    <div class="card">
        <h3 id="studentWelcome">Welcome, Student</h3>
        <p class="muted">Your borrowing history and outstanding fines.</p>
        <br>
        <button class="primary" onclick="showPage('books')">
            Search Books
        </button>
    </div>

    <h2 style="margin-top:25px">My Borrow History</h2>
    <div class="table-wrap">
        <table>
            <thead>
                <tr>
                    <th>Book</th>
                    <th>Issue Date</th>
                    <th>Due Date</th>
                    <th>Status</th>
                    <th>Fine</th>
                </tr>
            </thead>
            <tbody id="historyList"></tbody>
        </table>
    </div>

    <div class="card" style="margin-top:20px">
        <h3>Total Outstanding Fine</h3>
        <div class="stat-number" id="totalFine">₹0</div>
    </div>
</section>

<!-- STUDY MATERIALS -->
<section id="materials" class="page">
    <h2>Academic Study Materials</h2>
    <p class="muted">Filter sample resources by academic year and semester.</p>

    <div class="card">
        <label>Academic Year</label>
        <select id="yearFilter" onchange="renderMaterials()">
            <option value="all">All Years</option>
            <option value="1">First Year</option>
            <option value="2">Second Year</option>
            <option value="3">Third Year</option>
            <option value="4">Fourth Year</option>
        </select>

        <label>Semester</label>
        <select id="semesterFilter" onchange="renderMaterials()">
            <option value="all">All Semesters</option>
            <option value="1">Semester 1</option>
            <option value="2">Semester 2</option>
            <option value="3">Semester 3</option>
            <option value="4">Semester 4</option>
            <option value="5">Semester 5</option>
            <option value="6">Semester 6</option>
            <option value="7">Semester 7</option>
            <option value="8">Semester 8</option>
        </select>

        <div id="materialList"></div>
    </div>
</section>

<!-- ADMIN DASHBOARD -->
<section id="admin" class="page">
    <h2>Librarian Admin Dashboard</h2>

    <div class="card">
        <h3>Add a Book</h3>
        <form onsubmit="addBook(event)">
            <label>Book Title</label>
            <input id="newTitle" required placeholder="Book title">

            <label>Author</label>
            <input id="newAuthor" required placeholder="Author name">

            <label>Category</label>
            <input id="newCategory" required placeholder="e.g. Computer Science">

            <label>Number of Copies</label>
            <input id="newQuantity" type="number" min="1" value="1" required>

            <button class="primary" type="submit">Add Book</button>
        </form>
    </div>

    <div class="card" style="margin-top:20px">
        <h3>Issue a Book to a Student</h3>
        <form onsubmit="issueBook(event)">
            <label>Student Email</label>
            <input id="issueEmail" type="email" required>

            <label>Book</label>
            <select id="issueBookId"></select>

            <button class="primary" type="submit">Issue Book</button>
        </form>
    </div>

    <h3 style="margin-top:25px">Inventory Management</h3>
    <div class="table-wrap">
        <table>
            <thead>
                <tr>
                    <th>ID</th>
                    <th>Title</th>
                    <th>Author</th>
                    <th>Available</th>
                    <th>Action</th>
                </tr>
            </thead>
            <tbody id="adminBookList"></tbody>
        </table>
    </div>

    <h3 style="margin-top:25px">Manage Returns and Fines</h3>
    <div class="table-wrap">
        <table>
            <thead>
                <tr>
                    <th>Student</th>
                    <th>Book</th>
                    <th>Due Date</th>
                    <th>Fine</th>
                    <th>Action</th>
                </tr>
            </thead>
            <tbody id="adminHistoryList"></tbody>
        </table>
    </div>

    <div class="card" style="margin-top:20px">
        <h3>Add Study Material</h3>
        <form onsubmit="addMaterial(event)">
            <label>Material Title</label>
            <input id="matTitle" required placeholder="Subject or PDF title">

            <label>Academic Year</label>
            <select id="matYear">
                <option value="1">First Year</option>
                <option value="2">Second Year</option>
                <option value="3">Third Year</option>
                <option value="4">Fourth Year</option>
            </select>

            <label>Semester</label>
            <select id="matSemester">
                <option value="1">Semester 1</option>
                <option value="2">Semester 2</option>
                <option value="3">Semester 3</option>
                <option value="4">Semester 4</option>
                <option value="5">Semester 5</option>
                <option value="6">Semester 6</option>
                <option value="7">Semester 7</option>
                <option value="8">Semester 8</option>
            </select>

            <label>PDF file</label>
            <input id="matFile" type="file" accept=".pdf">

            <button class="primary" type="submit">Add Material</button>
        </form>
    </div>
</section>

<!-- CONTACT -->
<section id="contact" class="page">
    <div class="card">
        <h2>Contact the Library</h2>
        <p class="muted">Send a question to the library team.</p>

        <form onsubmit="sendMessage(event)">
            <label>Your Name</label>
            <input id="contactName" required>

            <label>Email</label>
            <input id="contactEmail" type="email" required>

            <label>Message</label>
            <textarea id="contactMessage" rows="5" required></textarea>

            <button class="primary" type="submit">Send Message</button>
        </form>
    </div>
</section>

</main>

<footer>
    <p>© 2026 Smart Library Management System</p>
    <p style="margin-top:8px">Making library services simpler and smarter.</p>
</footer>

<script>
// Sample book data
let books = JSON.parse(localStorage.getItem("libraryBooks")) || [
    {id:1, title:"Introduction to Artificial Intelligence",
     author:"Stuart Russell", category:"AI", quantity:5, available:5},
    {id:2, title:"Python Programming",
     author:"Guido van Rossum", category:"Programming", quantity:4, available:4},
    {id:3, title:"Database Management Systems",
     author:"Raghu Ramakrishnan", category:"DBMS", quantity:3, available:3},
    {id:4, title:"Computer Networks",
     author:"Andrew Tanenbaum", category:"Networking", quantity:2, available:2}
];

let users = JSON.parse(localStorage.getItem("libraryUsers")) || [];
let records = JSON.parse(localStorage.getItem("libraryRecords")) || [];
let materials = JSON.parse(localStorage.getItem("libraryMaterials")) || [
    {id:1, title:"Python Programming Notes", year:"1",
     semester:"1", url:""},
    {id:2, title:"Database Management Notes", year:"2",
     semester:"3", url:""}
];

let currentUser = null;

function saveData() {
    localStorage.setItem("libraryBooks", JSON.stringify(books));
    localStorage.setItem("libraryUsers", JSON.stringify(users));
    localStorage.setItem("libraryRecords", JSON.stringify(records));
    localStorage.setItem("libraryMaterials", JSON.stringify(materials));
}

function showPage(id) {
    document.querySelectorAll(".page").forEach(page => {
        page.classList.remove("active");
    });
    document.getElementById(id).classList.add("active");
    renderAll();
    window.scrollTo(0, 0);
}

function renderBooks() {
    let search = document.getElementById("searchInput").value.toLowerCase();
    let filtered = books.filter(b =>
        (b.title + " " + b.author + " " + b.category)
        .toLowerCase().includes(search)
    );

    document.getElementById("bookList").innerHTML = filtered.map(b => `
        <div class="card book-card">
            <div class="book-icon">📘</div>
            <h3>${escapeHTML(b.title)}</h3>
            <p class="muted">Author: ${escapeHTML(b.author)}</p>
            <p class="muted">Category: ${escapeHTML(b.category)}</p>
            <p><b>Available: ${b.available} / ${b.quantity}</b></p>
            <br>
            <button class="primary" onclick="borrowRequest(${b.id})">
                Request Book
            </button>
        </div>
    `).join("") || "<p>No books found.</p>";
}

function loginUser(event) {
    event.preventDefault();
    let email = document.getElementById("email").value.trim().toLowerCase();
    let password = document.getElementById("password").value;
    let role = document.getElementById("role").value;

    // This is a frontend demonstration, not secure authentication.
    if (role === "admin") {
        if (email === "admin@library.com" && password === "admin123") {
            currentUser = {name:"Librarian Admin", email, role};
            alert("Demo admin login successful!");
            showPage("admin");
        } else {
            alert("Demo admin: use admin@library.com / admin123");
        }
        return;
    }

    let user = users.find(u =>
        u.email.toLowerCase() === email && u.password === password
    );

    if (!user) {
        alert("Student account not found. Please register first.");
        return;
    }

    currentUser = {name:user.name, email:user.email, role:"student"};
    alert("Login successful!");
    showPage("student");
}

function registerUser(event) {
    event.preventDefault();

    let name = document.getElementById("regName").value.trim();
    let email = document.getElementById("regEmail").value.trim().toLowerCase();
    let password = document.getElementById("regPassword").value;

    if (users.some(u => u.email.toLowerCase() === email)) {
        alert("This email is already registered.");
        return;
    }

    users.push({name, email, password});
    saveData();
    event.target.reset();
    alert("Registration successful! You can now log in.");
}

function borrowRequest(id) {
    if (!currentUser || currentUser.role !== "student") {
        alert("Please register and log in as a student first.");
        showPage("login");
        return;
    }

    let book = books.find(b => b.id === id);
    if (!book || book.available < 1) {
        alert("This book is currently unavailable.");
        return;
    }

    if (records.some(r => r.email === currentUser.email &&
        r.bookId === id && !r.returnDate)) {
        alert("You already have this book issued.");
        return;
    }

    alert("Request submitted. The librarian must issue the book from the admin dashboard.");
}

function addBook(event) {
    event.preventDefault();

    let title = document.getElementById("newTitle").value.trim();
    let author = document.getElementById("newAuthor").value.trim();
    let category = document.getElementById("newCategory").value.trim();
    let quantity = Number(document.getElementById("newQuantity").value);

    books.push({
        id: books.length ? Math.max(...books.map(b => b.id)) + 1 : 1,
        title, author, category, quantity, available:quantity
    });

    saveData();
    event.target.reset();
    renderAll();
    alert("Book added successfully!");
}

function issueBook(event) {
    event.preventDefault();

    let email = document.getElementById("issueEmail").value.trim().toLowerCase();
    let bookId = Number(document.getElementById("issueBookId").value);
    let book = books.find(b => b.id === bookId);

    if (!users.some(u => u.email.toLowerCase() === email)) {
        alert("Register the student before issuing a book.");
        return;
    }

    if (!book || book.available < 1) {
        alert("No available copies.");
        return;
    }

    if (records.some(r => r.email === email &&
        r.bookId === bookId && !r.returnDate)) {
        alert("This student already has this book.");
        return;
    }

    let today = new Date();
    let due = new Date();
    due.setDate(due.getDate() + 14);

    records.push({
        id:Date.now(), email, bookId,
        issueDate:formatDate(today),
        dueDate:formatDate(due),
        returnDate:""
    });

    book.available--;
    saveData();
    renderAll();
    alert("Book issued successfully. Due in 14 days.");
}

function returnBook(id) {
    let record = records.find(r => r.id === id);
    if (!record || record.returnDate) return;

    record.returnDate = formatDate(new Date());
    let book = books.find(b => b.id === record.bookId);
    if (book) book.available = Math.min(book.quantity, book.available + 1);

    saveData();
    renderAll();
    alert("Book returned successfully. Fine updated if overdue.");
}

function calculateFine(record) {
    let endDate = record.returnDate
        ? parseDate(record.returnDate) : new Date();

    let dueDate = parseDate(record.dueDate);
    let days = Math.max(0, Math.floor((endDate - dueDate) / 86400000));
    return days * 5;
}

function renderHistory() {
    let mine = currentUser && currentUser.role === "student"
        ? records.filter(r => r.email === currentUser.email) : [];

    document.getElementById("studentWelcome").textContent =
        currentUser ? "Welcome, " + currentUser.name : "Welcome, Student";

    document.getElementById("historyList").innerHTML = mine.map(r => {
        let book = books.find(b => b.id === r.bookId);
        return `<tr>
            <td>${escapeHTML(book ? book.title : "Deleted book")}</td>
            <td>${r.issueDate}</td>
            <td>${r.dueDate}</td>
            <td>${r.returnDate ? "Returned" : "Issued"}</td>
            <td>₹${r.returnDate ? 0 : calculateFine(r)}</td>
        </tr>`;
    }).join("") || '<tr><td colspan="5">No borrowing records.</td></tr>';

    document.getElementById("totalFine").textContent = "₹" +
        mine.filter(r => !r.returnDate).reduce((sum, r) => sum + calculateFine(r), 0);
}

function renderAdmin() {
    document.getElementById("adminBookList").innerHTML = books.map(b => `
        <tr>
            <td>${b.id}</td>
            <td>${escapeHTML(b.title)}</td>
            <td>${escapeHTML(b.author)}</td>
            <td>${b.available} / ${b.quantity}</td>
            <td>
                <button class="primary" onclick="editBook(${b.id})">Edit</button>
                <button class="danger" onclick="deleteBook(${b.id})">Delete</button>
            </td>
        </tr>
    `).join("");

    document.getElementById("issueBookId").innerHTML = books.map(b =>
        `<option value="${b.id}">${escapeHTML(b.title)} (${b.available} available)</option>`
    ).join("");

    document.getElementById("adminHistoryList").innerHTML = records.map(r => {
        let book = books.find(b => b.id === r.bookId);
        return `<tr>
            <td>${escapeHTML(r.email)}</td>
            <td>${escapeHTML(book ? book.title : "Deleted book")}</td>
            <td>${r.dueDate}</td>
            <td>₹${r.returnDate ? 0 : calculateFine(r)}</td>
            <td>${r.returnDate ? "Returned" :
                `<button class="primary" onclick="returnBook(${r.id})">Return</button>`}</td>
        </tr>`;
    }).join("") || '<tr><td colspan="5">No issued books.</td></tr>';
}

function editBook(id) {
    let b = books.find(b => b.id === id);
    if (!b) return;

    let title = prompt("Enter book title:", b.title);
    if (title === null) return;
    let author = prompt("Enter author:", b.author);
    if (author === null) return;
    let quantity = prompt("Total copies (cannot be below issued copies):", b.quantity);
    if (quantity === null) return;

    quantity = Number(quantity);
    let issued = b.quantity - b.available;

    if (!title.trim() || !author.trim() ||
        !Number.isInteger(quantity) || quantity < issued) {
        alert("Invalid details or quantity below the number of issued copies.");
        return;
    }

    b.title = title.trim();
    b.author = author.trim();
    b.quantity = quantity;
    b.available = quantity - issued;

    saveData();
    renderAll();
}

function deleteBook(id) {
    let b = books.find(b => b.id === id);
    if (!b) return;

    if (records.some(r => r.bookId === id && !r.returnDate)) {
        alert("Return all issued copies before deleting this book.");
        return;
    }

    if (confirm("Delete " + b.title + "?")) {
        books = books.filter(b => b.id !== id);
        saveData();
        renderAll();
    }
}

function addMaterial(event) {
    event.preventDefault();

    let file = document.getElementById("matFile").files[0];
    let title = document.getElementById("matTitle").value.trim();

    if (file && file.type !== "application/pdf") {
        alert("Please select a PDF file.");
        return;
    }

    // The selected PDF is only linked for this browser session.
    // Real uploads need a backend and file storage.
    let url = file ? URL.createObjectURL(file) : "";

    materials.push({
        id:Date.now(), title,
        year:document.getElementById("matYear").value,
        semester:document.getElementById("matSemester").value,
        url
    });

    event.target.reset();
    renderMaterials();
    alert("Material added for this session.");
}

function renderMaterials() {
    let year = document.getElementById("yearFilter").value;
    let semester = document.getElementById("semesterFilter").value;

    let filtered = materials.filter(m =>
        (year === "all" || m.year === year) &&
        (semester === "all" || m.semester === semester)
    );

    document.getElementById("materialList").innerHTML = filtered.map(m => `
        <div class="card" style="margin-top:15px">
            <h3>${escapeHTML(m.title)}</h3>
            <p class="muted">Year ${m.year} · Semester ${m.semester}</p>
            ${m.url ? `<a href="${m.url}" target="_blank" rel="noopener">Open PDF</a>`
                    : "<p class='muted'>Sample entry — no PDF attached.</p>"}
        </div>
    `).join("") || "<p>No materials found.</p>";
}

function sendMessage(event) {
    event.preventDefault();
    alert("Demo only: message form submitted. A backend is needed to deliver it to the librarian.");
    event.target.reset();
}

function formatDate(date) {
    return date.getFullYear() + "-" +
        String(date.getMonth()+1).padStart(2,"0") + "-" +
        String(date.getDate()).padStart(2,"0");
}

function parseDate(value) {
    let [y,m,d] = value.split("-").map(Number);
    return new Date(y, m-1, d);
}

function escapeHTML(value) {
    return String(value).replace(/[&<>"']/g, c => ({
        "&":"&amp;", "<":"&lt;", ">":"&gt;",
        '"':"&quot;", "'":"&#39;"
    }[c]));
}

function renderAll() {
    renderBooks();
    renderHistory();
    renderAdmin();
    renderMaterials();

    document.getElementById("totalBooks").textContent =
        books.reduce((sum,b) => sum + b.quantity, 0);
    document.getElementById("availableBooks").textContent =
        books.reduce((sum,b) => sum + b.available, 0);
    document.getElementById("borrowedBooks").textContent =
        records.filter(r => !r.returnDate).length;
}

renderAll();
</script>

</body>
</html>
