# Claude Master Prompt Documentation

> Full documentation of the Claude API prompt used by the LinkedIn Autopilot Agent.

---

## Overview

The master prompt is sent to Claude API every morning via Make.com. It tells Claude exactly who Satya is, how to write, what pattern to follow, and what rules to never break.

The raw prompt ready for copy-paste is in: `prompts/master-prompt.txt`

---

## Prompt Structure

```
1. Identity Statement    — Who Satya is
2. Voice Rules           — How every post must sound
3. Writing Samples       — Real posts to learn from
4. Post Type             — News reframe or personal observation
5. News Story            — Filled dynamically by Make.com
6. Format Type           — Short / Medium / Long
7. Post Pattern          — 4-step structure every post follows
8. Content Rules         — What never to do
9. Output Instructions   — Write the post and nothing else
```

---

## Dynamic Variables

These are filled automatically by Make.com each morning:

| Variable | Values | Set By |
|---|---|---|
| `{{post_type}}` | TYPE_A_NEWS or TYPE_B_OBSERVATION | Day of week logic |
| `{{news_headline}}` | Top AI story title | NewsAPI response |
| `{{news_summary}}` | 2–3 sentence story summary | NewsAPI response |
| `{{news_url}}` | Link to original article | NewsAPI response |
| `{{attribution_style}}` | INLINE or ENDLINK | Daily alternation |
| `{{format_type}}` | SHORT / MEDIUM / LONG | Day of week rotation |

---

## The Post Pattern

Every post follows this exact structure — derived from Satya's highest performing post (13,000+ impressions):

```
Step 1 — Opening Provocation
A trending fear, bold claim, or unexpected observation.
One or two lines. Creates immediate tension or curiosity.

Step 2 — Reframe
Flip the assumption. Show the unexpected angle.
This is where Satya separates from everyone else.

Step 3 — Three Sharp Lines
Build the argument in exactly three punchy lines.
Each line stands alone. Each builds on the previous.

Step 4 — Quiet Closing Punch
One final line. Standalone. Slow burn.
The kind of line people screenshot.
Never a CTA. Never a question. A quiet truth.

Step 5 — Hashtags
4–6 hashtags. Broad + niche mix.
Optional source link if news-based.
```

---

## Content Rules

### Always
- Show AI + human writer as the winning combination
- Sound like field notes from someone already in the future
- Stay calm, confident, three steps ahead
- Connect news stories to what they mean for technical writers

### Never
- Say AI replaces technical writers
- Say technical writers don't need AI
- Post about politics, religion, or controversy
- Use salesy or self-promotional language
- Use hype words: game-changer, revolutionary, disruptive
- Use exclamation marks
- Start a post with "I"
- Sound like a TED talk

---

## When to Update the Prompt

| Trigger | What to Update |
|---|---|
| Post gets 500+ impressions | Add its opening line as a new writing sample |
| 3 consecutive underperforming posts | Review format rotation, adjust rules |
| Role or focus changes at Redwood | Update identity statement |
| New AI tool adopted in daily work | Add as context in voice section |
| Negative audience feedback | Add pattern to the NEVER list |

---

## Model Configuration

```json
{
  "model": "claude-sonnet-4-20250514",
  "max_tokens": 1000
}
```

- Always use `claude-sonnet-4` — best balance of quality and cost
- `max_tokens: 1000` is sufficient for all three format types
- No temperature setting needed — default works well for this use case
