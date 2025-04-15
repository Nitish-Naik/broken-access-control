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