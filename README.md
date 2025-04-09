# https-github.com-ReshmikaMendi-OnlineExamSystem
Key Features
✅ Multiple Subjects: Allow students to select from various exams.
✅ Instant Scoring: Auto-grade exams and display real-time results.
✅ Timed Questions: Set countdown timers for each question. 
✅ Review Mode: Students can revisit unanswered or flagged questions.
✅ Login & Authentication: Secure login with hashed passwords. 
✅ Admin Dashboard: Teachers can create, edit, and track exams.
✅ Responsive Design: Optimized for all devices.

####PROJECT STRUCTURE#####
/OnlineExamSystem
│── /public        → Frontend files (HTML, CSS, JS)
│── /routes        → API routes for exams & users
│── /models        → Database schemas
│── /controllers   → Business logic handling
│── server.js      → Main server file
│── .env           → Configuration variables
│── package.json   → Project dependencies


*****Tech Stack****
Frontend: HTML, CSS, JavaScript (Vanilla JS)

Backend: Node.js, Express.js

Database: MongoDB (For user data & test storage)

Authentication: JSON Web Token (JWT)

Security: bcrypt for password hashing, secure API routes

Backend (Node.js + Express)
server.js
JAVASCRIPT
const express = require('express');
const mongoose = require('mongoose');
const dotenv = require('dotenv');
const cors = require('cors');

dotenv.config();
const app = express();
app.use(express.json());
app.use(cors());

mongoose.connect(process.env.DB_URI, { useNewUrlParser: true, useUnifiedTopology: true })
    .then(() => console.log('Database connected'))
    .catch(err => console.error(err));

app.use('/api/users', require('./routes/userRoutes'));
app.use('/api/exams', require('./routes/examRoutes'));

const PORT = process.env.PORT || 5000;
app.listen(PORT, () => console.log(`Server running on port ${PORT}`));

const mongoose = require('mongoose');
const bcrypt = require('bcrypt');

const UserSchema = new mongoose.Schema({
    username: String,
    email: String,
    password: String
});

UserSchema.pre('save', async function(next) {
    this.password = await bcrypt.hash(this.password, 10);
    next();
});

module.exports = mongoose.model('User', UserSchema);
const mongoose = require('mongoose');

const QuestionSchema = new mongoose.Schema({
    subject: String,
    text: String,
    options: [String],
    correctOption: Number,
    timeLimit: Number
});

const ExamSchema = new mongoose.Schema({
    title: String,
    questions: [QuestionSchema],
    createdBy: String
});

module.exports = mongoose.model('Exam', ExamSchema);
Frontend (HTML + CSS + JavaScript) LOGIN PAGE
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Online Exam Login</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="login-container">
        <h2>Login</h2>
        <input type="text" id="username" placeholder="Username">
        <input type="password" id="password" placeholder="Password">
        <button onclick="login()">Login</button>
    </div>
    <script src="script.js"></script>
</body>
</html>
body {
    font-family: Arial, sans-serif;
    background: #f4f4f4;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}
.login-container {
    background: white;
    padding: 20px;
    border-radius: 5px;
    box-shadow: 0px 0px 10px rgba(0,0,0,0.1);
}
function login() {
    const username = document.getElementById('username').value;
    const password = document.getElementById('password').value;
    
    fetch('/api/users/login', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ username, password })
    }).then(res => res.json())
    .then(data => console.log(data));
}

