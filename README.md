# CareHub Security Lab — Web Security Blue Team Project

## Use Case
Healthcare operations dashboard (CareHub Portal) with patient records, clinical notes, and admin functions.

## Tech Stack
- **Backend:** Node.js 22+, Express 5, better-sqlite3
- **Frontend:** Vanilla HTML/CSS/JS
- **Security:** DOMPurify (safe mode XSS), scrypt (password hashing), CSRF token set
- **Database:** SQLite (file-based, in `data/app.sqlite`)

## Two Versions
Controlled by `SECURITY_MODE` environment variable:

| Mode | Description |
|------|-------------|
| `vulnerable` | All XSS, CSRF, and SQLi attacks work end-to-end |
| `safe` | All attacks fail; proper defenses enabled |

## How to Run

```bash
# Install dependencies
npm install

# Initialize database
npm run db:init

# Seed demo data
npm run db:seed

# Run vulnerable version (localhost only)
SECURITY_MODE=vulnerable npm start

# Run safe version
SECURITY_MODE=safe npm start