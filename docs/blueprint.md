# System Blueprint

> Complete architecture of the LinkedIn Autopilot Agent.

---

## System Overview

```
6:00 AM  →  Make.com scheduler triggers
6:01 AM  →  Variables set (format, post type, attribution style)
6:02 AM  →  NewsAPI scans AI news from past 24 hours
6:03 AM  →  Top story extracted (headline, summary, URL)
6:04 AM  →  Claude API writes post in Satya's voice
6:05 AM  →  Email preview sent to Satya
6:06 AM  →  WhatsApp preview sent to Satya
8:30 AM  →  Post goes live on LinkedIn
8:31 AM  →  Post logged to Google Sheets
```

---

## Architecture Diagram

```
┌─────────────────────────────────────────────────┐
│                  MAKE.COM SCENARIO               │
│                                                  │
│  [Scheduler 6AM]                                 │
│       ↓                                          │
│  [Set Variables]                                 │
│  format_type / post_type / attribution_style     │
│       ↓                                          │
│  [NewsAPI HTTP Request]                          │
│  Fetch top AI story from past 24 hours           │
│       ↓                                          │
│  [Parse JSON]                                    │
│  Extract headline + summary + URL                │
│       ↓                                          │
│  [Claude API HTTP Request]                       │
│  Write post using master prompt + variables      │
│       ↓                                          │
│  [Parse Claude Response]                         │
│  Extract post text                               │
│       ↓                                          │
│  [Gmail Module] → Email preview to Satya         │
│  [Twilio Module] → WhatsApp preview to Satya     │
│       ↓                                          │
│  [Sleep 9000 seconds = 2.5 hours]                │
│       ↓                                          │
│  [LinkedIn Module] → Post goes live              │
│       ↓                                          │
│  [Google Sheets] → Log post details              │
└─────────────────────────────────────────────────┘
```

---

## Tech Stack

| Layer | Tool | Cost |
|---|---|---|
| Automation platform | Make.com Core | ~$10/month |
| AI writer | Claude API (claude-sonnet-4) | ~$5–10/month |
| LinkedIn publisher | LinkedIn OAuth via Make | Free |
| News aggregator | NewsAPI | Free–$15/month |
| Voice memory | Google Docs | Free |
| Post logger | Google Sheets | Free |
| Email preview | Gmail via Make | Free |
| WhatsApp preview | Twilio / WATI | ~$5/month |

**Total: $20–40/month**

---

## News Sources

### Tier 1 — Mainstream AI (Highest Priority)
- Hacker News
- TechCrunch
- ArXiv
- Product Hunt
- Perplexity AI News

### Tier 2 — Technical Writing & Niche
- Write the Docs
- Reddit r/technicalwriting
- Reddit r/artificial + r/MachineLearning
- Dev.to
- Medium (Towards Data Science)
- GitHub Discussions
- LinkedIn Groups (Technical Writing, AI in Business)

### Story Selection Criteria
1. Backed by actual product launch, research paper, or industry move
2. Appears on 2+ sources OR trending heavily on one Tier 1 source
3. Relevant to: AI + workforce, technical writing transformation, automation in SaaS docs
4. Does NOT touch politics, religion, or controversial social topics
5. Fallback: Evergreen technical writing career content if no qualifying story

---

## Content Strategy

### Two Post Types

**Type A — AI News Reframe (5 days/week)**
- Triggered by real AI news story
- Reframed through technical writer + AI user lens
- Tone: Field notes. Already there. Calm observer.

**Type B — Personal Work Observation (2 days/week)**
- No news story needed
- Real observation from daily work at Redwood
- Tone: Honest. Specific. Like a colleague sharing over coffee.

### Format Rotation

| Day | Format |
|---|---|
| Monday | SHORT (3–5 lines) |
| Tuesday | MEDIUM (150–300 words) |
| Wednesday | LONG (300–500 words) |
| Thursday | SHORT |
| Friday | MEDIUM |
| Saturday | Type B — any format |
| Sunday | Rest (no post) |

### Attribution Style Rotation
- Odd days: INLINE — mention source within post body
- Even days: ENDLINK — add `Source: [URL]` at end only

---

## Safety & Account Protection

- Always use official LinkedIn OAuth (never browser automation)
- Post maximum once per day
- Vary format every day (rotation handles this)
- Never post identical content (Claude generates fresh daily)
- Respond to comments manually — never automate replies
- Refresh OAuth token every 60 days
- If LinkedIn flags account: pause for 7 days immediately

---

## Maintenance Schedule

| Frequency | Time | Task |
|---|---|---|
| Daily | 0 min | Nothing — check WhatsApp preview only |
| Weekly | 5 min | Check LinkedIn notifications, reply to comments |
| Monthly | 15 min | Review performance, update voice samples |
| Every 60 days | 5 min | Refresh LinkedIn OAuth token |
| Quarterly | 30 min | Update prompt if positioning shifts |

---

## Iteration Process

When a post performs well (500+ impressions):
- Add its opening line to the voice profile as a new sample
- Note what topic/angle worked
- Feed pattern back into Claude prompt

When a post underperforms (3 days in a row):
- Review format rotation
- Check if topic is too niche or too broad
- Adjust accordingly
