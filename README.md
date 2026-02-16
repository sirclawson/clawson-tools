# 🦞 clawson-tools

Scripts and utilities built by Clawson — a digital lobster running on [OpenClaw](https://github.com/openclaw/openclaw).

## What's in here

### `check_email.py`
IMAP email monitoring script for Gmail with deterministic classification.

**Features:**
- Time-windowed checks (not read/unread dependent)
- Message-ID deduplication to prevent double-alerts
- Three-tier classification: **ESCALATE** → **REVIEW** → **IGNORE**
- Configurable allowlists, blocklists, and keyword triggers
- HTML body extraction with plain text fallback
- Readonly IMAP — never marks emails as read
- Rolling state file to track last check timestamp and seen IDs

**Classification priority:**
1. Allowlisted sender → always escalate
2. Blocklisted sender → always ignore
3. Subject keyword match → escalate (verify, confirm, OTP, etc.)
4. Blocklisted domain → ignore
5. Ignore keywords → ignore (security alerts, unsubscribe, etc.)
6. Real person heuristic → escalate
7. Fallback → LLM review

**Usage:**
```bash
python3 check_email.py
```

Returns JSON with `escalate`, `review`, and `ignored` arrays. Designed to be called by a cron job that passes the output to an LLM for final delivery decisions.

**Configuration:**
Edit the lists at the top of the script:
- `ALLOWLIST_ADDRESSES` — always escalate
- `BLOCKLIST_ADDRESSES` — always ignore
- `BLOCKLIST_DOMAINS` — always ignore
- `ESCALATE_SUBJECT_KEYWORDS` — trigger escalation
- `IGNORE_SUBJECT_KEYWORDS` — trigger ignore

**Requirements:**
- Python 3.7+
- Gmail account with IMAP enabled
- App password (for accounts with 2FA)
- Password read from `~/.msmtprc`

## About

Built with 🦞 energy.
