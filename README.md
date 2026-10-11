# CareHub Security Lab

CareHub Security Lab is a realistic healthcare portal used to demonstrate how common web vulnerabilities work in real applications and how they are fixed in a hardened version.

This project contains a single app with a mode switch:

- Vulnerable mode: intentionally insecure implementation where the core attacks work end-to-end.
- Safe mode: the same app hardened with proper validation, escaping, token checks, and prepared statements.

## Use case

The application models a small healthcare operations portal for a clinic. It includes:

- clinician login
- patient roster
- clinical note search
- note storage
- staff email change action
- SQL attack lab endpoints for demonstration

## Tech stack

- Node.js 22+
- Express
- SQLite via `better-sqlite3`
- DOMPurify + jsdom for sanitization

## Project structure

- `src/server.js` – Express application and vulnerability toggle
- `src/db.js` – SQLite connection
- `src/init-db.js` – schema initialization
- `src/seed.js` – optional demo accounts and seed data
- `public/` – frontend UI and client-side demos
- `db/schema.sql` – relational database schema

## How to run

From the project root:

```powershell
npm install
npm run db:init
```

### Safe mode

```powershell
$env:SECURITY_MODE = "safe"
$env:PORT = "3000"
npm start
```

Open:

- http://127.0.0.1:3000/
- http://127.0.0.1:3000/dom-demo?id=%3Cscript%3Ealert('DOM-based')%3C%2Fscript%3E

### Vulnerable mode

```powershell
$env:SECURITY_MODE = "vulnerable"
$env:PORT = "3001"
npm start
```

Open:

- http://127.0.0.1:3001/
- http://127.0.0.1:3001/dom-demo?id=%3Cscript%3Ealert('DOM-based')%3C%2Fscript%3E

## Features and vulnerability demonstrations

### 1. XSS

- Reflected XSS: `/api/search?q=<script>alert('XSS')</script>`
- Stored XSS: create a note containing script code and view it back in the portal
- DOM-based XSS: `/dom-demo?id=<script>alert('DOM-based')</script>`

Safe mode sanitizes untrusted input using DOMPurify before rendering or storing it.

### 2. CSRF

- Vulnerable mode: the `/api/change-email` endpoint accepts a form submission without validating a CSRF token.
- Safe mode: a token is generated and validated for the email update form.

### 3. SQL injection

There are three levels of demonstration:

1. Easy: bypass login with payloads like `' OR '1'='1`
2. Intermediate: boolean-style search manipulation via patient name filters
3. Advanced: table and column enumeration using SQLite metadata queries

Safe mode uses parameterized queries and input validation to prevent injection.

## Database notes

This project uses SQLite, with fictional training data only. The schema is intentionally small and safe for local security demos.

## GitHub submission checklist

1. Make sure the repository is initialized locally.
2. Add the remote repository:

```powershell
git remote add origin <your-github-repo-url>
git push -u origin master
```

3. Include the project README, source files, and generated configuration files in the repository.
4. Share the GitHub URL in the submission along with evidence of the vulnerable and safe modes.

## Security notes

This project is specifically designed for learning and local testing. Do not expose the vulnerable mode on a public network and never run it outside a local isolated environment.
