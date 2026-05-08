---
title: "Agentspace License Dashboard: Dev Log & Runbook"
date: 2026-05-08
type: article
status: draft
tags: [article, gemini, claude-code, kf]
summary: "Full development log and complete runbook for building a weekly automated HTML dashboard that pulls Google Agentspace (Gemini Enterprise) user license data via the Discovery Engine API and publishes to sharehub — written for a gcloud admin to own, run, and extend."
hero: images/agentspace-license-dashboard-dev-log-hero.jpg
created: 2026-05-08
---

# Agentspace License Dashboard: Dev Log & Runbook

![](/images/agentspace-license-dashboard-dev-log-hero.jpg)

## What Was Built

A weekly automated process that:
1. Pulls all user license records from **Google Agentspace** (marketed as "Gemini Enterprise Plus") via the GCP Discovery Engine API
2. Flags **ASSIGNED** users who haven't logged in for **14+ days**
3. Generates a self-contained **HTML dashboard**
4. **Git commits and pushes** to [sharehub.zorro.hk/gemini-license-dashboard.html](https://sharehub.zorro.hk/gemini-license-dashboard.html)
5. Runs automatically every **Monday at 9am** via cron

**Live dashboard:** `https://sharehub.zorro.hk/gemini-license-dashboard.html`  
**Script location:** `~/Dev/gcloud/gemini-license-check.py`  
**GCP project:** `solutionday-cloudsummit`

---

## The Investigation: What We Tried First (and Why It Failed)

This section exists so you don't repeat the same dead ends.

### Dead End 1 — `gcloud gemini` CLI
```bash
gcloud gemini --help
# Groups: code-repository-indexes, code-tools-settings,
#         gemini-gcp-enablement-settings, logging-settings...
```
No user-license subcommand exists in any `gcloud gemini` group (stable, beta, or alpha). The gcloud CLI covers configuration settings only, not license assignment.

### Dead End 2 — Admin SDK Licensing API (`licensing.googleapis.com`)
This is the **Google Workspace** licensing API for Workspace products (Gmail, Drive, etc.). It requires:
- The `apps.licensing` OAuth scope
- A **Workspace super admin** account

Standard `gcloud auth login` doesn't include this scope. Attempting `gcloud auth application-default login --scopes=...apps.licensing` triggers a "unsafe application" block from Google.  
**Wrong product entirely** — Agentspace is a GCP product, not a Workspace product.

### Dead End 3 — `cloudaicompanion.googleapis.com`
The Gemini for Google Cloud API. Confirmed enabled on the project. But its full API surface (via discovery) only exposes settings bindings:
- `codeToolsSettings`, `loggingSettings`, `geminiGcpEnablementSettings`, `releaseChannelSettings`

No user-license endpoints. The `--product` flag only accepts: `gemini-cloud-assist`, `gemini-code-assist`, `gemini-in-bigquery`, `gemini-in-looker`.

### What Actually Works — `discoveryengine.googleapis.com`

The product behind "Gemini Enterprise Plus" in the GCP Console is **Google Agentspace**, which runs on the **Discovery Engine API**. The Console's "Manage users" page calls this API directly.

**Key insight:** Standard `gcloud auth login` credentials with the `cloud-platform` scope are sufficient. No Workspace admin access needed.

---

## The API

**Base URL:**
```
https://discoveryengine.googleapis.com/v1alpha/projects/{PROJECT}/locations/global/userStores/default_user_store/userLicenses
```

**Required headers:**
```
Authorization: Bearer <gcloud auth print-access-token>
x-goog-user-project: solutionday-cloudsummit
```

The `x-goog-user-project` header is mandatory — the API refuses calls without a quota project.

**Response structure (one record):**
```json
{
  "userPrincipal": "user@domain.com",
  "user": "projects/745425319636/locations/global/userStores/default_user_store/users/<hash>",
  "licenseAssignmentState": "ASSIGNED",
  "licenseConfig": "projects/.../licenseConfigs/free_trial_agent_space",
  "createTime": "2026-03-18T10:07:06Z",
  "updateTime": "2026-05-08T03:12:00Z",
  "lastLoginTime": "2026-05-08T03:12:00Z"
}
```

**`licenseAssignmentState` values:**
| Value | Meaning |
|-------|---------|
| `ASSIGNED` | Has a license |
| `NO_LICENSE` | In the system, no license assigned |
| `NO_LICENSE_ATTEMPTED_LOGIN` | Tried to log in but has no license |

---

## The Complete Script

Save at `~/Dev/gcloud/gemini-license-check.py`. No external dependencies — stdlib only.

```python
#!/usr/bin/env python3
"""
Weekly Gemini Enterprise (Agentspace) license inactivity checker.
Fetches user license data, flags ASSIGNED users inactive for 14+ days,
generates an HTML dashboard, and pushes to sharehub for publishing.
"""

import urllib.request
import json
import subprocess
import os
from datetime import datetime, timezone, timedelta

# ── Config ────────────────────────────────────────────────────────────────────
PROJECT         = "solutionday-cloudsummit"
INACTIVITY_DAYS = 14                                    # change threshold here
SHAREHUB_DIR    = os.path.expanduser("~/Dev/sharehub")
OUTPUT_FILE     = os.path.join(SHAREHUB_DIR, "gemini-license-dashboard.html")
API_BASE        = (
    f"https://discoveryengine.googleapis.com/v1alpha/projects/{PROJECT}"
    f"/locations/global/userStores/default_user_store/userLicenses"
)

# ── Fetch data ────────────────────────────────────────────────────────────────
def get_token():
    return subprocess.check_output(
        ["gcloud", "auth", "print-access-token"]
    ).decode().strip()

def fetch_all_licenses(token):
    headers = {
        "Authorization": f"Bearer {token}",
        "x-goog-user-project": PROJECT,
    }
    rows, page_token = [], ""
    while True:
        url = API_BASE + "?pageSize=100" + (f"&pageToken={page_token}" if page_token else "")
        req = urllib.request.Request(url, headers=headers)
        resp = json.loads(urllib.request.urlopen(req).read())
        rows.extend(resp.get("userLicenses", []))
        page_token = resp.get("nextPageToken", "")
        if not page_token:
            break
    return rows

# ── Analyse ───────────────────────────────────────────────────────────────────
def analyse(rows):
    now    = datetime.now(timezone.utc)
    cutoff = now - timedelta(days=INACTIVITY_DAYS)
    assigned = [r for r in rows if r.get("licenseAssignmentState") == "ASSIGNED"]

    inactive, active = [], []
    for u in assigned:
        raw = u.get("lastLoginTime", "")
        if raw:
            last     = datetime.fromisoformat(raw.replace("Z", "+00:00"))
            days_ago = (now - last).days
            if last < cutoff:
                inactive.append({**u, "_days": days_ago})
            else:
                active.append({**u, "_days": days_ago})
        else:
            inactive.append({**u, "_days": None})   # assigned but never logged in

    inactive.sort(key=lambda x: (x["_days"] is None, -(x["_days"] or 9999)))
    return assigned, active, inactive, now

# ── HTML generation ───────────────────────────────────────────────────────────
def badge(days):
    if days is None:
        return '<span class="badge never">Never logged in</span>'
    if days > 30:
        return f'<span class="badge critical">{days}d ago</span>'
    return f'<span class="badge warn">{days}d ago</span>'

def render_row(u):
    email    = u.get("userPrincipal", "")
    raw      = u.get("lastLoginTime", "")
    last_str = raw[:10] if raw else "—"
    days     = u.get("_days")
    css      = "never" if days is None else ("critical" if days > 30 else "warn")
    return f"""
      <tr class="row-{css}">
        <td>{email}</td>
        <td>{last_str}</td>
        <td>{badge(days)}</td>
      </tr>"""

def generate_html(rows, assigned, active, inactive, generated_at):
    no_license_count = sum(1 for r in rows if r.get("licenseAssignmentState") == "NO_LICENSE")
    attempted_count  = sum(1 for r in rows if r.get("licenseAssignmentState") == "NO_LICENSE_ATTEMPTED_LOGIN")
    rows_html        = "\n".join(render_row(u) for u in inactive)
    ts               = generated_at.strftime("%Y-%m-%d %H:%M UTC")
    next_run         = (generated_at + timedelta(days=7)).strftime("%Y-%m-%d")

    return f"""<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Gemini Enterprise License Dashboard</title>
<style>
  :root {{
    --bg:#f8fafc;--surface:#fff;--border:#e2e8f0;--text:#1e293b;--muted:#64748b;
    --green:#16a34a;--green-bg:#dcfce7;--amber:#d97706;--amber-bg:#fef3c7;
    --red:#dc2626;--red-bg:#fee2e2;--blue:#2563eb;--accent:#4f46e5;
  }}
  *{{box-sizing:border-box;margin:0;padding:0}}
  body{{font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",sans-serif;
        background:var(--bg);color:var(--text);padding:2rem}}
  header{{display:flex;justify-content:space-between;align-items:flex-end;
          margin-bottom:2rem;flex-wrap:wrap;gap:.5rem}}
  h1{{font-size:1.5rem;font-weight:700;color:var(--accent)}}
  .meta{{font-size:.8rem;color:var(--muted);text-align:right}}
  .cards{{display:grid;grid-template-columns:repeat(auto-fit,minmax(160px,1fr));
          gap:1rem;margin-bottom:2rem}}
  .card{{background:var(--surface);border:1px solid var(--border);
         border-radius:12px;padding:1.2rem}}
  .card .num{{font-size:2rem;font-weight:700;line-height:1}}
  .card .lbl{{font-size:.75rem;color:var(--muted);margin-top:.3rem;
              text-transform:uppercase;letter-spacing:.04em}}
  .card.green .num{{color:var(--green)}}.card.amber .num{{color:var(--amber)}}
  .card.red .num{{color:var(--red)}}.card.blue .num{{color:var(--blue)}}
  .card.gray .num{{color:var(--muted)}}
  section{{background:var(--surface);border:1px solid var(--border);
           border-radius:12px;padding:1.5rem}}
  h2{{font-size:1rem;font-weight:600;margin-bottom:1rem}}
  table{{width:100%;border-collapse:collapse;font-size:.88rem}}
  th{{text-align:left;padding:.5rem .75rem;font-size:.75rem;
      text-transform:uppercase;letter-spacing:.05em;color:var(--muted);
      border-bottom:2px solid var(--border)}}
  td{{padding:.6rem .75rem;border-bottom:1px solid var(--border)}}
  tr:last-child td{{border-bottom:none}}
  .badge{{display:inline-block;padding:.2rem .55rem;border-radius:999px;
          font-size:.75rem;font-weight:600}}
  .badge.warn{{background:var(--amber-bg);color:var(--amber)}}
  .badge.critical{{background:var(--red-bg);color:var(--red)}}
  .badge.never{{background:var(--border);color:var(--muted)}}
  .empty{{text-align:center;color:var(--green);padding:2rem;font-weight:500}}
  footer{{margin-top:1.5rem;font-size:.75rem;color:var(--muted);text-align:center}}
</style>
</head>
<body>
<header>
  <div>
    <h1>Gemini Enterprise · License Dashboard</h1>
    <p style="color:var(--muted);font-size:.85rem;margin-top:.25rem">
      Project: {PROJECT} &nbsp;·&nbsp; Inactive threshold: {INACTIVITY_DAYS} days
    </p>
  </div>
  <div class="meta">Updated: {ts}<br>Next check: {next_run}</div>
</header>
<div class="cards">
  <div class="card blue"><div class="num">{len(assigned)}</div><div class="lbl">Assigned licenses</div></div>
  <div class="card green"><div class="num">{len(active)}</div><div class="lbl">Active (≤{INACTIVITY_DAYS}d)</div></div>
  <div class="card red"><div class="num">{len(inactive)}</div><div class="lbl">Inactive (&gt;{INACTIVITY_DAYS}d)</div></div>
  <div class="card gray"><div class="num">{no_license_count}</div><div class="lbl">No license</div></div>
  <div class="card amber"><div class="num">{attempted_count}</div><div class="lbl">Attempted login</div></div>
  <div class="card gray"><div class="num">{len(rows)}</div><div class="lbl">Total users</div></div>
</div>
<section>
  <h2>⚠️ Assigned users inactive for &gt;{INACTIVITY_DAYS} days ({len(inactive)})</h2>
  {"<p class='empty'>✅ All assigned users are active!</p>" if not inactive else f"""
  <table>
    <thead><tr><th>Email</th><th>Last Login</th><th>Status</th></tr></thead>
    <tbody>{rows_html}</tbody>
  </table>"""}
</section>
<footer>Auto-generated by gemini-license-check.py · sharehub.zorro.hk</footer>
</body>
</html>"""

# ── Git push to sharehub ──────────────────────────────────────────────────────
def publish(generated_at):
    msg = f"Auto: Gemini license dashboard {generated_at.strftime('%Y-%m-%d')}"
    subprocess.run(["git", "-C", SHAREHUB_DIR, "add", "gemini-license-dashboard.html"], check=True)
    subprocess.run(["git", "-C", SHAREHUB_DIR, "commit", "-m", msg], check=True)
    subprocess.run(["git", "-C", SHAREHUB_DIR, "push", "origin", "main"], check=True)
    print(f"Published: https://sharehub.zorro.hk/gemini-license-dashboard.html")

# ── Main ──────────────────────────────────────────────────────────────────────
if __name__ == "__main__":
    print("Fetching license data...")
    token = get_token()
    rows  = fetch_all_licenses(token)
    print(f"  {len(rows)} users fetched")

    assigned, active, inactive, now = analyse(rows)
    print(f"  {len(assigned)} assigned · {len(active)} active · {len(inactive)} inactive")

    html = generate_html(rows, assigned, active, inactive, now)
    with open(OUTPUT_FILE, "w") as f:
        f.write(html)
    print(f"Dashboard written: {OUTPUT_FILE}")

    publish(now)
```

---

## Setup & Prerequisites

### 1. Authenticate gcloud
```bash
gcloud auth login
# Log in as any GCP IAM user with Viewer access to solutionday-cloudsummit
```

### 2. Verify the script runs
```bash
python3 ~/Dev/gcloud/gemini-license-check.py
```
Expected output:
```
Fetching license data...
  106 users fetched
  50 assigned · 36 active · 14 inactive
Dashboard written: ~/Dev/sharehub/gemini-license-dashboard.html
Published: https://sharehub.zorro.hk/gemini-license-dashboard.html
```

### 3. The cron job (already set up)
```
0 9 * * 1  /usr/bin/python3 /Users/zorro/Dev/gcloud/gemini-license-check.py >> /tmp/gemini-license-check.log 2>&1
```
Runs every **Monday at 09:00**. Check logs at `/tmp/gemini-license-check.log`.

To edit: `crontab -e`  
To verify: `crontab -l | grep gemini`

---

## How to Customise

### Change the inactivity threshold
Edit line in the Config section:
```python
INACTIVITY_DAYS = 14   # change to 7, 30, etc.
```

### Change the GCP project
```python
PROJECT = "your-project-id"
```
The `userStore` ID `default_user_store` is standard across all Agentspace projects.

### Save a CSV instead of (or alongside) HTML
Add after `fetch_all_licenses()`:
```python
import csv
with open("licenses.csv", "w", newline="") as f:
    w = csv.DictWriter(f, fieldnames=["userPrincipal","licenseAssignmentState","lastLoginTime"])
    w.writeheader()
    w.writerows(rows)
```

### Filter to a specific domain
```python
rows = [r for r in rows if "@hkmci.com" in r.get("userPrincipal","")]
```

### Add email notification for inactive users
After `analyse()`, send a summary email using `smtplib` or pipe to a Slack webhook — the `inactive` list has everything you need.

### Publish to a different path
Change `OUTPUT_FILE` and update the `git add` path in `publish()`.

---

## Key Facts for Future Reference

| Item | Value |
|------|-------|
| GCP project | `solutionday-cloudsummit` |
| API | `discoveryengine.googleapis.com` |
| API version | `v1alpha` |
| UserStore | `default_user_store` |
| Auth scope needed | `cloud-platform` (standard gcloud) |
| Subscription ID | `free_trial_agent_space` |
| Dashboard URL | `sharehub.zorro.hk/gemini-license-dashboard.html` |
| Script | `~/Dev/gcloud/gemini-license-check.py` |
| Cron schedule | Every Monday 09:00 |
| Log file | `/tmp/gemini-license-check.log` |

---

*Created: 2026-05-08*
