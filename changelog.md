# Changelog

---

## [2.0.0] — 2026-05-27

### Major: System Gone Live

**Infrastructure built and deployed:**
- n8n self-hosted on Render.com (free tier)
- Supabase PostgreSQL database connected
- UptimeRobot monitoring active (100% uptime)
- All 11 workflow nodes built and tested

**Integrations connected:**
- Anthropic Claude API (claude-sonnet-4-5)
- NewsAPI for daily AI news
- Twilio WhatsApp sandbox for previews
- LinkedIn OAuth for personal profile posting
- Google OAuth + Google Sheets for logging

**Stack changed:**
- Migrated from Make.com plan to n8n self-hosted
- Zero monthly software cost (only Claude API ~$1-5/month)

**Files added:**
- `n8n/workflow-overview.md` — all 11 nodes with full code
- `n8n/setup-guide.md` — complete deployment guide
- `setup/credentials-checklist.md` — all services reference
- `setup/render-env-variables.md` — all environment variables

---

## [1.0.0] — 2026-05-19

### Initial Release

**Foundation built:**
- Complete system architecture and blueprint
- Claude master prompt (v1.0) with Satya's voice profile
- Post pattern locked from highest performing post (13K impressions)
- Two post types: Type A (AI news) and Type B (personal observation)
- Full news source list: 10+ sources
- GitHub repo created: satyasai7090/linkedin-autopilot-agent

**Files added:**
- `README.md`
- `changelog.md`
- `docs/blueprint.md`
- `docs/master-prompt.md`
- `docs/architecture-explained.md`
- `prompts/master-prompt.txt`
- `make-com/scenario-guide.md`
- `voice-profile/satya-voice.md`
