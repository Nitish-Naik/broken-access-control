# Broken Access Control Demo

This project demonstrates common vulnerabilities related to **Broken Access Control** in web applications. It includes examples of missing authentication, improper authorization, insecure direct object references (IDOR), and other access control flaws.

## Project Structure

## Features and Vulnerabilities

### 1. **Login Page (`index.html`)**
   - **Vulnerability #1: Client-Side Authentication**
     - Authentication is handled entirely on the client side, making it easy to bypass by directly accessing other pages (e.g., `dashboard.html`).

### 2. **Dashboard (`dashboard.html`)**
   - **Vulnerability #2: Missing Function-Level Access Control**
     - The dashboard is accessible without proper authentication checks.
   - **Vulnerability #3: Improper Authorization**
     - Links to sensitive pages (e.g., `secret.html`, `admin.html`) are visible to all users, regardless of their role.
   - **Vulnerability #4: Insecure Direct Object References (IDOR)**
     - Users can view or modify other users' notes by manipulating URL parameters (e.g., `?userId=admin`).

### 3. **Secret Page (`secret.html`)**
   - **Vulnerability #5: Missing Authentication Check**
     - This page is accessible to anyone who knows the URL, exposing sensitive admin and user notes.

### 4. **Note Details Page (`note.html`)**
   - **Vulnerability #6: IDOR**
     - Notes can be accessed by changing the `id` and `owner` parameters in the URL, without verifying ownership.

### 5. **Admin Panel (`admin.html`)**
   - **Vulnerability #7: Improper Authorization**
     - The admin panel only checks if the user is logged in but does not verify if the user has admin privileges.

## How to Run

1. Open the project in any web server or directly in your browser.
2. Start with `index.html` to log in as either:
   - **Admin**: `username: admin`, `password: 1234`
   - **User**: `username: user`, `password: user123`
3. Explore the vulnerabilities by navigating to different pages and manipulating URL parameters or localStorage.

## Secure Alternatives

Each page includes a **Secure Implementation** section in the code, demonstrating how to fix the vulnerabilities. Key recommendations include:
- Implementing server-side authentication and authorization.
- Validating user roles and permissions on the server.
- Avoiding reliance on client-side storage (e.g., `localStorage`) for sensitive data.
- Using secure session management.

## Disclaimer

This project is for educational purposes only. Do not use these insecure practices in production environments. Always follow secure coding guidelines to protect your applications from access control vulnerabilities.











develop a project that shows broken access controlEditBroken Access Control Demonstration Project
I'd be happy to develop a project that demonstrates broken access control vulnerabilities. Broken access control is one of the most common web application security flaws, where authorization mechanisms fail to properly restrict what actions users can perform.
Let's create a simple web application that demonstrates various broken access control vulnerabilities, along with explanations of the issues and how to fix them.
Broken Access Control DemonstrationCode 
Project Overview
This project demonstrates several common broken access control vulnerabilities in a simple Express.js web application. The application shows:

Missing Function-Level Access Control: The dashboard route doesn't verify authentication
Insecure Direct Object References (IDOR): Document access doesn't verify ownership
Improper API Authorization: Sensitive API endpoints missing role checks
Parameter Manipulation: The user ID can be manipulated in profile updates
Horizontal Privilege Escalation: Users can change roles without admin verification

How to Test the Vulnerabilities

Missing Authentication: Navigate directly to /dashboard without logging in
IDOR: Access documents by changing the ID in the URL (e.g., /documents/2 when logged in as a different user)
API Authorization: Send requests to /api/users without admin privileges
Parameter Manipulation: Modify the userId parameter in update requests
Privilege Escalation: Submit a request to /api/update-user-role as a regular user

Security Fixes Demonstration
The project also includes secure implementations showing how to properly implement access controls:

/secure/documents/:id: Proper ownership verification for document access
/secure/api/users: Proper role-based authorization check

To Complete This Project
To fully implement this demo, you would need to:

Create the EJS view templates mentioned in the comments
Set up a proper directory structure
Install the required dependencies (express, body-parser, express-session, ejs)

Would you like me to provide any additional components, such as the EJS templates or a setup guide to make this project fully functional?RetryPNi am a newbie can you give me step by stepEditStep-by-Step Guide to Create a Broken Access Control Demonstration Project
I'll help you set up this project from scratch, assuming you're new to web development. We'll go through each step carefully.
Step 1: Set up your development environment

Install Node.js and npm:

Download and install from nodejs.org
Verify installation by opening a terminal/command prompt and typing:
node -v
npm -v



Create a project folder:

Create a new folder named broken-access-control-demo
Open your terminal and navigate to this folder:
cd path/to/broken-access-control-demo




Step 2: Initialize your project

Initialize a new Node.js project:
npm init -y

Install required dependencies:
npm install express express-session body-parser ejs


Step 3: Create the project structure
Create these folders and files:
broken-access-control-demo/
  ├── public/               (for static files)
  │   └── styles.css
  ├── views/                (for EJS templates)
  │   ├── index.ejs
  │   ├── login.ejs
  │   ├── dashboard.ejs
  │   └── document.ejs
  └── app.js               (main application file)
Step 4: Create the main application file
Copy the code I provided earlier into your app.js file.
Step 5: Create the view templates
Let's create the EJS templates one by one:
1. Create views/index.ejs:
index.ejs TemplateCode 
2. Create views/login.ejs:
login.ejs TemplateCode 
3. Create views/dashboard.ejs:
dashboard.ejs TemplateCode 
4. Create views/document.ejs:
document.ejs TemplateCode 
5. Create public/styles.css:
styles.cssCode 
Step 6: Run the application

Start the server:
node app.js

Open your browser and go to http://localhost:3000

Step 7: Testing the vulnerabilities
Now that your application is running, let's test each vulnerability:
1. Missing Function Level Access Control

Open a new browser window in incognito/private mode
Navigate directly to http://localhost:3000/dashboard
Notice that you can access the dashboard without logging in

2. Insecure Direct Object References (IDOR)

Log in as alice (password: alice123)
Navigate to http://localhost:3000/dashboard
You'll see all documents listed
Try accessing Bob's document by going to http://localhost:3000/documents/3
Notice that you can access documents that don't belong to you

3. Improper API Authorization

Log in as a regular user (alice or bob)
Try accessing http://localhost:3000/api/users directly in the browser
Notice that you can access all user data even though you're not an admin

4. Parameter Manipulation

Log in as any user
On the dashboard, use the "Parameter Manipulation Test" form
Change the dropdown to a different user ID and submit
Notice that you can update profiles for other users

5. Horizontal Privilege Escalation

Log in as a regular user (alice or bob)
On the dashboard, use the "Privilege Escalation Test" form
Set your own account or another account to have admin role
Submit the form and see that you can escalate privileges

Step 8: Understanding the Secure Implementations
To see how these vulnerabilities should be fixed, check out:

Secure Document Access:

Navigate to http://localhost:3000/secure/documents/3 while logged in as alice
You'll get an "Access denied" message


Secure API with Authorization:

Try accessing http://localhost:3000/secure/api/users while logged in as a regular user
You'll see an "Access denied" message



Understanding the Code
The key security issues in the app are:

Missing Authentication:
javascript// VULNERABLE: No authentication check
app.get('/dashboard', (req, res) => {
  res.render('dashboard', { user: req.session.user, documents: privateDocuments });
});

// SECURE: Proper authentication check
app.get('/secure/dashboard', (req, res) => {
  if (!req.session.user) {
    return res.redirect('/login');
  }
  res.render('dashboard', { user: req.session.user, documents: privateDocuments });
});

Insecure Direct Object References:
javascript// VULNERABLE: No ownership check
app.get('/documents/:id', (req, res) => {
  const documentId = parseInt(req.params.id);
  const document = privateDocuments.find(doc => doc.id === documentId);
  
  if (document) {
    res.render('document', { document });
  } else {
    res.status(404).send('Document not found');
  }
});

// SECURE: Proper ownership verification
app.get('/secure/documents/:id', (req, res) => {
  if (!req.session.user) {
    return res.redirect('/login');
  }
  
  const documentId = parseInt(req.params.id);
  const document = privateDocuments.find(doc => doc.id === documentId);
  
  if (document && (document.userId === req.session.user.id || req.session.user.role === 'admin')) {
    res.render('document', { document });
  } else if (!document) {
    res.status(404).send('Document not found');
  } else {
    res.status(403).send('Access denied: You do not have permission to view this document');
  }
});


This step-by-step guide should help you understand how broken access control vulnerabilities work and how to fix them. If you have any questions about any specific part or need further explanation on any of the vulnerabilities, please let me know!RetryPNi want to make on this 
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Login</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div class="container">
    <h2>Login Page</h2>
    <input type="text" id="username" placeholder="Enter Username" />
    <input type="password" id="password" placeholder="Enter Password" />
    <button onclick="login()">Login</button>
  </div>  <script>
    function login() {
      const user = document.getElementById("username").value;
      const pass = document.getElementById("password").value;
      if (user === "admin" && pass === "1234") {
        localStorage.setItem("loggedIn", "true");
        window.location.href = "dashboard.html";
      } else {
        alert("Invalid credentials");
      }
    }
  </script>
</body>
</html><!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Dashboard</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div class="container">
    <h2>Notes Dashboard</h2>
    <input type="text" id="noteInput" placeholder="Enter your note" />
    <button onclick="addNote()">Add Note</button>    <h3>Your Notes</h3>
    <ul id="noteList"></ul>    <button onclick="logout()">Logout</button>
    <a href="secret.html">Go to Secret Page 🚨</a>
  </div>  <script>
    if (localStorage.getItem("loggedIn") !== "true") {
      alert("Please login first!");
      window.location.href = "index.html";
    }    function loadNotes() {
      const notes = JSON.parse(localStorage.getItem("notes") || "[]");
      const list = document.getElementById("noteList");
      list.innerHTML = "";      notes.forEach((note, index) => {
        const li = document.createElement("li");        li.innerHTML = 
          <input type="text" value="${note}" onchange="updateNote(${index}, this.value)" />
          <button onclick="deleteNote(${index})">Delete</button>
        ;        list.appendChild(li);
      });
    }    function addNote() {
      const note = document.getElementById("noteInput").value;
      if (!note) return alert("Note can't be empty!");
      let notes = JSON.parse(localStorage.getItem("notes") || "[]");
      notes.push(note);
      localStorage.setItem("notes", JSON.stringify(notes));
      document.getElementById("noteInput").value = "";
      loadNotes();
    }    function deleteNote(index) {
      let notes = JSON.parse(localStorage.getItem("notes") || "[]");
      notes.splice(index, 1);
      localStorage.setItem("notes", JSON.stringify(notes));
      loadNotes();
    }    function updateNote(index, newText) {
      let notes = JSON.parse(localStorage.getItem("notes") || "[]");
      notes[index] = newText;
      localStorage.setItem("notes", JSON.stringify(notes));
    }    function logout() {
      localStorage.removeItem("loggedIn");
      window.location.href = "index.html";
    }    loadNotes();
  </script>
</body>
</html><!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Secret Page</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div class="container">
    <h2>🚨 Secret Notes Page (No Access Control)</h2>
    <p>These notes are visible to anyone who visits this page — even without login!</p>    <h3>Exposed Notes:</h3>
    <ul id="secretNotes"></ul>    <button onclick="goBack()">Back to Dashboard</button>
  </div>  <script>
    function goBack() {
      window.location.href = "dashboard.html";
    }    function showSecretNotes() {
      const notes = JSON.parse(localStorage.getItem("notes") || "[]");
      const list = document.getElementById("secretNotes");
      list.innerHTML = "";
      notes.forEach(note => {
        const li = document.createElement("li");
        li.textContent = note;
        list.appendChild(li);
      });
    }    window.onload = showSecretNotes;
  </script>
</body>
</html>EditI'd be happy to help you modify this project to better demonstrate broken access control vulnerabilities. Let me analyze your code first:
You already have a foundation with:

A login page (index.html)
A dashboard that's supposed to be protected (dashboard.html)
A "secret" page with no access control (secret.html)

Your current implementation demonstrates a basic form of broken access control, but we can enhance it to show more types of vulnerabilities while keeping the same structure.
Let's improve these files to better demonstrate broken access control issues:
Enhanced Login Page (index.html)Code 
Enhanced Dashboard Page (dashboard.html)Code 
Enhanced Secret Page (secret.html)Code 
Admin Panel Page (admin.html)Code 
Individual Note Page (note.html)Code 
Enhanced CSS Stylesheet (style.css)Code 
Step-by-Step Instructions to Implement This Project
Step 1: Create the Project Files
Create a new folder for your project and add these files:

index.html - The login page
dashboard.html - The main notes dashboard
secret.html - Secret page demonstrating missing access control
admin.html - Admin panel with insufficient authorization
note.html - Individual note page showing IDOR vulnerability
style.css - Stylesheet for all pages

Step 2: Use the Code Above
Copy the code from each artifact into the corresponding file.
Step 3: Test the Application

Open index.html in your browser
Try logging in with the provided test accounts (admin/1234 or user/user123)

Step 4: Explore the Vulnerabilities
Vulnerability 1: Missing Function Level Access Control

Try accessing dashboard.html directly without logging in
Notice that while there's a check, it can be bypassed by manipulating localStorage

Vulnerability 2: Improper Authorization

After logging in as a regular user, try accessing admin.html directly
Notice that the page is accessible even though you're not an admin

Vulnerability 3: Insecure Direct Object References (IDOR)

Log in as "user"
Visit a note detail page (note.html?id=0&owner=user)
Change the URL to access admin's notes (note.html?id=0&owner=admin)

Vulnerability 4: Parameter Manipulation

Log in as "user"
Visit the dashboard
Notice the URL parameter option to view other users' notes (dashboard.html?userId=admin)

Vulnerability 5: Access to Sensitive Data

Notice that secret.html is accessible without any authentication at all
It displays all users' notes regardless of login status
