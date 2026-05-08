---
title: "Pulling Gemini Enterprise User License List via Script"
date: 2026-05-08
type: article
status: draft
tags: [article, gemini, claude-code, ai-tools]
summary: "A ready-to-use Claude Code prompt for your Google Workspace admin to build a script that exports the full Gemini Enterprise user license list — and the investigation that led to it."
hero: images/gemini-enterprise-license-script-hero.jpg
created: 2026-05-08
---

# Pulling Gemini Enterprise User License List via Script

![](/images/gemini-enterprise-license-script-hero.jpg)

## Background

The Google Cloud Console at `console.cloud.google.com/gemini-enterprise/user-license` shows a full table of Gemini Enterprise license assignments — who has a license, when it was assigned, and when it was last used. This is useful data for auditing and reporting. The obvious question: can you pull it programmatically?

### What We Investigated

| Approach | Result |
|----------|--------|
| `gcloud gemini` CLI commands | No user-license subcommand exists |
| Admin SDK Licensing API (`licensing.googleapis.com`) | Requires `apps.licensing` OAuth scope — not in default gcloud token |
| `cloudaicompanion.googleapis.com` REST API | Private/internal, not in public API catalog |
| `gcloud auth application-default login --scopes=apps.licensing` | Blocked by Google as "unsafe application" |

**Conclusion:** The Console uses a private internal API. GCP-level credentials alone cannot access it. You need **Google Workspace super admin** credentials with the `apps.licensing` scope, via GAM (Google Apps Manager).

---

## The Claude Code Prompt for Your GWS Admin

Copy and paste this prompt into Claude Code. Your GWS admin needs to be a **Workspace super admin** for the org that owns the Gemini Enterprise subscription.

---

```
I need your help setting up GAM (Google Apps Manager) to export the full
Gemini Enterprise user license list for our organisation.

Context:
- We use Gemini Enterprise Plus (productId: 1010310008) managed through Google Workspace
- I am a Google Workspace super admin
- GAM v7 is already installed at /Users/<you>/bin/gam7/gam
  (if not installed, install via: bash <(curl -s -S -L https://gam-shortn.appspot.com/gam-install))

Please help me:

1. Set up GAM OAuth credentials if not already configured:
   - Run: gam create project
     (opens browser → log in as super admin → creates GCP project + OAuth client)
   - Then run: gam oauth create
     (opens browser again → authorise all requested Workspace scopes)

2. Once authorised, pull the Gemini Enterprise user license list:
   gam print licenses productid 1010310008 > gemini-licenses.csv

3. If that product ID doesn't return results, also try:
   gam print licenses productid 1010310006 >> gemini-licenses.csv
   (covers Gemini Business tier)

4. Parse the CSV output and produce a clean report showing:
   - User email
   - License tier (SKU)
   - Date assigned
   Write it as both CSV and a summary count by license tier.

5. Save the final report as gemini-license-report-YYYY-MM-DD.csv

If you encounter any OAuth scope errors or "insufficient permissions", check that
the account used in gam oauth create is a super admin and that the
apps.licensing scope was included.
```

---

## What This Process Does

1. **`gam create project`** — creates a dedicated GCP project and OAuth 2.0 client credentials specifically for GAM. This bypasses the "unsafe app" restriction that blocks `gcloud auth application-default login` with custom scopes.

2. **`gam oauth create`** — authenticates using the newly created client, with the full set of Workspace admin scopes including `apps.licensing`. Credentials are stored in `~/.gam/oauth2.txt`.

3. **`gam print licenses productid 1010310008`** — calls the Admin SDK Enterprise License Manager API and returns all assigned licenses for the Gemini Enterprise product as CSV.

## Expected Output

```csv
userId,productId,skuId,selfLink
alex.chang@example.com,1010310008,1010310008,https://...
amy.lu@example.com,1010310008,1010310008,https://...
...
```

The output maps directly to what the Cloud Console shows — one row per assigned user.

## Notes

- **Product IDs to know:**
  - `1010310008` = Gemini Enterprise Plus
  - `1010310006` = Gemini Business
- The GAM setup only needs to be done once; after that `gam print licenses` can be run any time or scheduled via cron.
- To automate and schedule: `crontab -e` → `0 9 * * 1 /path/to/gam print licenses productid 1010310008 > ~/reports/gemini-licenses-$(date +\%Y-\%m-\%d).csv`

---

*Created: 2026-05-08*
