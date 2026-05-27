# n8n Setup Guide

> Complete step-by-step guide to deploy n8n on Render.com with Supabase as the database.

---

## Prerequisites

- GitHub account (satyasai7090)
- Render.com account (linked to GitHub)
- Supabase account (linked to GitHub)

---

## Step 1 — Supabase Database

1. Go to supabase.com → Create new project
2. Name: `Linkedin-autopilot-agent`
3. Region: Northeast Asia (Tokyo) — ap-northeast-1
4. Save the DB password securely

**Connection details needed:**
- Host (Session Pooler): `aws-1-ap-northeast-1.pooler.supabase.com`
- Port: `5432`
- Database: `postgres`
- User: `postgres.YOUR_PROJECT_ID`
- Password: your DB password

---

## Step 2 — Render.com Deployment

1. Go to Render.com → New Web Service
2. Choose: Existing Image
3. Image URL: `docker.io/n8nio/n8n`
4. Name: `n8n`
5. Region: Singapore
6. Instance Type: Free

---

## Step 3 — Environment Variables

Add ALL of these in Render.com → Environment:

```
N8N_BASIC_AUTH_ACTIVE     = true
N8N_BASIC_AUTH_USER       = satya
N8N_BASIC_AUTH_PASSWORD   = [your password]
DB_TYPE                   = postgresdb
DB_POSTGRESDB_HOST        = aws-1-ap-northeast-1.pooler.supabase.com
DB_POSTGRESDB_PORT        = 5432
DB_POSTGRESDB_DATABASE    = postgres
DB_POSTGRESDB_USER        = postgres.[YOUR_PROJECT_ID]
DB_POSTGRESDB_PASSWORD    = [your Supabase DB password]
DB_POSTGRESDB_SSL         = true
N8N_HOST                  = n8n-9a0r.onrender.com
N8N_PORT                  = 5678
N8N_PROTOCOL              = https
WEBHOOK_URL               = https://n8n-9a0r.onrender.com
N8N_EDITOR_BASE_URL       = https://n8n-9a0r.onrender.com
N8N_RUNNERS_ENABLED       = false
N8N_ENCRYPTION_KEY        = [32 character random string]
```

---

## Step 4 — UptimeRobot

1. Go to uptimerobot.com
2. Add new monitor: HTTP
3. URL: `https://n8n-9a0r.onrender.com`
4. Interval: 5 minutes
5. Purpose: Keeps Render free tier from sleeping

---

## Step 5 — n8n Owner Account

On first visit to `https://n8n-9a0r.onrender.com`:
- Email: satyasai7090@gmail.com
- First Name: Satya Sai
- Last Name: Pasupuleti
- Create a strong password

---

## Troubleshooting

| Issue | Fix |
|---|---|
| DB connection error ENOTFOUND | Check DB_POSTGRESDB_USER includes project ID: postgres.XXXXX |
| DB connection error ENOIDENTIFIER | Same as above — tenant ID missing |
| Python runner error | Add N8N_RUNNERS_ENABLED=false |
| OAuth redirect mismatch | Add both n8n.onrender.com and n8n-9a0r.onrender.com to LinkedIn app |
| n8n shows wrong OAuth URL | Add N8N_EDITOR_BASE_URL and N8N_PROTOCOL variables |
