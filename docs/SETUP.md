# 📖 Setup Guide — Shield Mail

Complete step-by-step setup instructions for running Shield Mail locally.

---

## Prerequisites

| Requirement | Version | Link |
|-------------|---------|------|
| n8n | Latest | [n8n.io](https://n8n.io) |
| Node.js | 18+ | [nodejs.org](https://nodejs.org) |
| Git | Any | [git-scm.com](https://git-scm.com) |
| Google Account | - | Gmail to monitor |
| Groq Account | Free | [console.groq.com](https://console.groq.com) |

---

## Step 1 — Install & Start n8n

```bash
# Install n8n globally
npm install n8n -g

# Start n8n
n8n start
```

Open browser → `http://localhost:5678`

---

## Step 2 — Import Workflow

1. In n8n → click **+** → **New Workflow**
2. Top right `...` menu → **Import from JSON**
3. Paste contents of `shieldmail_n8n_workflow.json`
4. Click **Import**

---

## Step 3 — Google Cloud Setup

### 3a. Create Project
1. Go to [console.cloud.google.com](https://console.cloud.google.com)
2. Click project dropdown → **New Project**
3. Name: `ShieldMail` → **Create**

### 3b. Enable APIs
1. **APIs & Services** → **Library**
2. Search and enable:
   - ✅ Gmail API
   - ✅ Google Sheets API
   - ✅ Google Drive API

### 3c. Configure OAuth Consent Screen
1. **APIs & Services** → **OAuth consent screen**
2. User Type → **External** → **Create**
3. Fill:
   - App name: `ShieldMail`
   - Support email: your Gmail
   - Developer email: your Gmail
4. Save and Continue through all steps
5. **Audience** → **Test Users** → Add your Gmail

### 3d. Create OAuth Credentials
1. **APIs & Services** → **Credentials**
2. **+ Create Credentials** → **OAuth Client ID**
3. Application type: **Web Application**
4. Name: `n8n Local`
5. Authorized redirect URIs → Add:
   ```
   http://localhost:5678/rest/oauth2-credential/callback
   ```
6. **Create** → Copy **Client ID** and **Client Secret**

---

## Step 4 — Get Groq API Key

1. Go to [console.groq.com/keys](https://console.groq.com/keys)
2. **Create API Key** → Copy key (starts with `gsk_`)

---

## Step 5 — Configure n8n Credentials

### Gmail OAuth2
1. n8n → **Credentials** → **Add Credential** → `Gmail OAuth2 API`
2. Paste Client ID + Client Secret
3. Click **Sign in with Google** → authorize

### Groq API (HTTP Header Auth)
1. n8n → **Credentials** → **Add Credential** → `Header Auth`
2. Fill:
   ```
   Name: Gemini Header Auth
   Value: Bearer gsk_YOUR_GROQ_KEY_HERE
   ```

### Google Sheets OAuth2
1. n8n → **Credentials** → **Add Credential** → `Google Sheets OAuth2 API`
2. Paste same Client ID + Client Secret
3. Click **Sign in with Google** → authorize

---

## Step 6 — Create Google Sheet

1. Go to [sheets.google.com](https://sheets.google.com)
2. New sheet → name it `ShieldMail Logs`
3. Add headers in Row 1:

| A | B | C | D | E | F |
|---|---|---|---|---|---|
| Timestamp | Sender | Subject | Classification | Confidence | Reason |

4. Copy the URL from browser

---

## Step 7 — Update Workflow Nodes

### Log to Google Sheet node
- Open node → paste your Sheet URL in Document field

### Send Spam Alert node
- Open node → update **To** field with your Gmail address

---

## Step 8 — Activate & Test

1. Click **Publish** in n8n (top right)
2. Send a test spam email to your monitored Gmail:

```
Subject: URGENT: You won $1,000,000 — Claim NOW!!!
Body: Click here to claim your prize. Send bank details. Limited time!!!
```

3. Wait 60 seconds → verify:
   - ✅ Email labeled `Spam-AutoDetected` in Gmail
   - ✅ Row added to Google Sheet
   - ✅ Alert email received in inbox

---

## Troubleshooting

| Issue | Fix |
|-------|-----|
| OAuth blocked (403) | Add Gmail to Test Users in Google Cloud Console |
| Groq API 401 error | Check `Bearer ` prefix before API key |
| Sheet not logging | Verify column headers match exactly |
| Emails not triggering | Check workflow is Published (not just saved) |
| IF node not routing | Recreate condition: `classification` equals `SPAM` |

---

## Support

Open an issue on [GitHub](https://github.com/aryanp7974-crypto/Shield-Mail/issues)
