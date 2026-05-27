# SKILL.md — How to Build Any Automation Agent

> A battle-tested playbook for building automation agents with non-technical users.
> Derived from building the LinkedIn Autopilot Agent end-to-end.
> Use this skill whenever someone wants to build any kind of automation agent.

---

## What This Skill Is

This is not a tutorial for one specific agent. It is a **framework for building any agent** — a step-by-step thinking process that Claude follows to take someone from "I have an idea" to "it's live and running."

It covers:
- How to interview the user before touching any tool
- How to design the right architecture
- The exact build order that avoids back-and-forth debugging
- Prerequisites to complete before each phase
- How to set up Notion, GitHub, and documentation from the start
- Common errors and how to avoid them
- How to explain every decision in plain English throughout

**The golden rule:** Never touch a tool until you fully understand what the user wants and why.

---

## Phase 0 — Deep Discovery (Before Anything Else)

This phase is non-negotiable. Do not suggest any tools or architecture until this is complete.

### Step 1 — Understand the Goal

Ask these questions one group at a time. Never dump all questions at once.

**Group 1 — The What and Why**
- What do you want to automate?
- What problem does it solve for you?
- What does success look like — how will you know it's working?

**Group 2 — The How Much Control**
- Do you want full autopilot or do you want to approve before anything happens?
- How often should it run?
- What should it never do?

**Group 3 — The Context**
- Who is the audience or recipient of the output?
- Is there any existing content, data, or accounts this connects to?
- What platforms or tools are you already using?

**Group 4 — Technical Comfort**
- How comfortable are you with technology? (Use plain options: non-technical / some experience / developer)
- Where should this run — your laptop, a cloud server, or a no-code platform?
- What is your monthly budget for tools?

### Step 2 — Extract the Voice or Style (if content is involved)

If the agent produces any kind of content (posts, emails, messages, reports):
- Ask for 5–10 real examples of their best work
- Analyze: sentence length, tone, structure, what makes their top performers work
- Build a voice profile before writing a single prompt

### Step 3 — Lock the Requirements

Before moving to architecture, summarize everything back to the user:
- What the agent does
- What it never does
- How often it runs
- Who/what receives the output
- The user's technical comfort level
- The budget

Get explicit confirmation. Only then proceed.

---

## Phase 1 — Architecture Design

### Step 1 — Map the Flow

Every agent follows this universal skeleton:

```
TRIGGER → FETCH → PROCESS → NOTIFY → WAIT → PUBLISH → LOG
```

Map the user's specific goal onto this skeleton before choosing any tools.

Example mapping for a LinkedIn posting agent:
```
TRIGGER   = Schedule (6 AM daily)
FETCH     = NewsAPI (get today's top story)
PROCESS   = Claude API (write post in user's voice)
NOTIFY    = WhatsApp (send preview to user)
WAIT      = 2.5 hours (review window)
PUBLISH   = LinkedIn OAuth (post publicly)
LOG       = Google Sheets (record everything)
```

Do this mapping exercise for any agent before recommending tools.

### Step 2 — Choose the Stack

**Recommended default stack (proven, tested, mostly free):**

| Layer | Tool | Cost | Why |
|---|---|---|---|
| Automation engine | n8n (self-hosted) | Free | Most powerful, no monthly fee |
| Server | Render.com | Free | Reliable, easy deployment |
| Database | Supabase | Free | Permanent memory for n8n |
| Keep-alive | UptimeRobot | Free | Prevents server from sleeping |
| AI brain | Claude API | ~$1–5/month | Best instruction following |
| Notifications | Twilio (WhatsApp) or Gmail | Free/cheap | Preview before publishing |
| Logging | Google Sheets | Free | Simple, accessible |
| Version control | GitHub | Free | Track every change |
| Credentials | Notion (private page) | Free | Safe credential storage |

**Alternative stacks:**
- If user wants no-code simplicity and has budget: Make.com ($10/month) instead of n8n
- If user wants zero server management: n8n Cloud ($24/month)
- If agent is simple (2–3 steps): Zapier free tier may suffice

### Step 3 — Explain the Architecture in Plain English

Before building anything, explain every tool using simple analogies.

**The standard analogy set (use this every time):**
- n8n = The Head Chef. Coordinates everything. Tells each tool what to do and when.
- Render.com = The Office Building. Always open 24/7. The computer that never sleeps.
- Supabase = The Filing Cabinet. Permanent memory. Never forgets even when the server restarts.
- UptimeRobot = The Security Guard. Knocks on the door every 5 minutes so the server never sleeps.
- Claude API = The Staff Writer. Reads the data and produces the output.
- The source API = The Newspaper Boy. Delivers fresh data every time.
- The publisher = The Printing Press. Sends the output to its destination.
- Google Sheets = The Diary. Records everything permanently.
- GitHub = The Document Library. Tracks every change ever made.
- Notion = The Private Safe. Stores passwords and keys securely.

Ask the user to confirm they understand before proceeding.

---

## Phase 2 — Setup in the Right Order

**Critical:** Always follow this exact order. Deviating causes back-and-forth debugging.

```
1. Notion private page (credentials safe)
2. GitHub repo (version control ready)
3. Supabase database (memory first)
4. Render.com deployment (server second)
5. UptimeRobot monitor (keep-alive third)
6. n8n owner account (dashboard access)
7. All API keys (collect before building)
8. All OAuth connections (connect before building)
9. Build workflow nodes (now and only now)
10. Test (every node before activating)
11. Activate (go live)
```

### Step 1 — Create Notion Private Config Page

Do this FIRST. Every credential goes here as it's created.

Create a Notion page with these sections:
- Project Overview (name, status, live URL)
- All service credentials (one section per service)
- Build progress log
- Maintenance schedule
- Token refresh reminders

**If Claude has Notion access:** Create the page automatically. Never ask the user to create it manually.

Template structure:
```
## [Service Name]
| Key | Value |
|---|---|
| Account Email | |
| Password | |
| API Key | |
| Created | |
| Notes | |
```

Update Notion automatically every time a new credential is created. Never ask the user to remember this.

### Step 2 — Create GitHub Repo

Do this SECOND.

Files to create immediately:
- `README.md` — what the agent does, tech stack, architecture diagram
- `changelog.md` — version history starting at v0.1
- `docs/architecture.md` — plain English explanation
- `docs/complete-guide.md` — full end-to-end story
- `prompts/` — all Claude prompts used
- `setup/` — environment variables and credentials checklist
- `.gitignore` — block any .env files

**If Claude has GitHub access:** Create repo and push files automatically.
**If not:** Generate a ZIP file for the user to download and push with 3 Terminal commands.

Commit message convention:
- `v0.1.0 — Initial project structure`
- `v1.0.0 — System live`
- `v1.0.1 — Fix: [what was fixed]`

### Step 3 — Supabase Setup

1. Create account at supabase.com (sign up with GitHub)
2. Create new organization: `[project-name]`
3. Create new project: `[project-name]`
4. Select region closest to user's location
5. Generate strong DB password → save to Notion immediately
6. Get connection details from: Project Settings → API Keys

**Critical lesson learned:**
- Use Session Pooler connection, NOT direct connection
- Session Pooler host format: `aws-[N]-[region].pooler.supabase.com`
- DB username format: `postgres.[PROJECT_ID]` (must include project ID)
- Port: 5432 for session pooler
- Find exact host from: Project dashboard → Connect button → Direct → Session pooler tab

Save to Notion: Project URL, Publishable Key, Secret Key, DB Password, Pooler Host, DB User with project ID.

### Step 4 — Render.com Deployment

1. Create account at render.com (sign up with GitHub)
2. Create new workspace: `[project-name]`
3. New Web Service → Existing Image → `docker.io/n8nio/n8n`
4. Name: `n8n`
5. Region: Singapore (or closest to user)
6. Instance Type: Free

**Environment variables to add (all required, in this exact form):**

```
N8N_BASIC_AUTH_ACTIVE     = true
N8N_BASIC_AUTH_USER       = [username]
N8N_BASIC_AUTH_PASSWORD   = [strong password]
DB_TYPE                   = postgresdb
DB_POSTGRESDB_HOST        = [supabase pooler host]
DB_POSTGRESDB_PORT        = 5432
DB_POSTGRESDB_DATABASE    = postgres
DB_POSTGRESDB_USER        = postgres.[SUPABASE_PROJECT_ID]
DB_POSTGRESDB_PASSWORD    = [supabase DB password]
DB_POSTGRESDB_SSL         = true
N8N_HOST                  = [render-url].onrender.com
N8N_PORT                  = 5678
N8N_PROTOCOL              = https
WEBHOOK_URL               = https://[render-url].onrender.com
N8N_EDITOR_BASE_URL       = https://[render-url].onrender.com
N8N_RUNNERS_ENABLED       = false
N8N_ENCRYPTION_KEY        = [32 character random string]
```

**Critical lessons learned:**
- `N8N_HOST` = domain only, no https://
- `DB_POSTGRESDB_USER` must include project ID: `postgres.PROJECTID`
- `N8N_EDITOR_BASE_URL` is required for OAuth callbacks to work correctly
- `N8N_RUNNERS_ENABLED=false` required because Python is not available on free tier
- If DB connection fails with ENOTFOUND: check the pooler host region matches your Supabase project region

Save n8n login URL and credentials to Notion.

### Step 5 — UptimeRobot

1. Create account at uptimerobot.com (sign up with GitHub)
2. Add new monitor: HTTP
3. URL: your Render.com n8n URL
4. Interval: 5 minutes
5. Alert email: user's email

Save monitor URL and status page to Notion.

### Step 6 — n8n Owner Account

Visit your n8n URL → complete owner account setup → accept free license key offer.

Save credentials to Notion.

---

## Phase 3 — Collect All API Keys Before Building

**Never start building workflow nodes until ALL required API keys and OAuth connections are ready.**

This prevents stopping mid-build to set up accounts.

For each service:
1. Create account
2. Get API key or credentials
3. Save to Notion immediately
4. Do NOT proceed to next service until current one is saved

Common services and how to get credentials:

**Claude API (Anthropic)**
- console.anthropic.com
- Add $5 minimum credits before generating key
- Generate key → copy immediately (shown only once)

**NewsAPI**
- newsapi.org → sign up → dashboard shows key immediately
- Free tier: 100 requests/day

**Twilio (WhatsApp)**
- twilio.com → Start for free → Trial plan
- Dashboard shows Account SID and Auth Token
- WhatsApp sandbox: Messaging → Try it out → Send a WhatsApp message
- User must send "join [code]" to +14155238886 on WhatsApp to activate sandbox
- Verify user's phone number in Verified Caller IDs before testing

**LinkedIn Developer App**
- linkedin.com/developers → Create app
- Requires a LinkedIn Company Page (create one if needed)
- Add products: Share on LinkedIn + Sign In with LinkedIn using OpenID Connect
- Auth settings: Organization Support OFF, Legacy OFF
- Add redirect URI: `https://[n8n-url]/rest/oauth2-credential/callback`
- Also add: `https://n8n.onrender.com/rest/oauth2-credential/callback` (n8n sometimes uses this)
- Critical: Both products must be added for all required scopes to work

**Google OAuth (for Sheets, Gmail, Drive)**
- console.cloud.google.com → New project
- Enable APIs: Google Sheets API + Google Drive API
- OAuth consent screen: External → Add test user (user's Gmail)
- Create OAuth Client ID → Web application
- Add redirect URI: `https://[n8n-url]/rest/oauth2-credential/callback`

---

## Phase 4 — Build the Workflow

Only start this phase when ALL of these are true:
- [ ] Notion page created and populated
- [ ] GitHub repo created with initial files
- [ ] Supabase connected and n8n running
- [ ] UptimeRobot active
- [ ] All API keys saved to Notion
- [ ] All OAuth apps created

### Building Nodes

Build one node at a time. Test each node before adding the next.

**Node testing rule:** Click "Execute step" after every node. Verify the output looks correct before connecting the next node. Never build 5 nodes then test — you won't know where the error is.

**The universal node order:**
1. Trigger (schedule or webhook)
2. Fetch data (HTTP Request to data source)
3. Process/transform data (Code node)
4. Fetch additional data if needed (HTTP Request)
5. Clean/prepare data (Code node)
6. AI processing (HTTP Request to Claude API)
7. Extract AI output (Code node)
8. Notify user (WhatsApp/Email)
9. Wait (if review window needed)
10. Publish/act (the main output)
11. Log (Google Sheets or database)

### Claude API Node Setup

**Method:** HTTP Request (not n8n's built-in Claude node — HTTP Request is more reliable)

```
Method: POST
URL: https://api.anthropic.com/v1/messages
Headers:
  x-api-key: [your key]
  anthropic-version: 2023-06-01
  content-type: application/json
Body (JSON):
  model: claude-sonnet-4-5
  max_tokens: {{ 1000 }}  ← must be expression, not plain number
  messages: {{ [{"role": "user", "content": [your prompt variable]}] }}
```

**Critical lessons learned:**
- `max_tokens` must be set as expression `{{ 1000 }}` not plain text `1000`
- `messages` must be array expression, not JSON.stringify wrapped
- Model name: use `claude-sonnet-4-5` — verify current model names at console.anthropic.com

### Twilio WhatsApp Node Setup

**Method:** HTTP Request (more reliable than n8n's Twilio node)

```
Method: POST
URL: https://api.twilio.com/2010-04-01/Accounts/[ACCOUNT_SID]/Messages.json
Authentication: Generic Credential Type → Basic Auth
  Username: [Account SID]
  Password: [Auth Token]
Body: Form URLencoded
  From: whatsapp:+14155238886
  To: whatsapp:+[user's number]
  Body: [message expression]
```

**Critical lessons learned:**
- Use HTTP Request, not native Twilio node (auth issues with trial accounts)
- Body content type must be Form URLencoded, not JSON
- WhatsApp message limit: 1,600 characters — truncate if needed
- User's number must be verified in Twilio dashboard first

---

## Phase 5 — Testing

### Test Order

1. **Test each node individually** using Execute Step
2. **Test the chain** — run from Node 1 to Node 7 (before any wait or publish)
3. **Verify outputs** — check WhatsApp, check Google Sheets
4. **Disable the publish node** — run full workflow without actually publishing
5. **Enable publish node** — run one real test post
6. **Activate the workflow** — flip the active toggle

### Never Skip Testing

The most common mistake: activating the workflow without testing the publish step. Always do one real test post before going live.

---

## Phase 6 — Documentation and Go-Live

### Before Activating

1. Rename workflow to something meaningful (not "My workflow")
2. Commit all workflow code to GitHub
3. Update Notion with final system status
4. Update README with live status

### Activating

In n8n: click Publish → save version → toggle Active.

### After Activating

1. Update Notion: mark status as LIVE, add activation date
2. Commit to GitHub: `v1.0.0 — System live`
3. Set calendar reminder for OAuth token refresh (60 days)

---

## Common Errors and Fixes

This section captures every real error encountered building the LinkedIn Autopilot Agent.

| Error | Cause | Fix |
|---|---|---|
| `ENOTFOUND tenant/user postgres.xxx not found` | Wrong Supabase pooler host or missing project ID in username | Use session pooler host. Username must be `postgres.PROJECTID` |
| `ENOIDENTIFIER no tenant identifier provided` | Project ID missing from DB username | Change DB_POSTGRESDB_USER to `postgres.PROJECTID` |
| `Connection terminated unexpectedly` | Wrong pooler host region | Get exact host from Supabase → Connect → Session pooler tab |
| `Exited with status 1 — Python runner` | Python not available on Render free tier | Add `N8N_RUNNERS_ENABLED=false` |
| `max_tokens: Input should be a valid integer` | max_tokens sent as string | Wrap in expression: `{{ 1000 }}` |
| `messages: Input should be a valid array` | Messages formatted incorrectly | Use expression array, not JSON.stringify |
| `model not found` | Wrong model name | Check current models at console.anthropic.com |
| `redirect_uri does not match` | OAuth callback URL mismatch | Add both `n8n.onrender.com` and `n8n-xxx.onrender.com` to LinkedIn app |
| `unauthorized_scope_error: w_organization_social` | Organization Support toggle is ON | Turn OFF Organization Support in LinkedIn credential |
| `unauthorized_scope_error: openid` | OpenID Connect product not added | Add Sign In with LinkedIn using OpenID Connect product |
| `unauthorized_scope_error: r_emailaddress` | Legacy toggle is ON | Turn OFF Legacy in LinkedIn credential |
| `Access blocked: app not verified` | Google OAuth test users not added | Add user's Gmail as test user in Google Cloud Console |
| `No columns found in Google Sheets` | Google Sheets API not enabled | Enable Google Sheets API + Google Drive API in Google Cloud Console |
| `Message body exceeds 1600 character limit` | WhatsApp message too long | Use `.substring(0, 1550)` to truncate |
| n8n OAuth URL shows wrong domain | WEBHOOK_URL not updating OAuth callback | Add `N8N_EDITOR_BASE_URL` environment variable |

---

## The Master Prompt Structure

When Claude API is used to generate content, the prompt must contain:

```
1. WHO the person is (identity, role, company, positioning)
2. HOW they write (voice rules — sentence length, tone, patterns)
3. EXAMPLES of their real writing (3–10 samples)
4. WHAT to write today (the input data/context)
5. WHAT FORMAT to use (length, structure)
6. RULES to never break (guardrails)
7. OUTPUT INSTRUCTIONS (write only the output, no preamble)
```

Never skip the examples. The examples are what make the output sound human.

---

## Maintenance Reminders to Always Set

For every agent built, always tell the user:

| Task | When |
|---|---|
| Refresh LinkedIn OAuth | Every 60 days |
| Refresh Google OAuth | Every 60 days |
| Check API credit balance | Monthly |
| Review output quality | Weekly for first month |
| Update voice/prompt if needed | When output feels off |

---

## The Universal Mental Model

Every automation agent ever built follows the same skeleton:

```
TRIGGER → FETCH → PROCESS → NOTIFY → WAIT → PUBLISH → LOG
```

The tools change. The logic never does.

When starting any new agent project, the first thing to do is map the user's goal onto this skeleton. Once the skeleton is clear, choosing tools and building nodes becomes straightforward.

---

## How to Use This Skill

**When a user says they want to build an automation agent:**

1. Read this entire SKILL.md first
2. Start with Phase 0 — ask discovery questions before suggesting anything
3. Map their goal to the universal skeleton
4. Follow the setup order exactly (Notion → GitHub → Supabase → Render → UptimeRobot → API keys → Build)
5. Explain every tool using the analogy set before touching it
6. Update Notion automatically throughout — never ask the user to remember credentials
7. Commit to GitHub at every major milestone
8. Test every node before moving to the next
9. Never activate without a real test run

**The north star:** The user should never be confused about what we're doing or why. If something breaks, explain what happened in plain English before fixing it.

---

*Skill version: 1.0*
*Created: May 2026*
*Based on: LinkedIn Autopilot Agent build*
*Repository: github.com/satyasai7090/linkedin-autopilot-agent*
