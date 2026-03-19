---
title: "Cloudflare WAF Security Incident Triage: A Standard Operating Procedure"
date: 2026-03-19
type: article
status: draft
tags: [cloudflare, waf, security, sop, firewall, incident-response, graphql, wrangler, operations]
summary: "A repeatable step-by-step procedure for investigating Cloudflare security events using the GraphQL Analytics API — covering event retrieval, attack summarization, and managed rule identification."
hero: images/cloudflare-waf-security-triage-sop-hero.jpg
created: 2026-03-19
---

# Cloudflare WAF Security Incident Triage: A Standard Operating Procedure

![](/images/cloudflare-waf-security-triage-sop-hero.jpg)

## Overview

Cloudflare's WAF generates security events every time a firewall rule triggers — whether from managed rules, custom rules, rate limiting, or bot detection. When incidents recur (scanner sweeps, CVE probes, credential stuffing), it's valuable to have a consistent triage procedure so you can quickly identify what's happening, which rules are firing, and whether any gaps exist.

This SOP covers how to pull and interpret security events for any zone using the Cloudflare GraphQL Analytics API and the `wrangler` CLI.

---

## Prerequisites

- `wrangler` CLI installed and authenticated (`wrangler login`)
- Access to the target Cloudflare account and zone
- Your **Account ID** and **Zone ID** on hand

### Quick Reference — mcmsp.dev

| Field | Value |
|---|---|
| Account | Master Concept Demo (mcmsp.dev) |
| Account ID | `b326904912840c25f63808a1d1e479aa` |
| Zone ID | `f7e211c055a837203d618519e74665d9` |

---

## The Prompt

Paste the following into any Claude Code session to run a full triage:

```
Check Cloudflare security analytics for mcmsp.dev (account: Master Concept Demo,
account_id: b326904912840c25f63808a1d1e479aa, zone_id: f7e211c055a837203d618519e74665d9).

Use the Cloudflare GraphQL API with wrangler auth token.

Steps:
1. Query `firewallEventsAdaptive` for the last 30 days (all actions, limit 100, orderBy datetime_DESC)
   Fields: action, clientIP, clientRequestPath, clientRequestQuery, ruleId, source, datetime

2. Summarize:
   - Total events, breakdown by action and source
   - Top attacker IPs and hit counts
   - Top blocked paths (most common first)
   - Date range of events

3. List all events in a table: #, Datetime, Action, Source, Client IP, Path

4. For each unique ruleId found, re-query firewallEventsAdaptive with extra fields:
   description, matchIndex, ref
   Then summarize which managed rule triggered and what CVEs/attack category it covers.

5. Highlight any events with action = log, challenge, managed_challenge, or skip
   — these are candidates to upgrade to block.
```

---

## How It Works

### Step 1 — Retrieve the Auth Token

```bash
wrangler auth token
```

This outputs a bearer token used for all subsequent API calls.

### Step 2 — Query Security Events via GraphQL

Cloudflare exposes security analytics through its GraphQL API at `https://api.cloudflare.com/client/v4/graphql`. The key dataset is `firewallEventsAdaptive`.

```bash
CF_TOKEN="<your-token>"
ZONE_ID="f7e211c055a837203d618519e74665d9"
START=$(date -u -v-30d '+%Y-%m-%dT%H:%M:%SZ')
END=$(date -u '+%Y-%m-%dT%H:%M:%SZ')

curl -s -X POST "https://api.cloudflare.com/client/v4/graphql" \
  -H "Authorization: Bearer $CF_TOKEN" \
  -H "Content-Type: application/json" \
  -d "{
    \"query\": \"{ viewer { zones(filter: {zoneTag: \\\"$ZONE_ID\\\"}) {
      firewallEventsAdaptive(
        filter: { datetime_geq: \\\"$START\\\", datetime_leq: \\\"$END\\\" },
        limit: 100,
        orderBy: [datetime_DESC]
      ) { action clientIP clientRequestPath ruleId source datetime }
    } } }\"
  }"
```

### Step 3 — Identify the Blocking Rule

Once you have a `ruleId`, re-query with the `description` field to get the human-readable rule name and associated CVEs:

```bash
curl -s -X POST "https://api.cloudflare.com/client/v4/graphql" \
  -H "Authorization: Bearer $CF_TOKEN" \
  -H "Content-Type: application/json" \
  -d "{
    \"query\": \"{ viewer { zones(filter: {zoneTag: \\\"$ZONE_ID\\\"}) {
      firewallEventsAdaptive(
        filter: { datetime_geq: \\\"$START\\\", datetime_leq: \\\"$END\\\",
                  ruleId: \\\"$RULE_ID\\\" },
        limit: 1
      ) { action ruleId source description matchIndex ref }
    } } }\"
  }"
```

The `description` field returns the rule name, e.g.:
> `DotNetNuke - File Inclusion - CVE:CVE-2018-9126, CVE:CVE-2011-1892 2`

### Step 4 — Assess Gaps

| Action | Meaning | Recommendation |
|---|---|---|
| `block` | Already stopped | No action needed |
| `log` | Observed, not blocked | Review and consider upgrading to block |
| `managed_challenge` | CAPTCHA challenge served | Consider block if clearly malicious |
| `challenge` | JS challenge served | Same as above |
| `skip` | Rule bypassed | Verify bypass is intentional |

---

## Real Example

**Incident:** Mar 15, 2026 — 100 requests from `185.177.72.22` over 2 seconds

| Field | Detail |
|---|---|
| Attacker IP | `185.177.72.22` |
| Paths targeted | `/webhook/*`, `/form/*` upload/file endpoints |
| Rule triggered | `e7e4b386797e417c998d872956c390a1` |
| Rule name | DotNetNuke - File Inclusion |
| CVEs | CVE-2018-9126, CVE-2011-1892 |
| Action | Block (all 100 requests) |
| Source | `firewallManaged` |

**Verdict:** Scanner probing for DNN CMS file inclusion vulnerabilities. Cloudflare managed rules blocked everything. No action required.

---

## Tips

- The API returns a maximum of **100 events per query**. For high-traffic zones, narrow the time window or filter by IP/action.
- `firewallEventsAdaptive` is the recommended dataset — it adapts sampling based on traffic volume.
- Keep your `wrangler` CLI updated (`npm install -g wrangler@latest`) — older versions may have API compatibility issues.
- Bookmark the Cloudflare dashboard path: **Zone → Security → Analytics → Events** for a visual view of the same data.

---

*Created: 2026-03-19*
