---
title: "Google Workspace API Setup Guide (for AI Assistants)"
date: 2026-02-09
---
# Google Workspace API Setup Guide (for AI Assistants)

Give your AI assistant full read/write access to Gmail, Drive, Calendar, and Docs via OAuth 2.0.

**Key principle:** Use a dedicated **alias Google account** — never give the AI access to your main account.

---

## Overview

| Step | Who | What |
|------|-----|------|
| 1. Project & APIs | 🤖 AI | Create GCP project, enable APIs |
| 2. OAuth credentials | 👤 You | Create Desktop OAuth client in browser (can't be done via CLI) |
| 3. Auth & verify | 🤖 AI | Run auth flow, you just click "Allow" in browser popup |

The AI handles everything except one browser task — Google doesn't allow creating OAuth credentials via command line.

---

## Step 1: AI Creates the Project (🤖)

Tell your AI:

> Set up Google Workspace API access for `alias@gmail.com`:
> ```bash
> gcloud auth login alias@gmail.com
> gcloud projects create my-ai-workspace --name="AI Workspace"
> gcloud config set project my-ai-workspace
> gcloud services enable \
>   gmail.googleapis.com drive.googleapis.com \
>   calendar-json.googleapis.com docs.googleapis.com \
>   sheets.googleapis.com slides.googleapis.com \
>   tasks.googleapis.com people.googleapis.com
> ```

The `gcloud auth login` opens a browser tab — approve it in your **alias Chrome profile** and come back.

---

## Step 2: You Create OAuth Credentials (👤 ~2 minutes)

This is the one step that **must** be done in the browser. Google has no CLI for creating OAuth client IDs.

1. Open [GCP Credentials](https://console.cloud.google.com/apis/credentials) in your **alias Chrome profile**
2. Make sure the correct project is selected in the top bar
3. If prompted to configure consent screen:
   - User Type → **External** → Create
   - Fill in app name + your alias email → Save and Continue through all steps
4. **Create Credentials → OAuth Client ID**
   - Application type: **Desktop application**
   - Click Create
   - **Download JSON**
5. Tell your AI where you saved the file (e.g. `~/Downloads/client_secret_xxx.json`)

That's it. You're done with the browser.

---

## Step 3: AI Completes Setup (🤖)

Tell your AI:

> OAuth client JSON is at `~/Downloads/client_secret_xxx.json`. Complete the auth:
> ```bash
> pip install google-api-python-client google-auth google-auth-oauthlib
> ```
> Then run the OAuth flow and test Gmail, Calendar, and Docs.

The AI runs a Python script that opens one more browser popup. Just click **Allow** (in your alias profile), then everything is automated.

### What the AI runs behind the scenes:

```python
import json, os
from google_auth_oauthlib.flow import InstalledAppFlow
from google.oauth2.credentials import Credentials
from googleapiclient.discovery import build

SCOPES = [
    'https://www.googleapis.com/auth/gmail.modify',
    'https://www.googleapis.com/auth/calendar',
    'https://www.googleapis.com/auth/drive',
    'https://www.googleapis.com/auth/documents',
    'https://www.googleapis.com/auth/spreadsheets',
]

client_secret = os.path.expanduser('~/Downloads/client_secret_xxx.json')
token_path = os.path.expanduser('~/.google_workspace_mcp/credentials/alias@gmail.com.json')

# First time: browser popup for consent
flow = InstalledAppFlow.from_client_secrets_file(client_secret, SCOPES)
creds = flow.run_local_server(port=0)

# Save tokens for future use
os.makedirs(os.path.dirname(token_path), exist_ok=True)
with open(token_path, 'w') as f:
    f.write(creds.to_json())

# Verify
gmail = build('gmail', 'v1', credentials=creds)
result = gmail.users().messages().list(userId='me', q='is:unread').execute()
print(f"✅ Gmail: {result.get('resultSizeEstimate', 0)} unread emails")
```

After this, tokens are saved locally. The AI can access Google Workspace anytime without further browser interaction.

---

## Daily Usage

Once set up, the AI loads saved tokens and calls Google APIs directly:

```python
import json, os
from google.oauth2.credentials import Credentials
from googleapiclient.discovery import build

token_path = os.path.expanduser('~/.google_workspace_mcp/credentials/alias@gmail.com.json')
with open(token_path) as f:
    creds = Credentials.from_authorized_user_info(json.load(f))

gmail    = build('gmail', 'v1', credentials=creds)
calendar = build('calendar', 'v3', credentials=creds)
drive    = build('drive', 'v3', credentials=creds)
docs     = build('docs', 'v1', credentials=creds)
```

No browser needed. Tokens auto-refresh.

---

## What the AI Can Do

| Service | Capabilities |
|---------|-------------|
| **Gmail** | Read, send, delete, archive, label emails |
| **Drive** | Create, modify, delete, share files |
| **Calendar** | Create, modify, delete events |
| **Docs/Sheets** | Read, create, edit documents |
| **Contacts** | View and manage contacts |

---

## Optional: MCP Server Config

For Claude Code or MCP-compatible tools:

```json
{
  "mcpServers": {
    "google-workspace": {
      "command": "uvx",
      "args": ["workspace-mcp"],
      "env": {
        "GOOGLE_WORKSPACE_MCP_CREDENTIALS_DIR": "~/.google_workspace_mcp/credentials"
      }
    }
  }
}
```

> **Note:** The `workspace-mcp` CLI sometimes hangs in interactive mode. If so, the AI can use the Python client directly — tokens are compatible.

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Wrong Google account in browser | Switch to the **alias Chrome profile** |
| `workspace-mcp` CLI hangs | Use Python `google-api-python-client` directly |
| "API not enabled" | AI runs `gcloud services enable <api>` |
| OAuth token expired | Auto-refreshes if `refresh_token` exists |
| "Invalid client" | Credential JSON must be **Desktop** type |

---

## Security

- **Account isolation** is your primary safety — alias account is disposable
- **Revoke anytime:** [myaccount.google.com/permissions](https://myaccount.google.com/permissions)
- **No passwords stored** — only OAuth tokens, locally on your machine
- All API actions logged in Google Account activity

---

*Last updated: 2026-02-09*
