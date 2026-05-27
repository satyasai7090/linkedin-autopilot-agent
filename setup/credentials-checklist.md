# Credentials Checklist

> Every service needed to run the LinkedIn Autopilot Agent. All actual credentials are stored in Notion (private).

---

## Services Required

| Service | Purpose | URL | Free? |
|---|---|---|---|
| Render.com | Hosts n8n server | render.com | Yes |
| Supabase | n8n database | supabase.com | Yes |
| UptimeRobot | Keeps server awake | uptimerobot.com | Yes |
| Anthropic | Claude API for writing | console.anthropic.com | Pay per use (~$1-5/mo) |
| NewsAPI | Fetches AI news | newsapi.org | Yes (100 req/day) |
| LinkedIn Developer | OAuth for posting | linkedin.com/developers | Yes |
| Twilio | WhatsApp preview | twilio.com | Free trial |
| Google Cloud | OAuth for Sheets | console.cloud.google.com | Yes |
| Google Sheets | Post logging | sheets.google.com | Yes |

---

## LinkedIn Developer App Setup

1. Create app at linkedin.com/developers
2. Add product: Share on LinkedIn
3. Add product: Sign In with LinkedIn using OpenID Connect
4. OAuth settings:
   - Organization Support: OFF
   - Legacy: OFF
   - Redirect URL: `https://n8n-9a0r.onrender.com/rest/oauth2-credential/callback`
   - Also add: `https://n8n.onrender.com/rest/oauth2-credential/callback`

---

## Google OAuth Setup

1. Create project at console.cloud.google.com
2. Enable: Google Sheets API + Google Drive API
3. OAuth consent screen: External
4. Add test user: satyasai7090@gmail.com
5. Create OAuth Client ID (Web application)
6. Redirect URI: `https://n8n-9a0r.onrender.com/rest/oauth2-credential/callback`

---

## Twilio WhatsApp Sandbox

1. Create account at twilio.com
2. Use trial plan
3. Sandbox number: +14155238886
4. Join code: join television-possible
5. Send "join television-possible" to +14155238886 on WhatsApp to activate

---

## Token Refresh Schedule

| Token | Expires | Action |
|---|---|---|
| LinkedIn OAuth | Every 60 days | Reconnect in n8n Credentials |
| Google OAuth | Every 60 days | Reconnect in n8n Credentials |
| Twilio Sandbox | Stays active | Keep WhatsApp connected |
| Anthropic API | Never | Monitor credit balance |
| NewsAPI | Never | Monitor usage (100/day free) |
