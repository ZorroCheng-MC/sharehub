---
title: "OpenClaw WhatsApp Setup Guide: Getting Your AI Bot to Message Friends"
date: 2026-02-27
type: article
status: draft
tags:
  - article
  - AI
  - automation
  - tools
  - whatsapp
  - openclaw
  - docker
  - messaging
  - chatbot
  - setup-guide
  - published
  - actionable
summary: "A practical guide to configuring OpenClaw's WhatsApp channel inside Docker, covering contact allowlists, E.164 phone number formatting, and testing inbound/outbound messaging."
hero: images/openclaw-whatsapp-setup-guide-hero.jpg
created: 2026-02-27
---

# OpenClaw WhatsApp Setup Guide: Getting Your AI Bot to Message Friends

![](/images/openclaw-whatsapp-setup-guide-hero.jpg)

## TL;DR

1. Add the number to **both** `channels.whatsapp.allowFrom` and `channels.whatsapp.accounts.default.allowFrom`
2. Use E.164 format — no spaces: `+85292319324`
3. Restart the gateway
4. Have the friend send "hi" first, then check `openclaw directory peers list`
5. Send with `openclaw message send --channel whatsapp --target "+852..." --message "hello"`

---

So you've got OpenClaw running in Docker and the WhatsApp channel is connected. Now you want your AI bot to actually message people — and for people to message it back. Sounds simple, but there are a few gotchas that will silently eat your messages if you don't get them right.

This guide covers the exact configuration steps, the pitfalls we discovered the hard way, and how to verify everything works before you tell your friends to try it.

## The Two Things That Must Be Right

OpenClaw's WhatsApp integration has two independent gatekeepers for direct messages:

1. **Top-level `allowFrom`** — controls which numbers the WhatsApp channel will accept
2. **`accounts.default.allowFrom`** — controls which numbers the specific WhatsApp account will accept

Both must include your friend's number. If only the top-level has it, the account-level silently drops the message. Your friend sends "hi" and... nothing. No error, no log entry, just silence.

## Step 1: Add Contacts to the Config

Open your OpenClaw config. In Docker, this is typically mounted at `~/.openclaw/openclaw.json` inside the container (or wherever your volume maps to).

Find the `channels.whatsapp` section and add the number to **both** allowFrom lists:

```json
{
  "channels": {
    "whatsapp": {
      "enabled": true,
      "dmPolicy": "allowlist",
      "allowFrom": [
        "+85290208609",
        "+85292319324"
      ],
      "accounts": {
        "default": {
          "enabled": true,
          "dmPolicy": "allowlist",
          "allowFrom": [
            "+85290208609",
            "+85292319324"
          ]
        }
      }
    }
  }
}
```

### The E.164 Rule

Every phone number **must** be in E.164 format — that means:

- Country code prefix with `+`
- No spaces, no dashes, no parentheses
- Just digits after the `+`

| Wrong | Right |
|-------|-------|
| `+852 9231 9324` | `+85292319324` |
| `+852-9231-9324` | `+85292319324` |
| `852 92319324` | `+85292319324` |
| `92319324` | `+85292319324` |

This matters more than you think. If the OpenClaw agent reads a number with spaces from a contacts file, it will pass `"+852 9231 9324"` to the message tool — and the tool will reject it with `Unknown target`. No automatic cleanup happens.

## Step 2: Restart the Gateway

Config changes don't take effect until the gateway restarts.

**Docker:**
```bash
docker restart <your-openclaw-container>
```

**Docker Compose:**
```bash
docker compose restart openclaw
```

**Native (macOS LaunchAgent):**
```bash
openclaw gateway restart
```

## Step 3: Test Inbound (Friend → Bot)

Ask your friend to send any message to the bot's WhatsApp number. Then check if they appeared in the directory:

```bash
# Docker
docker exec <container> openclaw directory peers list --channel whatsapp

# Native
openclaw directory peers list --channel whatsapp
```

You should see their number in the peers list:

```
Peers (2)
┌─────────────────────┬──────┐
│ ID                   │ Name │
├─────────────────────┼──────┤
│ +85290208609         │      │
│ +85292319324         │      │
└─────────────────────┴──────┘
```

**If they don't appear:** the allowFrom is misconfigured. Double-check both the top-level and `accounts.default` lists.

## Step 4: Test Outbound (Bot → Friend)

First, do a dry run to verify the target resolves:

```bash
# Docker
docker exec <container> openclaw message send \
  --channel whatsapp \
  --target "+85292319324" \
  --message "hello from openclaw" \
  --dry-run

# Native
openclaw message send \
  --channel whatsapp \
  --target "+85292319324" \
  --message "hello from openclaw" \
  --dry-run
```

You should see: `✅ Sent via gateway (whatsapp). Message ID: unknown`

The "unknown" message ID is normal for dry runs. Remove `--dry-run` to actually send:

```bash
docker exec <container> openclaw message send \
  --channel whatsapp \
  --target "+85292319324" \
  --message "hello from openclaw"
```

A real send returns an actual message ID like `3EB0D9B56440663C1D2B73`.

## Step 5: Using It With Claude Code

If you're using Claude Code as your AI agent and want it to resolve contact names (e.g., "message Tommy") instead of raw phone numbers, keep a `CONTACTS.md` file in your workspace with numbers in E.164 format:

```markdown
## Friends

**Tommy Shum**
- WhatsApp: +85292319324

**Gloria Choi**
- WhatsApp: +85290759952
```

The AI agent will read this file and extract the correct number. But if numbers contain spaces, the agent will pass them as-is and the message tool will reject them. This is the single most common failure mode.

## Troubleshooting

### "Unknown target" error

The message tool doesn't recognize the number. Check:
- Is it in E.164 format? No spaces, starts with `+`
- Is the number correct? Typos happen

### Message sent but friend didn't receive it

- The friend's WhatsApp might need to be active (not deleted/deactivated)
- Check the gateway error log: `docker exec <container> cat ~/.openclaw/logs/gateway.err.log | tail -20`

### Friend messaged the bot but got no reply

- Is their number in **both** allowFrom lists?
- Did you restart the gateway after adding them?
- Check the agent session is running: `docker exec <container> openclaw tui` to see active sessions

### Name-based lookup doesn't work

WhatsApp doesn't expose contact names through its API. The directory only stores phone numbers (JIDs). Name resolution must come from your AI agent reading a contacts file — OpenClaw itself can't resolve "Tommy" to a number.

## Quick Reference

| Task | Command |
|------|---------|
| List WhatsApp peers | `openclaw directory peers list --channel whatsapp` |
| Send message (dry run) | `openclaw message send --channel whatsapp -t "+852..." -m "text" --dry-run` |
| Send message (real) | `openclaw message send --channel whatsapp -t "+852..." -m "text"` |
| Check error logs | `cat ~/.openclaw/logs/gateway.err.log \| tail -20` |
| Restart gateway | `docker restart <container>` |

---

*Created: 2026-02-27*
