# LinkedIn Autopilot Agent 🤖

> A fully automated LinkedIn posting agent built on Make.com + Claude API — posting daily AI news reframes and personal observations in my voice, every morning, without manual effort.

---

## What This Is

A personal LinkedIn automation system that:

- Scans 10+ global AI and technical writing news sources every morning
- Identifies the most viral or impactful story from the previous day
- Writes a post in my exact voice and style using Claude API
- Sends me a WhatsApp + email preview before posting
- Posts automatically to my LinkedIn personal profile at 8:30 AM
- Logs every post to Google Sheets for tracking

**Built by:** Satya Sai Pasupuleti — Senior Technical Writer, Redwood Software  
**Stack:** Make.com · Claude API (Anthropic) · LinkedIn OAuth · NewsAPI · Gmail · Twilio  
**Status:** 🔨 In Progress

---

## My Positioning

> A technical writer without AI is good.  
> A technical writer with AI is a different category entirely.

This agent doesn't just automate posting — it automates *thinking out loud* about how technical writers can ride the AI wave instead of being swept by it.

---

## Repo Structure

```
linkedin-autopilot-agent/
│
├── README.md                        ← You are here
├── changelog.md                     ← Version history and updates
│
├── docs/
│   ├── blueprint.md                 ← Full system architecture and build guide
│   └── make-com-setup.md            ← Step by step Make.com build guide
│
├── prompts/
│   └── master-prompt.txt            ← Claude API master prompt (copy-paste ready)
│
├── make-com/
│   └── scenario-guide.md            ← Module by module Make.com scenario guide
│
└── voice-profile/
    └── satya-voice.md               ← Writing style, rules, and voice samples
```

---

## The Post Pattern (Locked)

Every post follows this 4-step formula derived from my highest performing post (13,000+ impressions):

```
Step 1 — Opening Provocation
A trending fear, bold claim, or unexpected observation.

Step 2 — Reframe
Flip the assumption. Show the unexpected angle.

Step 3 — Three Sharp Lines
Build the argument in exactly three punchy lines.

Step 4 — Quiet Closing Punch
One slow-burn truth. The kind of line people screenshot.
```

---

## Two Post Types

| Type | Trigger | Tone |
|------|---------|------|
| **Type A — AI News Reframe** | Real AI news story with fear/disruption angle | Field notes from someone already there |
| **Type B — Personal Observation** | Real moment from daily work at Redwood | Honest, specific, like a colleague sharing over coffee |

---

## Tech Stack

| Layer | Tool | Cost |
|-------|------|------|
| Automation | Make.com | ~$10/month |
| AI Writer | Claude API (Anthropic) | ~$5–10/month |
| Publisher | LinkedIn OAuth | Free |
| News Source | NewsAPI + Web Search | Free–$15/month |
| Voice Memory | Google Docs | Free |
| Post Log | Google Sheets | Free |
| Email Preview | Gmail via Make.com | Free |
| WhatsApp Preview | Twilio / WATI | ~$5/month |

**Total: ~$20–40/month**

---

## News Sources Monitored

**Tier 1 — Mainstream AI:**
- Hacker News
- TechCrunch
- ArXiv
- Product Hunt
- Perplexity AI News

**Tier 2 — Technical Writing Communities:**
- Write the Docs
- Reddit r/technicalwriting
- Reddit r/artificial
- Dev.to
- Medium (Towards Data Science)
- GitHub Discussions
- LinkedIn Groups

---

## Content Rules (Never Broken)

- ✅ Always show human + AI combination as the power
- ✅ Always stay neutral — never take sides in the AI debate
- ✅ Always sound like field notes, not a TED talk
- ❌ Never say AI replaces technical writers
- ❌ Never say technical writers don't need AI
- ❌ No politics, religion, or controversy
- ❌ No exclamation marks. Ever.
- ❌ No jargon: game-changer, revolutionary, disruptive

---

## Roadmap

- [x] System design and architecture
- [x] Voice profile analysis from real posts
- [x] Claude master prompt finalized
- [ ] Make.com account setup
- [ ] NewsAPI integration
- [ ] Claude API integration
- [ ] LinkedIn OAuth connection
- [ ] Gmail preview setup
- [ ] WhatsApp preview setup (Twilio)
- [ ] Full test run
- [ ] Go live

---

## Connect

- LinkedIn: [Satya Sai Pasupuleti](https://www.linkedin.com/in/satya-sai-pasupuleti)
- GitHub: [@satyasai7090](https://github.com/satyasai7090)
