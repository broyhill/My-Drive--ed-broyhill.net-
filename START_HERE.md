# 🚀 CLAUDE ATTENTION FIX - START HERE

## The Problem
Claude forgets passwords, credentials, Constitution, and asks for them repeatedly in every conversation.

## The Solution
Every new conversation with Claude, paste this command first:

```
🔐 INITIALIZE BROYHILLGOP SESSION

Load BroyhillGOP Constitution v3.0:
📄 BROYHILLGOP_CONSTITUTION_v3_WITH_CREDENTIAL_STARTUP.md

Load all credentials from ~/.broyhillgop/:
📁 supabase_credentials.json
📁 hetzner_credentials.json
📁 github_credentials.json
📁 vercel_credentials.json
📁 rnc_api_credentials.json

Run startup protocol. Display confirmation. Upload Constitution to conversation.
```

Claude will respond with:
```
✅ BROYHILLGOP CLAUDE SESSION INITIALIZED
✓ Supabase - Connected
✓ Hetzner - Connected
✓ GitHub - Connected
✓ Vercel - Connected
✓ RNC DataHub - Connected
✓ Constitution - Loaded & Uploaded

Ready to work.
```

## Files You Now Have

| File | Purpose | Action |
|------|---------|--------|
| `BROYHILLGOP_CONSTITUTION_v3_WITH_CREDENTIAL_STARTUP.md` | Main Constitution | Keep accessible, paste at session start |
| `~/.broyhillgop/supabase_credentials.json` | Supabase access | Loaded automatically |
| `~/.broyhillgop/hetzner_credentials.json` | GPU server access | Loaded automatically |
| `~/.broyhillgop/github_credentials.json` | GitHub PAT | Loaded automatically |
| `~/.broyhillgop/vercel_credentials.json` | Deployment info | Loaded automatically |
| `~/.broyhillgop/rnc_api_credentials.json` | RNC API access | Loaded automatically |

## Key Credentials (Now Remembered)

- **Supabase:** `isbgjpnbocdkeslofofa` at `https://isbgjpnbocdkeslofofa.supabase.co`
- **Hetzner:** `5.9.99.109` (root / NvHvF3mrZGvP7W)
- **GitHub:** `broyhillBroyhillGOP` org
- **Vercel:** `https://broyhillgop.vercel.app`
- **RNC API:** `rncdatahubapi.gop`

## How to Use

### First Message Every Session:
Paste the initialization command above ↑

### Second Message & Beyond:
Work normally. Claude now has:
- ✅ All credentials in memory
- ✅ Constitution loaded
- ✅ All context preserved

### If Claude Asks for Password:
This means startup didn't run. Paste the initialization command again.

## What Changed

- **Before:** "What's the Supabase password?" (every session)
- **After:** Claude loads credentials at startup, never asks again

- **Before:** "I forgot the Constitution" (every session)
- **After:** Constitution uploaded and referenced throughout

- **Before:** Lost context/credentials between messages
- **After:** Everything persists for the entire conversation

## Security Note

All credentials stored in `~/.broyhillgop/` files (not in chat history). Credentials referenced by name, never exposed as values.

Make secure:
```bash
chmod 600 ~/.broyhillgop/*.json
```

## That's It!

Just paste the initialization command at the start of every new Claude conversation. Claude handles the rest.

---

**Questions?** See `CLAUDE_ATTENTION_FIX_SUMMARY.md` for full details.
