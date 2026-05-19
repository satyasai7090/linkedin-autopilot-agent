# Make.com Scenario Build Guide

> Step-by-step instructions to build the LinkedIn Autopilot Agent in Make.com. No technical knowledge required. Follow each step in order.

---

## Before You Start

Make sure you have all of these ready:

- [ ] Make.com account (Core plan ~$10/month)
- [ ] Anthropic API key (from console.anthropic.com)
- [ ] NewsAPI key (from newsapi.org)
- [ ] Google account (for Gmail + Sheets + Docs)
- [ ] LinkedIn account
- [ ] Twilio or WATI account (for WhatsApp)

---

## Phase 1: One-Time Setup (Do Before Building)

### Step 1 — Google Doc (Voice Memory)
1. Go to drive.google.com
2. Create a new Google Doc
3. Name it: `Satya LinkedIn Voice`
4. Paste the contents of `voice-profile/satya-voice.md` into it
5. Copy the document URL — you'll need it later

### Step 2 — Google Sheet (Post Log)
1. Go to drive.google.com
2. Create a new Google Sheet
3. Name it: `LinkedIn Post Log`
4. Add these column headers in Row 1:
   - A: Date
   - B: Post Type
   - C: News Headline
   - D: Format Used
   - E: Post Text
   - F: Source URL
   - G: Status
5. Copy the sheet URL — you'll need it later

---

## Phase 2: Build the Make.com Scenario

### Open Make.com
1. Log into make.com
2. Click **Create a new scenario**
3. You'll see an empty canvas with a circle in the middle

---

### Module 1: Scheduler (Trigger)
1. Click the circle → search **Schedule**
2. Select **Schedule** module
3. Set: **Every day**
4. Set time: **6:00 AM**
5. Set timezone: **Asia/Kolkata (IST)**
6. Click OK

---

### Module 2: Set Variables (Format + Type Rotation)
1. Click **+** after Module 1 → search **Tools**
2. Select **Set Variable**
3. Create variable: `format_type`
   - Formula: Use a counter based on day of week
   - Mon/Thu = SHORT, Tue/Fri = MEDIUM, Wed = LONG
4. Create variable: `post_type`
   - Mon–Fri = TYPE_A_NEWS, Sat–Sun = TYPE_B_OBSERVATION
5. Create variable: `attribution_style`
   - Odd days = INLINE, Even days = ENDLINK

---

### Module 3: Fetch AI News (NewsAPI)
1. Click **+** → search **HTTP**
2. Select **Make a request**
3. URL: `https://newsapi.org/v2/everything`
4. Method: GET
5. Query parameters:
   - `q` = `artificial intelligence OR AI documentation OR technical writing AI`
   - `from` = yesterday's date (use Make.com date formula)
   - `sortBy` = `popularity`
   - `language` = `en`
   - `apiKey` = your NewsAPI key
6. Click OK

---

### Module 4: Parse Top Story
1. Click **+** → search **JSON**
2. Select **Parse JSON**
3. Connect to Module 3 output
4. Extract: `articles[0].title`, `articles[0].description`, `articles[0].url`

---

### Module 5: Write Post with Claude API
1. Click **+** → search **HTTP**
2. Select **Make a request**
3. URL: `https://api.anthropic.com/v1/messages`
4. Method: POST
5. Headers:
   - `x-api-key`: your Anthropic API key
   - `anthropic-version`: `2023-06-01`
   - `content-type`: `application/json`
6. Body (raw JSON):
```json
{
  "model": "claude-sonnet-4-20250514",
  "max_tokens": 1000,
  "messages": [
    {
      "role": "user",
      "content": "[PASTE FULL MASTER PROMPT HERE with {{variables}} replaced by Make.com mappings]"
    }
  ]
}
```
7. Map each `{{variable}}` to the corresponding Make.com output from previous modules
8. Click OK

---

### Module 6: Extract Post Text
1. Click **+** → search **JSON**
2. Parse the Claude API response
3. Extract: `content[0].text`
4. This is your final LinkedIn post

---

### Module 7: Send Email Preview
1. Click **+** → search **Gmail**
2. Select **Send an Email**
3. Connect your Google account
4. To: your email address
5. Subject: `LinkedIn Post Preview — {{today's date}}`
6. Body: Map the post text from Module 6
7. Add footer: "This will post in 2.5 hours. Reply SKIP to this email to cancel."
8. Click OK

---

### Module 8: Send WhatsApp Preview
1. Click **+** → search **Twilio** (or WATI)
2. Select **Send a WhatsApp Message**
3. Connect your Twilio account
4. To: your WhatsApp number
5. Message: First 200 characters of post + "Full preview sent to email."
6. Click OK

---

### Module 9: Wait (2.5 Hour Delay)
1. Click **+** → search **Tools**
2. Select **Sleep**
3. Set delay: **9000 seconds** (= 2.5 hours)
4. This makes the post go live at ~8:30 AM
5. Click OK

---

### Module 10: Post to LinkedIn
1. Click **+** → search **LinkedIn**
2. Select **Create a Post**
3. Click **Add connection** → Sign in with LinkedIn
4. Grant permissions: Share content, View profile
5. Map post text from Module 6
6. Visibility: Public
7. Click OK

---

### Module 11: Log to Google Sheets
1. Click **+** → search **Google Sheets**
2. Select **Add a Row**
3. Connect your Google account
4. Select your `LinkedIn Post Log` spreadsheet
5. Map columns:
   - Date → today's date
   - Post Type → `{{post_type}}` variable
   - News Headline → Module 4 title
   - Format Used → `{{format_type}}` variable
   - Post Text → Module 6 output
   - Source URL → Module 4 url
   - Status → "Posted"
6. Click OK

---

## Phase 3: Testing

### Test Run 1 (Safe — No LinkedIn posting)
1. Temporarily disable Module 10 (right-click → Disable)
2. Click **Run once** button
3. Check that email preview arrives
4. Check post quality — does it sound like you?
5. If yes, proceed to Test Run 2

### Test Run 2 (Full test)
1. Re-enable Module 10
2. Click **Run once** again
3. Verify post appears on your LinkedIn profile
4. Check Google Sheet — new row should appear

### Go Live
1. Click the **ON/OFF toggle** at the bottom of the scenario
2. Switch to **ON**
3. The agent will now run every morning at 6:00 AM automatically

---

## Error Handling

Make.com sends automatic email alerts when any module fails.

| Error | Likely Cause | Fix |
|---|---|---|
| Module 3 fails | NewsAPI key expired or rate limit | Check newsapi.org dashboard |
| Module 5 fails | Anthropic API key issue | Check console.anthropic.com |
| Module 10 fails | LinkedIn OAuth expired | Go to Connections → Reconnect LinkedIn |
| Module 11 fails | Google Sheets permission | Reconnect Google account |

---

## OAuth Token Maintenance

LinkedIn OAuth tokens expire every **60 days**.

Make.com will notify you when it expires. When you get the notification:
1. Go to Make.com → Connections
2. Find LinkedIn connection
3. Click **Reconnect**
4. Sign in with LinkedIn again
5. Done — agent resumes automatically
