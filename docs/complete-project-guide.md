# The Complete Story — LinkedIn Autopilot Agent

> Everything about this project explained clearly from start to finish.
> Written so that anyone — technical or not — can understand exactly what was built, why every piece exists, and how it all works together.

---

## The Problem We Solved

Satya Sai Pasupuleti is a Senior Technical Writer at Redwood Software — an AI-first SaaS company. Satya wanted to post consistently on LinkedIn every day to build a personal brand as someone who understands AI and uses it in documentation work.

The problem: posting consistently takes time, energy, and discipline. Most professionals have the knowledge but not the consistency.

**The solution:** Build a digital assistant that does it automatically — every single morning — without Satya having to think about it.

---

## What the Agent Does — In Plain English

Imagine you hired a personal assistant. Every morning at 6 AM, this assistant:

1. Reads all the AI tech news from yesterday
2. Picks the most interesting, viral, or important story
3. Reads the full article to understand it deeply
4. Writes a LinkedIn post in your exact voice and style
5. Sends you a WhatsApp message saying "here's what I'm about to post — read this"
6. Waits 2.5 hours while you drink your morning chai
7. Posts it on your LinkedIn at 8:30 AM
8. Writes it all down in a diary for your records

That assistant is exactly what we built. Except instead of a human — it's a bunch of software tools talking to each other, fully automatically, every single day.

---

## The Cast of Characters

Think of it like a kitchen. Every person has one specific job:

| Tool | Role | Real-World Analogy |
|---|---|---|
| **n8n** | The Head Chef | Coordinates everyone. Decides who does what and when. The brain of the operation. |
| **Render.com** | The Office Building | The physical computer that runs 24/7. Never sleeps, never travels. |
| **Supabase** | The Filing Cabinet | Permanently stores n8n's memory — workflows, settings, credentials. |
| **UptimeRobot** | The Security Guard | Knocks on the office door every 5 minutes so it never goes to sleep. |
| **Claude API** | The Staff Writer | The creative brain that reads the news and writes the post in Satya's voice. |
| **NewsAPI** | The Newspaper Boy | Delivers fresh AI news every morning from hundreds of sources. |
| **LinkedIn OAuth** | The Printing Press | Officially posts to LinkedIn on Satya's behalf — safely and legally. |
| **Twilio WhatsApp** | The Messenger | Sends Satya a WhatsApp preview before anything goes live. |
| **Google Sheets** | The Diary | Records every post — date, headline, content, source — permanently. |
| **GitHub** | The Document Library | Stores every file in this project with full version history. |
| **Notion** | The Private Safe | Stores all passwords and API keys privately and securely. |

---

## Why Each Tool Was Chosen

### n8n — Why Not Make.com or Zapier?

Make.com costs $10/month. Zapier costs $20/month. n8n is free when you host it yourself.

All three do the same job — connecting tools and automating workflows. n8n just doesn't charge a monthly fee for the software itself. The only cost is the server it runs on, which is also free (Render.com free tier).

### Render.com — Why Not Your Own Laptop?

n8n needs a computer to run on. Your laptop could work, but:
- Your laptop sleeps when you close the lid
- Your laptop might not be on at 6 AM every day
- If the power goes out, the automation fails
- If you're travelling, nothing runs

Render.com is a server in the cloud that runs 24 hours a day, 7 days a week, 365 days a year. Free tier. No interruptions.

### Supabase — Why Is a Database Needed?

Render.com's free servers have one quirk: **they forget everything when they restart.** This is called "ephemeral storage."

Every time the server restarts (which happens occasionally), all locally stored data disappears. Without a database, n8n would lose all its workflows, credentials, and settings every time.

Supabase is an external PostgreSQL database. It lives separately from Render. Even when the server restarts, n8n connects to Supabase and gets everything back in 30 seconds. Nothing is ever lost.

**Database = permanent memory.**

### UptimeRobot — Why Is a "Pinger" Needed?

Render.com's free tier has an energy-saving feature: if your server receives no traffic for 15 minutes, it goes to sleep.

If the server is asleep at 6 AM, the automation never triggers. No news gets fetched. No post gets written. Nothing happens.

UptimeRobot sends a tiny automated "are you awake?" ping to the server every 5 minutes — keeping it permanently awake. It's completely free for up to 50 monitors.

### Claude API — Why Claude Specifically?

Claude (by Anthropic) was chosen because:
- It follows complex voice instructions more precisely than other models
- It maintains consistent tone across hundreds of posts
- It understands nuanced writing style rules (no exclamation marks, rule of threes, etc.)
- The cost is minimal — about $0.01–0.03 per post, roughly $1–5/month total

### NewsAPI — Why Not Scrape Websites Directly?

Scraping individual news websites is unreliable, slow, and often against terms of service. NewsAPI aggregates hundreds of news sources into one single API call. One request returns ranked, filtered, recent articles from TechCrunch, Hacker News, Wired, VentureBeat, and more — instantly.

### LinkedIn OAuth — Why Not Browser Automation?

Some tools log into LinkedIn by controlling a browser — clicking buttons like a human would. LinkedIn actively detects and bans this behavior.

OAuth is LinkedIn's official, approved method for applications to post on a user's behalf. It's completely within LinkedIn's terms of service and carries zero risk of account penalty. We created a LinkedIn Developer App specifically for this project.

---

## The Full Daily Flow — One Morning in Detail

Here is exactly what happens between 6:00 AM and 8:31 AM every day:

```
All night (every 5 minutes):
UptimeRobot pings https://n8n-9a0r.onrender.com
Server stays awake. n8n stays running.
Supabase holds all workflow data safely.


6:00 AM — The Alarm Goes Off
n8n's built-in scheduler fires.
"It is 6:00 AM. Execute the LinkedIn workflow."


6:01 AM — Fetch the News
n8n calls NewsAPI with this query:
"artificial intelligence + technical writing + automation"
From: yesterday. Sorted by: popularity.

NewsAPI returns top articles from hundreds of sources.
n8n picks the #1 most popular story.
Extracts: headline, summary, source URL.


6:02 AM — Read the Full Article
n8n visits the actual article URL.
Downloads the full webpage as raw text.
Strips all the HTML tags, ads, navigation menus.
Keeps only the clean article content — first 3,000 characters.


6:03 AM — Write the Post
n8n sends everything to Claude API:
- Satya's complete voice profile (writing samples, rules, pattern)
- The full article content
- Today's format type (SHORT / MEDIUM / LONG — rotates daily)
- Writing instructions

Claude reads all of it and writes one LinkedIn post.
It sounds exactly like Satya wrote it.


6:04 AM — Send WhatsApp Preview
n8n calls Twilio's API.
Sends Satya's WhatsApp number the full post text.
Message: the complete post, up to 1,550 characters.

Satya can read the full post on WhatsApp.
If it looks wrong, Satya can log into n8n and stop the workflow.
Otherwise — nothing to do. Just wait.


6:05 AM to 8:30 AM — The Wait
n8n enters a 9,000 second pause (2.5 hours).
No activity. Server stays running. Supabase holds everything.
This is the review window.


8:30 AM — Post Goes Live
n8n wakes from the wait.
Calls LinkedIn API using the OAuth token.
Sends the post text with visibility set to PUBLIC.
Post appears on Satya's LinkedIn profile immediately.
Visible to all connections and followers worldwide.


8:31 AM — Write It Down
n8n calls Google Sheets API.
Appends a new row to "LinkedIn Post Log":

Date       | Headline          | Post Text | Source URL | Status
27/05/2026 | What's the AI...  | [post]    | [url]      | Posted

Workflow complete. n8n goes back to sleep until tomorrow 6 AM.
```

---

## How the Voice Works

This is the most important part of the entire system.

The Claude API prompt contains Satya's complete voice profile — derived from analyzing real LinkedIn posts, including the highest performing post (13,000+ impressions, 200+ likes).

### What Was Discovered From Analyzing Satya's Posts

**Writing style:**
- Short sentences. Maximum 12 words per sentence.
- Arguments built in sets of three lines (rule of threes)
- Never starts a post with "I"
- One concrete metaphor per post
- Last line is always a standalone slow-burn truth — never a call to action

**What makes posts viral:**
- Pick a trending fear or industry assumption
- Flip it with an unexpected angle
- Build the argument in three sharp lines
- End with a quiet line people screenshot and share

**The #1 post pattern (locked from the 13K impression post):**
```
Step 1 — Opening provocation (trending fear or bold claim)
Step 2 — Reframe (flip the assumption)
Step 3 — Three sharp lines (build the argument)
Step 4 — Quiet closing punch (standalone truth)
Step 5 — 4-6 hashtags (mix broad + niche)
```

**Satya's positioning:**
Not just a technical writer posting about writing. The person who makes people realize what they're losing when they ignore clarity, documentation, and human understanding in tech.

Every post connects AI news to what it means for technical writers, SaaS documentation professionals, and people who want to future-proof their careers.

---

## The Content Strategy

### Two Types of Posts

**Type A — AI News Reframe (5 days/week)**
Triggered by a real AI news story from the previous day.
The post reframes the story through the lens of a technical writer who already uses AI daily.
Tone: field notes from someone already living in the future.

**Type B — Personal Work Observation (2 days/week)**
No news story needed.
A real observation from daily work at Redwood Software.
Something AI did well, something it failed at, a realization mid-task.
Tone: honest colleague sharing something useful over coffee.

### Format Rotation

| Day | Format |
|---|---|
| Monday | SHORT (3–5 lines) |
| Tuesday | MEDIUM (150–300 words) |
| Wednesday | LONG (300–500 words) |
| Thursday | SHORT |
| Friday | MEDIUM |
| Saturday/Sunday | Type B observation |

### Content Rules — What the Agent Never Does
- Never says AI replaces technical writers
- Never says technical writers don't need AI
- No politics, religion, or controversial opinions
- No exclamation marks — ever
- No hype words: game-changer, revolutionary, disruptive
- Never sounds like a TED talk

---

## The Infrastructure — How It Was Built

This section documents every step of the actual build in order.

### Phase 1 — Foundation (One Time Setup)

**1. Supabase**
- Created project: Linkedin-autopilot-agent
- Region: Northeast Asia (Tokyo)
- Connection type: Session Pooler
- Host: aws-1-ap-northeast-1.pooler.supabase.com
- Key learning: Must use pooler URL, not direct URL. Must include project ID in username format: postgres.PROJECTID

**2. Render.com**
- Deployed n8n using Docker image: docker.io/n8nio/n8n
- Instance type: Free (512MB RAM, 0.1 CPU)
- Region: Singapore
- 17 environment variables configured (see setup/render-env-variables.md)
- Key learning: N8N_HOST must be the domain only (not https://). N8N_EDITOR_BASE_URL needed for OAuth callbacks. N8N_RUNNERS_ENABLED=false needed because Python is not available on free tier.

**3. UptimeRobot**
- Monitor: HTTP
- URL: https://n8n-9a0r.onrender.com
- Interval: 5 minutes
- Prevents Render free tier from sleeping

**4. n8n Owner Account**
- Email: satyasai7090@gmail.com
- Licensed with free n8n license key
- Instance URL: https://n8n-9a0r.onrender.com

### Phase 2 — API Keys

**Anthropic (Claude API)**
- Account created at console.anthropic.com
- $5 credits purchased
- API key generated for this project

**NewsAPI**
- Account created at newsapi.org
- Free tier: 100 requests/day (we use 1/day)

**Twilio (WhatsApp)**
- Account created at twilio.com
- Trial plan ($15.50 credit)
- WhatsApp sandbox connected
- Sandbox number: +14155238886
- Join code: join television-possible
- Satya's number verified: +919381684789

### Phase 3 — OAuth Connections

**LinkedIn Developer App**
- App name: Autopilot Agent
- Client ID: 86w17258vvpvyt
- Products added: Share on LinkedIn + Sign In with LinkedIn using OpenID Connect
- Scopes: openid, profile, email, w_member_social
- OAuth settings: Organization Support OFF, Legacy OFF
- Key learning: Both scopes products must be added. Organization Support must be OFF for personal profile posting.

**Google OAuth**
- Project created: LinkedIn Autopilot Agent
- APIs enabled: Google Sheets API + Google Drive API
- OAuth consent screen: External
- Test user added: satyasai7090@gmail.com
- Redirect URI: https://n8n-9a0r.onrender.com/rest/oauth2-credential/callback

### Phase 4 — The 11 Workflow Nodes

Built inside n8n one by one. Full code for every node is in n8n/workflow-overview.md.

| # | Node Name | What It Does |
|---|---|---|
| 1 | Schedule Trigger | Fires at 6 AM IST every day |
| 2 | Fetch AI News | Calls NewsAPI, gets top AI articles |
| 3 | Extract Top Story | Picks best article, builds rich context |
| 4 | Fetch Full Article | Scrapes full article text from URL |
| 5 | Clean HTML | Strips HTML tags, extracts clean text |
| 6 | Write Post with Claude | Sends context to Claude, gets post back |
| 7 | Extract Post Text | Pulls clean post from Claude response |
| 8 | Send WhatsApp Preview | Sends full post to Satya's WhatsApp |
| 9 | Wait 2.5 Hours | 9,000 second pause for review window |
| 10 | Post to LinkedIn | Posts to personal profile via OAuth |
| 11 | Log to Google Sheets | Records post in permanent log |

---

## The Account Safety Rules

These protect Satya's LinkedIn account permanently:

- Always use official LinkedIn OAuth — never browser automation
- Post maximum once per day
- Vary format every day (rotation handles this automatically)
- Never post identical content (Claude generates fresh content daily)
- Respond to comments manually — never automate replies
- Refresh OAuth token every 60 days (n8n will show an error when it expires)
- If LinkedIn ever flags the account: pause the workflow immediately for 7 days

---

## The Costs

| Tool | Monthly Cost |
|---|---|
| n8n (self-hosted) | $0 |
| Render.com | $0 |
| Supabase | $0 |
| UptimeRobot | $0 |
| NewsAPI | $0 |
| Gmail | $0 |
| Google Sheets | $0 |
| LinkedIn OAuth | $0 |
| Twilio (after trial) | ~$1–2 |
| Claude API | ~$1–5 |
| **Total** | **~$2–7/month** |

---

## Maintenance — What Satya Actually Needs to Do

| Frequency | Time | Task |
|---|---|---|
| Daily | 0 minutes | Nothing. Check WhatsApp preview at 6 AM if you want. |
| Weekly | 5 minutes | Reply to LinkedIn comments manually. |
| Monthly | 15 minutes | Review which posts performed well. Update voice samples if needed. |
| Every 60 days | 5 minutes | Refresh LinkedIn OAuth token in n8n Credentials. |
| Every 60 days | 5 minutes | Refresh Google OAuth token in n8n Credentials. |
| Quarterly | 30 minutes | Update Claude prompt if positioning shifts. |

---

## The Mental Model — Remember This Always

This entire system is just one idea repeated:

> **Trigger → Fetch → Process → Notify → Wait → Publish → Log**

Every automation agent ever built — whether it posts on LinkedIn, sends sales emails, monitors stock prices, or files expense reports — follows this same skeleton.

The tools change. The logic never does.

Once you understand why each piece exists in this system, you can build any automation agent for any purpose.

---

## What Makes This System Durable

**Separation of concerns** — every tool does exactly one job. If one fails, others continue.

**Fault tolerance:**
- NewsAPI down → n8n catches the error. No silent failures.
- LinkedIn OAuth expired → only the posting step fails. You get an error notification.
- Render.com restarts → Supabase retains all data. n8n reconnects in 30 seconds.

**Version control** — every change to the prompt, workflow, or documentation is committed to GitHub with a description. You can roll back to any previous state instantly.

**Zero single point of failure** — no single tool going down breaks the system permanently. Every failure is recoverable in minutes.

---

## Project Links

| Resource | URL |
|---|---|
| GitHub Repository | github.com/satyasai7090/linkedin-autopilot-agent |
| n8n Dashboard | https://n8n-9a0r.onrender.com |
| Render.com Dashboard | dashboard.render.com |
| Supabase Dashboard | supabase.com/dashboard |
| UptimeRobot Dashboard | dashboard.uptimerobot.com |
| LinkedIn Post Log | docs.google.com/spreadsheets/d/1KBiCAs3X9PR57pZm9EO7L7XlkalPwb_sj5F0HxdVZZc |
| Notion Private Config | notion.so/36d14b3f551d81f8b1b2d8074aedfee4 |

---

## Glossary

**API** — A way for two software tools to talk to each other. Like a waiter who takes your order to the kitchen and brings food back.

**OAuth** — A security standard that lets you give one app permission to act on your behalf in another app — without sharing your password.

**Token** — A temporary password that expires after a set time. LinkedIn tokens last 60 days.

**Docker Image** — A pre-packaged version of software ready to run anywhere. We used n8n's official Docker image.

**Environment Variable** — A secure locker on the server where passwords and API keys are stored privately. The code references them by name without ever seeing the actual value.

**Self-hosted** — Running software on your own server instead of the company's cloud service. Same software. Zero monthly subscription cost.

**Ephemeral Storage** — Temporary storage that disappears when a server restarts. Why Supabase is essential.

**Webhook** — A way for one tool to notify another instantly when something happens.

**Scheduler** — A trigger that fires a workflow at a specific time on a recurring basis. Our scheduler fires at 6:00 AM IST every day.

**Session Pooler** — A Supabase connection method that handles multiple connections efficiently. Required for Render.com free tier compatibility.

---

*Document version: 1.0*
*Created: May 27, 2026*
*Author: Satya Sai Pasupuleti*
*Repository: github.com/satyasai7090/linkedin-autopilot-agent*
