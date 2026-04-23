# CVAI

AI-powered resume tailoring. Paste your CV and a job description — get back a tailored version with structured feedback on alignment, strengths, and gaps.

Built on Replit. **This project is vibe coded** — scaffolded and iterated with AI assistance throughout.

---

## How It Works

1. Sign in with Google
2. Upload your CV and paste the target job description
3. An n8n workflow handles the AI processing pipeline
4. The app polls for completion, then surfaces:
   - Tailored CV (formatted, ATS-friendly)
   - Structured comments: Overall Impression, Alignment with JD, Strengths, Factual Accuracy, Formatting Notes
   - Match score and keyword analysis

---

## Stack

| Layer | Tech |
|---|---|
| Frontend | React 18, TypeScript, Vite |
| Styling | Tailwind CSS, shadcn/ui (Radix UI) |
| Routing | Wouter |
| Backend | Express.js, Node.js (ESM) |
| Auth | Google OAuth via Passport.js + express-session |
| Database | Neon PostgreSQL via Drizzle ORM |
| AI pipeline | n8n webhook |
| Hosting | Replit |

---

## Architecture

```
Browser (React + Vite)
  → POST /api/process (CV + JD)
    → Express server
      → n8n webhook (AI tailoring pipeline)
        ← webhook response: tailored CV + structured comments
  → Poll /api/status/:id every 2s until complete
  → Display: tailored CV + comment breakdown
```

Auth is session-based (PostgreSQL session store). Google OAuth flow via Passport.

---

## Running Locally

```bash
npm install
npm run db:push   # apply schema
npm run dev
```

Required environment variables:

```
DATABASE_URL=postgresql://...
SESSION_SECRET=<long random string>
GOOGLE_CLIENT_ID=...
GOOGLE_CLIENT_SECRET=...
N8N_WEBHOOK_URL=https://your-n8n-instance/webhook/...
```

---

## Status

v1 — functional. Auth, CV processing, and comment display all working. Deployed on Replit.
