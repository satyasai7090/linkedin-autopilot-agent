# LinkedIn Autopilot Agent 🤖

> A fully automated LinkedIn posting agent built on n8n + Claude API — posting daily in my voice, driven by real AI news and personal observations.

**Status: LIVE ✅ — Active since May 27, 2026**

---

## What This Does

Every morning at 6 AM IST, this agent automatically:

1. Scans global AI news from the previous 24 hours (NewsAPI + full article scraping)
2. Picks the most trending, impactful story
3. Writes a LinkedIn post in my voice using Claude API
4. Sends me a full preview via WhatsApp before posting
5. Posts automatically to my LinkedIn personal profile at 8:30 AM
6. Logs every post to Google Sheets

---

## My Positioning

> A technical writer without AI is good.
> A technical writer with AI is a different category entirely.

I'm a Senior Technical Writer at an AI-first SaaS company. Every post connects AI news to what it means for technical writers, SaaS documentation professionals, and people who want to future-proof their careers.

---

## Tech Stack

| Layer | Tool | Cost |
|---|---|---|
| Automation | n8n (self-hosted on Render.com) | Free |
| Database | Supabase (PostgreSQL) | Free |
| Server uptime | UptimeRobot | Free |
| AI Writer | Claude API (Anthropic) | ~$1-5/month |
| News Source | NewsAPI | Free |
| Publisher | LinkedIn OAuth | Free |
| Preview | Twilio WhatsApp Sandbox | Free (trial) |
| Logger | Google Sheets | Free |

**Total monthly cost: ~$1-5**

---

## Architecture

```
6:00 AM  →  Scheduler triggers
6:01 AM  →  NewsAPI fetches top AI stories
6:02 AM  →  Full article scraped for context
6:03 AM  →  Claude writes post in Satya's voice
6:04 AM  →  WhatsApp preview sent
8:30 AM  →  Post goes live on LinkedIn
8:31 AM  →  Row logged to Google Sheets
```

---

## Project Structure

```
linkedin-autopilot-agent/
├── README.md
├── changelog.md
├── docs/
│   ├── blueprint.md
│   ├── master-prompt.md
│   └── architecture-explained.md
├── n8n/
│   ├── workflow-overview.md
│   └── setup-guide.md
├── prompts/
│   └── master-prompt.txt
├── setup/
│   ├── render-env-variables.md
│   └── credentials-checklist.md
└── voice-profile/
    └── satya-voice.md
```

---

## The 11 Workflow Nodes

| # | Node | Type | Purpose |
|---|---|---|---|
| 1 | Schedule Trigger | Trigger | Fires daily at 6 AM IST |
| 2 | Fetch AI News | HTTP Request | Calls NewsAPI for top stories |
| 3 | Extract Top Story | Code (JS) | Picks most popular article |
| 4 | Fetch Full Article | HTTP Request | Scrapes full article text |
| 5 | Clean HTML | Code (JS) | Extracts clean text for Claude |
| 6 | Write Post | HTTP Request | Calls Claude API |
| 7 | Extract Post Text | Code (JS) | Pulls clean post from response |
| 8 | WhatsApp Preview | HTTP Request | Sends preview via Twilio |
| 9 | Wait | Wait | Pauses 2.5 hours |
| 10 | Post to LinkedIn | LinkedIn | Posts to personal profile |
| 11 | Log to Sheets | Google Sheets | Logs post details |

---

## Post Pattern

Derived from highest performing post (13,000+ impressions):

```
Step 1 — Opening provocation
Step 2 — Reframe the assumption
Step 3 — Three sharp lines
Step 4 — Quiet closing punch
Step 5 — 4-6 hashtags
```

---

## Maintenance

| Frequency | Task |
|---|---|
| Daily | Check WhatsApp preview |
| Weekly | Reply to LinkedIn comments manually |
| Monthly | Review performance, update voice samples |
| Every 60 days | Refresh LinkedIn + Google OAuth tokens |

---

## Author

**Satya Sai Pasupuleti**
Senior Technical Writer | Redwood Software
Hyderabad, India
github.com/satyasai7090
