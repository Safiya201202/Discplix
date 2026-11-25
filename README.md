# Disciplix

Productivity app (tasks, habits, focus timer, calendar, goals, analytics, leaderboard, chat)
with a secure backend on Vercel + Postgres.

## Folder layout
```
api/            serverless backend (auth.js, data.js, social.js, _lib/)
db/schema.sql   database tables
public/         the website (index.html + disciplix-client.js)
vercel.json     security headers + settings
```

## Setup
1. Create a free Postgres database at https://neon.tech and copy the connection string.
2. In Neon's SQL editor, paste and run `db/schema.sql`.
3. In Vercel > Project > Settings > Environment Variables add:
   - `DATABASE_URL`  (your Neon connection string)
   - `JWT_SECRET`    (32+ random characters, e.g. `node -e "console.log(require('crypto').randomBytes(48).toString('base64'))"`)
   - `OWNER_EMAIL`   (your email; you become the chat moderator)
4. Push this folder to GitHub. Vercel redeploys automatically.

Local testing: `npm install`, copy `.env.example` to `.env`, then `npx vercel dev`.

## Security included
- Passwords hashed with bcrypt (cost 12); never stored or logged in plain text
- Login in an HttpOnly, SameSite=Strict, Secure cookie (JWT, 7 days); not readable by JavaScript
- CSRF defence: Origin check + required custom header on every write
- Rate limits on register, login, chat, DMs, search and saves (stored in Postgres)
- Input validation with zod; parameterised SQL everywhere (no injection)
- Leaderboard score is calculated on the server; users can only write their own row
- Private messages can only be deleted by their sender; only the owner can moderate community chat
- Security headers + Content-Security-Policy in `vercel.json`
- Local data is wiped from the device on login/logout
