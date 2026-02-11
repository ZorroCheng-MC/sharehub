---
title: "Instagram Graph API Token Setup Guide"
date: 2026-02-11
---
# Instagram Graph API Token Setup Guide

## For CCF Shave for Hope Hashtag Bot

**Date:** 2026-02-11
**Prepared by:** Zorro (Development Team)
**For:** Kenny (CCF Team)
**Purpose:** Retrieve an Instagram Graph API access token for the `#shaveforhopehk` / `#剃亮希望` hashtag scraper bot

---

## Prerequisites

Before starting, confirm you have:

- [ ] A **verified** Instagram Business or Creator account (CCF's account)
- [ ] The Instagram account is **linked to a Facebook Page**
- [ ] Admin access to the Facebook Page
- [ ] A Meta Developer account (sign up at https://developers.facebook.com)

> **Important:** The Instagram Basic Display API was deprecated in December 2024. We use the **Instagram Graph API** which requires a Business/Creator account linked to a Facebook Page.

---

## Step 1: Create a Meta Developer App

1. Go to **https://developers.facebook.com**
2. Log in with the Facebook account that manages the CCF Facebook Page
3. Click **My Apps** (top right) > **Create App**
4. Select app type: **Business**
5. Fill in:
   - **App Name:** `CCF Shave for Hope Bot`
   - **App Contact Email:** your CCF email
   - **Business Account:** select the CCF business account (if available)
6. Click **Create App** and confirm your password

---

## Step 2: Add Instagram Graph API Product

1. In your new app dashboard, find the **Add Products** section
2. Locate **Instagram** and click **Set Up**
3. This adds the Instagram Graph API product to your app

---

## Step 3: Configure App Settings

### 3a. Basic Settings

1. Go to **App Settings** > **Basic** (left sidebar)
2. Note down these values (we will need them later):
   - **App ID** — e.g. `123456789012345`
   - **App Secret** — click "Show", confirm password, copy it
3. Add your **App Domains:** `shaveforhope.ccf.org.hk`
4. Set **Privacy Policy URL:** `https://ccf.org.hk/privacy` (or your actual privacy page)
5. Click **Save Changes**

### 3b. Add Platform

1. Still in **App Settings** > **Basic**, scroll to **Platforms**
2. Click **Add Platform** > **Website**
3. Enter **Site URL:** `https://shaveforhope.ccf.org.hk`
4. Click **Save Changes**

---

## Step 4: Get Your Instagram Business Account ID

1. Go to **Graph API Explorer:** https://developers.facebook.com/tools/explorer/
2. Select your app (`CCF Shave for Hope Bot`) from the **Application** dropdown
3. Click **Generate Access Token** (this creates a short-lived User Token)
4. When prompted, grant the following permissions:
   - `instagram_basic`
   - `instagram_manage_comments`
   - `pages_show_list`
   - `pages_read_engagement`
5. Run this query in the explorer:

```
GET /me/accounts?fields=id,name,instagram_business_account{id,username}
```

6. Find the CCF Facebook Page in the results and note down:
   - **Page ID** — e.g. `987654321098765`
   - **Instagram Business Account ID** — e.g. `17841400123456789`

> **This Instagram Business Account ID is critical** — the hashtag search API requires it as the `user_id` parameter.

---

## Step 5: Generate a Long-Lived Access Token

The token from Graph API Explorer is short-lived (expires in ~1 hour). We need a long-lived token (60 days).

### 5a. Get Short-Lived Token

If you don't already have one from Step 4:

1. In **Graph API Explorer**, click **Generate Access Token**
2. Copy the token — this is your **short-lived token**

### 5b. Exchange for Long-Lived Token

Open a terminal (or use a browser) and make this request:

```bash
curl -X GET "https://graph.facebook.com/v21.0/oauth/access_token?\
grant_type=fb_exchange_token&\
client_id=YOUR_APP_ID&\
client_secret=YOUR_APP_SECRET&\
fb_exchange_token=YOUR_SHORT_LIVED_TOKEN"
```

Replace:
- `YOUR_APP_ID` — from Step 3a
- `YOUR_APP_SECRET` — from Step 3a
- `YOUR_SHORT_LIVED_TOKEN` — from Step 5a

**Response:**

```json
{
  "access_token": "EAAI...long_token_string...",
  "token_type": "bearer",
  "expires_in": 5184000
}
```

The `access_token` in the response is your **long-lived token** (valid for 60 days).

> **Copy this token carefully.** This is what we need for the bot.

---

## Step 6: Verify the Token Works

### 6a. Test your identity

```bash
curl "https://graph.facebook.com/v21.0/me?access_token=YOUR_LONG_LIVED_TOKEN"
```

Should return your name and ID.

### 6b. Test hashtag search

```bash
curl "https://graph.facebook.com/v21.0/ig_hashtag_search?\
user_id=YOUR_IG_BUSINESS_ACCOUNT_ID&\
q=shaveforhopehk&\
access_token=YOUR_LONG_LIVED_TOKEN"
```

**Expected response:**

```json
{
  "data": [
    {
      "id": "17843853986012345"
    }
  ]
}
```

The `id` is the hashtag node ID. Note it down.

### 6c. Test fetching recent posts for the hashtag

```bash
curl "https://graph.facebook.com/v21.0/HASHTAG_NODE_ID/recent_media?\
user_id=YOUR_IG_BUSINESS_ACCOUNT_ID&\
fields=id,caption,media_type,media_url,permalink,timestamp&\
access_token=YOUR_LONG_LIVED_TOKEN"
```

If this returns posts, everything is working.

---

## Step 7: Send Credentials to Development Team

Send the following values to Zorro **securely** (not via email — use Signal, WhatsApp, or in person):

| Credential | Example | Description |
|---|---|---|
| **App ID** | `123456789012345` | Meta App ID |
| **App Secret** | `abc123def456...` | Meta App Secret |
| **Long-Lived Access Token** | `EAAI...` | 60-day token |
| **IG Business Account ID** | `17841400123456789` | Instagram account ID |
| **Page ID** | `987654321098765` | Facebook Page ID |

> **Security:** Never share the App Secret or Access Token in public channels, emails, or unencrypted messages.

---

## Token Renewal (Every 60 Days)

Long-lived tokens expire after **60 days**. They must be refreshed before expiry.

### Option A: Manual Refresh

Run this before the token expires:

```bash
curl -X GET "https://graph.facebook.com/v21.0/oauth/access_token?\
grant_type=fb_exchange_token&\
client_id=YOUR_APP_ID&\
client_secret=YOUR_APP_SECRET&\
fb_exchange_token=YOUR_CURRENT_LONG_LIVED_TOKEN"
```

This returns a new 60-day token. Update the bot's configuration with the new token.

### Option B: Automated Refresh (We Will Build This)

The development team will implement an automated Cloud Function that refreshes the token before expiry. Kenny does not need to do anything once the initial token is provided — the bot will handle renewals.

---

## Permissions Summary

| Permission | Purpose |
|---|---|
| `instagram_basic` | Read Instagram profile and media data |
| `instagram_manage_comments` | Required for hashtag search access |
| `pages_show_list` | List Facebook Pages you manage |
| `pages_read_engagement` | Read engagement data from Pages |

---

## Rate Limits to Be Aware Of

| Limit | Value |
|---|---|
| API requests | 200 per hour per account |
| Unique hashtag searches | 30 per 7-day rolling window |
| Token validity | 60 days (must refresh) |

Our bot searches only 2 hashtags (`#shaveforhopehk` and `#剃亮希望`), well within the 30/week limit.

---

## Troubleshooting

### "Invalid OAuth access token"
- Token may have expired — regenerate following Step 5
- Check that the correct App ID and Secret are used

### "Unsupported request" on hashtag search
- Ensure the Instagram account is a **Business** or **Creator** account (not Personal)
- Ensure it is linked to a Facebook Page
- Check that `instagram_basic` and `pages_read_engagement` permissions are granted

### "Application does not have permission"
- The app may need **App Review** by Meta for advanced permissions
- In development mode, the app only works for accounts listed as Testers/Admins in the app settings
- Go to **App Roles** > **Roles** and add the Instagram account holder as a Tester

### No results for hashtag search
- The hashtag may not have any public posts yet
- Only public posts from Business/Creator accounts are returned
- Personal/private account posts are not included

---

## Quick Reference

```
Meta Developer Console:  https://developers.facebook.com
Graph API Explorer:      https://developers.facebook.com/tools/explorer/
Access Token Debugger:   https://developers.facebook.com/tools/debug/accesstoken/
API Documentation:       https://developers.facebook.com/docs/instagram-api/
```

---

**Next Step:** Once Kenny provides the credentials, the development team will configure the scraper bot and begin testing.

**Questions?** Contact Zorro via the project WhatsApp group.
