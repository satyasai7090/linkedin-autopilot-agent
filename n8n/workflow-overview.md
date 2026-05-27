# n8n Workflow — All 11 Nodes

> Complete reference for every node in the LinkedIn Autopilot Agent workflow.

---

## Workflow Name
LinkedIn Autopilot Agent

## n8n Instance
https://n8n-9a0r.onrender.com

---

## Node 1 — Schedule Trigger

**Type:** Schedule Trigger  
**Fires:** Every day  
**Time:** 6:00 AM  
**Timezone:** Asia/Kolkata (IST)  
**Purpose:** Wakes up the entire workflow every morning

---

## Node 2 — Fetch AI News (HTTP Request)

**Type:** HTTP Request  
**Method:** GET  
**URL:**
```
https://newsapi.org/v2/everything?q=artificial+intelligence+technical+writing+automation&sortBy=popularity&language=en&pageSize=5&apiKey={{YOUR_NEWSAPI_KEY}}
```
**Purpose:** Fetches top AI news stories from the past 24 hours

---

## Node 3 — Extract Top Story (Code)

**Type:** Code (JavaScript)  
**Purpose:** Picks the most popular article and builds rich context

```javascript
const articles = $input.first().json.articles;

const filtered = articles.filter(a => 
  a.title && a.description && a.url && 
  a.title !== '[Removed]'
);

const top = filtered[0];

const cleanContent = top.content 
  ? top.content.replace(/\[\+\d+ chars\]/, '').trim()
  : '';

const richContext = `
HEADLINE: ${top.title}
SOURCE: ${top.source.name}
PUBLISHED: ${top.publishedAt}
WHAT HAPPENED (summary): ${top.description}
ARTICLE DETAILS: ${cleanContent}
KEY FACTS TO USE:
- This story is from ${top.source.name}
- Published on ${new Date(top.publishedAt).toDateString()}
- Full article: ${top.url}
WRITING INSTRUCTION:
Use the above to write a LinkedIn post as Satya Sai Pasupuleti — 
a Senior Technical Writer at an AI-first SaaS company who uses AI daily.
Connect this story to what it means for technical writers and documentation professionals.
`.trim();

return [{
  json: {
    headline: top.title,
    summary: top.description,
    richContext: richContext,
    url: top.url,
    source: top.source.name,
    publishedAt: top.publishedAt
  }
}];
```

---

## Node 4 — Fetch Full Article (HTTP Request)

**Type:** HTTP Request  
**Method:** GET  
**URL:** `{{ $json.url }}` (expression — uses URL from Node 3)  
**Response Format:** Text  
**Purpose:** Visits the actual article URL and retrieves full HTML

---

## Node 5 — Clean HTML to Text (Code)

**Type:** Code (JavaScript)  
**Purpose:** Strips HTML tags and extracts clean article text

```javascript
const html = $input.first().json.data;

let clean = html
  .replace(/<script[^>]*>[\s\S]*?<\/script>/gi, '')
  .replace(/<style[^>]*>[\s\S]*?<\/style>/gi, '')
  .replace(/<[^>]+>/g, ' ')
  .replace(/\s+/g, ' ')
  .trim();

const excerpt = clean.substring(0, 3000);

const fullContext = `
${$('Code in JavaScript').first().json.richContext}

FULL ARTICLE TEXT (extracted):
${excerpt}
`.trim();

return [{
  json: {
    headline: $('Code in JavaScript').first().json.headline,
    summary: $('Code in JavaScript').first().json.summary,
    url: $('Code in JavaScript').first().json.url,
    source: $('Code in JavaScript').first().json.source,
    fullContext: fullContext
  }
}];
```

---

## Node 6 — Write Post with Claude (HTTP Request)

**Type:** HTTP Request  
**Method:** POST  
**URL:** `https://api.anthropic.com/v1/messages`

**Headers:**
- `x-api-key`: your Anthropic API key
- `anthropic-version`: `2023-06-01`
- `content-type`: `application/json`

**Body (JSON):**
- `model`: `claude-sonnet-4-5`
- `max_tokens`: `{{ 1000 }}`
- `messages`: `{{ [{"role": "user", "content": $('Code in JavaScript1').first().json.fullContext}] }}`

**Purpose:** Sends the full article context to Claude and gets a LinkedIn post back

---

## Node 7 — Extract Post Text (Code)

**Type:** Code (JavaScript)  
**Purpose:** Pulls the clean post text from Claude's API response

```javascript
const response = $input.first().json;
const postText = response.content[0].text;

return [{
  json: {
    postText: postText,
    headline: $('Code in JavaScript1').first().json.headline,
    url: $('Code in JavaScript1').first().json.url,
    source: $('Code in JavaScript1').first().json.source,
    generatedAt: new Date().toISOString()
  }
}];
```

---

## Node 8 — Send WhatsApp Preview (HTTP Request)

**Type:** HTTP Request  
**Method:** POST  
**URL:** `https://api.twilio.com/2010-04-01/Accounts/{{ACCOUNT_SID}}/Messages.json`  
**Authentication:** Basic Auth (Account SID + Auth Token)  
**Body (Form URLencoded):**
- `From`: `whatsapp:+14155238886`
- `To`: `whatsapp:+919381684789`
- `Body`: `{{ "🤖 LinkedIn Post Preview\n\n" + $('Code in JavaScript2').first().json.postText.substring(0, 1550) }}`

**Purpose:** Sends full post preview to Satya's WhatsApp 2.5 hours before posting

---

## Node 9 — Wait

**Type:** Wait  
**Duration:** 9000 seconds (2.5 hours)  
**Purpose:** Gives Satya time to review preview before post goes live

---

## Node 10 — Post to LinkedIn

**Type:** LinkedIn  
**Operation:** Create Post  
**Post As:** Person  
**Person:** Satya Sai Pasupuleti  
**Text:** `{{ $('Code in JavaScript2').first().json.postText }}`  
**Purpose:** Posts to LinkedIn personal profile via official OAuth

---

## Node 11 — Log to Google Sheets

**Type:** Google Sheets  
**Operation:** Append Row  
**Document ID:** `1KBiCAs3X9PR57pZm9EO7L7XlkalPwb_sj5F0HxdVZZc`  
**Sheet:** Sheet1  

**Column Mapping:**
- `Date`: `{{ new Date().toLocaleDateString('en-IN') }}`
- `Headline`: `{{ $('Code in JavaScript2').first().json.headline }}`
- `Post Text`: `{{ $('Code in JavaScript2').first().json.postText }}`
- `Source URL`: `{{ $('Code in JavaScript2').first().json.url }}`
- `Status`: `Posted`

**Purpose:** Permanently logs every post for performance tracking
