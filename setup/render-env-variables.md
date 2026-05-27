# Render.com Environment Variables

> Complete reference for all environment variables set in Render.com for the n8n deployment.
> Actual values are stored in Notion (private). Never commit real values to GitHub.

---

## All Variables

| Variable | Value | Purpose |
|---|---|---|
| `N8N_BASIC_AUTH_ACTIVE` | `true` | Enables login protection |
| `N8N_BASIC_AUTH_USER` | `satya` | n8n login username |
| `N8N_BASIC_AUTH_PASSWORD` | [see Notion] | n8n login password |
| `DB_TYPE` | `postgresdb` | Database type |
| `DB_POSTGRESDB_HOST` | `aws-1-ap-northeast-1.pooler.supabase.com` | Supabase pooler host |
| `DB_POSTGRESDB_PORT` | `5432` | Database port |
| `DB_POSTGRESDB_DATABASE` | `postgres` | Database name |
| `DB_POSTGRESDB_USER` | `postgres.[PROJECT_ID]` | Database user with project ID |
| `DB_POSTGRESDB_PASSWORD` | [see Notion] | Supabase DB password |
| `DB_POSTGRESDB_SSL` | `true` | SSL required for Supabase |
| `N8N_HOST` | `n8n-9a0r.onrender.com` | Server hostname |
| `N8N_PORT` | `5678` | n8n port |
| `N8N_PROTOCOL` | `https` | Protocol |
| `WEBHOOK_URL` | `https://n8n-9a0r.onrender.com` | Public URL for webhooks |
| `N8N_EDITOR_BASE_URL` | `https://n8n-9a0r.onrender.com` | Base URL for OAuth callbacks |
| `N8N_RUNNERS_ENABLED` | `false` | Disables Python runner (not available on free tier) |
| `N8N_ENCRYPTION_KEY` | [see Notion] | 32-char encryption key for credentials |

---

## Important Notes

- `DB_POSTGRESDB_USER` must include the Supabase project ID in format: `postgres.PROJECTID`
- Both `WEBHOOK_URL` and `N8N_EDITOR_BASE_URL` must match your actual Render URL
- `N8N_HOST` should be the hostname only (no https://)
- Never store actual passwords or API keys in this file

---

## Supabase Connection

Supabase requires Session Pooler connection for Render free tier:
- Use pooler host, not direct host
- Port 5432 for session pooler
- Include project ID in username

Direct connection host (for reference only):
`sfidpbngzfdymhwxxtpe.supabase.co`

Session pooler host (use this):
`aws-1-ap-northeast-1.pooler.supabase.com`
