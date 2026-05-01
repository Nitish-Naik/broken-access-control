# Broken Access Control Demo

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Status](https://img.shields.io/badge/status-educational-orange.svg)]()
[![OWASP](https://img.shields.io/badge/OWASP-A01%3A2021-red.svg)](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)

> An intentionally vulnerable web application built to demonstrate **Broken Access Control** — ranked **#1** on the OWASP Top 10 (2021).

This project is designed for security students, web developers, and CTF practitioners who want hands-on experience identifying and exploiting access control flaws in a controlled environment.

---

## Table of Contents

- [Overview](#overview)
- [Project Structure](#project-structure)
- [Demonstrated Vulnerabilities](#demonstrated-vulnerabilities)
- [Test Credentials](#test-credentials)
- [Getting Started](#getting-started)
- [Exploiting the Vulnerabilities](#exploiting-the-vulnerabilities)
- [Secure Implementation Guidance](#secure-implementation-guidance)
- [Disclaimer](#disclaimer)
- [License](#license)

---

## Overview

The application is a minimal note-taking web app written in plain **HTML, CSS, and JavaScript** (no backend). It deliberately implements authentication and authorization on the client side — a common anti-pattern that opens the door to a wide range of access control vulnerabilities.

By interacting with the app, users can observe how attackers bypass authentication, escalate privileges, and access data that should be restricted.

---

## Project Structure

```
broken-access-control-demo/
├── index.html       # Login page (client-side authentication)
├── dashboard.html   # User notes dashboard
├── note.html        # Individual note view (vulnerable to IDOR)
├── secret.html      # Hidden page with no access control
├── admin.html       # Admin panel (improper authorization)
├── style.css        # Shared stylesheet
└── README.md        # Project documentation
```

---

## Demonstrated Vulnerabilities

The application illustrates five distinct categories of broken access control:

### 1. Client-Side Authentication

**File:** `index.html`

Login credentials are validated entirely in JavaScript. Authentication state is stored in `localStorage`, which any user can manually edit through browser developer tools to bypass the login flow.

### 2. Missing Function-Level Access Control

**File:** `dashboard.html`

The dashboard performs only a superficial login check. Because the check runs client-side, it can be circumvented by setting `localStorage.loggedIn = "true"` directly in the browser console.

### 3. Insecure Direct Object References (IDOR)

**File:** `note.html`

Notes are accessed via URL parameters such as `note.html?id=0&owner=admin`. There is no ownership verification, so any authenticated user can read or modify another user's notes simply by editing the URL.

### 4. Improper Authorization

**File:** `admin.html`

The admin panel only checks whether a user is logged in, not whether the user holds the admin role. Any authenticated user — regardless of role — can access privileged functionality.

### 5. Unprotected Sensitive Endpoint

**File:** `secret.html`

This page exposes all users' notes and has no authentication check whatsoever. Anyone with knowledge of the URL can access it directly, even without logging in.

---

## Test Credentials

| Role  | Username | Password  |
| :---- | :------- | :-------- |
| Admin | `admin`  | `1234`    |
| User  | `user`   | `user123` |

---

## Getting Started

### Option 1 — Open Directly in a Browser

The simplest way to run the demo:

```bash
git clone https://github.com/<your-username>/broken-access-control-demo.git
cd broken-access-control-demo
open index.html      # macOS
xdg-open index.html  # Linux
start index.html     # Windows
```

### Option 2 — Serve with a Local Web Server

For a more realistic environment that supports relative URLs cleanly:

```bash
# Using Python 3
python3 -m http.server 8000

# Or using Node.js
npx serve .
```

Then visit [http://localhost:8000](http://localhost:8000) in your browser.

---

## Exploiting the Vulnerabilities

Each scenario below walks through reproducing one of the demonstrated flaws.

### 1. Bypassing Client-Side Authentication

1. Open `index.html`.
2. Open the browser's DevTools console.
3. Run:
   ```js
   localStorage.setItem("loggedIn", "true");
   window.location.href = "dashboard.html";
   ```
4. You are now "logged in" without ever submitting valid credentials.

### 2. Accessing Another User's Notes (IDOR)

1. Log in as `user`.
2. Navigate to a note: `note.html?id=0&owner=user`.
3. Change the URL to `note.html?id=0&owner=admin`.
4. The admin's note is displayed without any ownership check.

### 3. Reaching the Admin Panel as a Standard User

1. Log in as `user` (a non-admin account).
2. Navigate directly to `admin.html`.
3. The admin panel loads — the page only verifies login, not role.

### 4. Reading the Secret Page Unauthenticated

1. Log out, or open a new private/incognito window.
2. Navigate directly to `secret.html`.
3. All notes are exposed despite no login session existing.

---

## Secure Implementation Guidance

To remediate the vulnerabilities demonstrated here, the following principles should be applied in any production system:

- **Move authentication and authorization to the server.** Never rely on client-side checks for security decisions.
- **Validate access on every request.** Verify both *who* the user is (authentication) and *what they may do* (authorization) on every protected endpoint.
- **Enforce ownership checks.** When a resource is accessed by ID, confirm that the requesting user owns the resource or holds an appropriate role.
- **Apply role-based access control (RBAC).** Distinguish clearly between user roles and gate functionality accordingly.
- **Use secure session management.** Issue signed, HTTP-only session cookies; do not store auth state in `localStorage`.
- **Adopt a default-deny posture.** Access should be explicitly granted rather than implicitly allowed.
- **Log and monitor access control failures.** Repeated authorization failures often indicate active probing.

---

## Disclaimer

This project is provided **strictly for educational and research purposes**. The vulnerabilities are intentional and serve to illustrate insecure design patterns. Do **not** deploy this code, or any code modeled on it, to a production environment. The author assumes no liability for misuse.

---

## License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.