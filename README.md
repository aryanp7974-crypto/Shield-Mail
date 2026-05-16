# 🛡️ Shield Mail — AI-Powered Spam Email Detection System

> A fully automated spam email detection workflow built with **n8n**, **Groq LLaMA 3.3 70B**, **Gmail API**, and **Google Sheets** — detects, labels, logs, and alerts on spam emails in real time.

![n8n](https://img.shields.io/badge/Built%20with-n8n-FF6D00?style=flat-square&logo=n8n)
![Groq](https://img.shields.io/badge/AI-Groq%20LLaMA%203.3%2070B-8B5CF6?style=flat-square)
![Gmail](https://img.shields.io/badge/Gmail-API-EA4335?style=flat-square&logo=gmail)
![Google Sheets](https://img.shields.io/badge/Google%20Sheets-Logging-34A853?style=flat-square&logo=googlesheets)
![License](https://img.shields.io/badge/License-MIT-blue?style=flat-square)

---

## 📌 Project Overview

**Shield Mail** is a project that automates Gmail inbox protection using AI. Every incoming email is analyzed by Groq's LLaMA 3.3 70B model and classified as `SPAM` or `CLEAN` with a confidence score and reason. Detected spam is automatically labeled, logged, and reported.

---

## ✨ Features

- 📧 **Real-time Gmail Monitoring** — Polls inbox every 60 seconds via OAuth2
- 🧠 **AI Classification** — Groq LLaMA 3.3 70B with 98%+ confidence scoring
- 🏷️ **Auto Labeling** — Spam emails automatically moved to `Spam-AutoDetected` folder
- 📊 **Audit Logging** — Every detection logged to Google Sheets with full metadata
- 🔔 **Instant Alerts** — Email notification sent when spam is detected
- ⚡ **Zero Manual Effort** — Fully automated end-to-end pipeline

---

## 🏗️ System Architecture

```
Gmail Inbox
    ↓
Gmail Trigger (n8n) — polls every 60s
    ↓
Extract Email Fields — subject, body, sender
    ↓
Groq API (LLaMA 3.3 70B) — AI classification
    ↓
Parse Response — extract JSON result
    ↓
IF Spam?
    ├── TRUE  → Add Label + Log to Sheet + Send Alert
    └── FALSE → No Action (Clean email)
```

---

## 🧰 Tech Stack

| Tool | Purpose |
|------|---------|
| [n8n](https://n8n.io) | Workflow automation engine (self-hosted) |
| [Groq API](https://console.groq.com) | LLaMA 3.3 70B for AI spam classification |
| Gmail API | Email trigger, labeling, and alert sending |
| Google Sheets | Audit log database |
| Google Cloud OAuth2 | Authentication for Gmail + Sheets |

---

## 📁 Project Structure

```
Shield-Mail/
├── shieldmail_n8n_workflow.json   # n8n importable workflow
├── SpamEmailDetection_Final.pptx  # Project presentation (15 slides)
└── README.md
```

---

## 🚀 Setup & Installation

### Prerequisites
- [n8n](https://docs.n8n.io/hosting/) self-hosted (localhost:5678)
- Google Cloud Project with Gmail API + Google Sheets API enabled
- Groq API Key (free at [console.groq.com](https://console.groq.com))
- Gmail account

### Step 1 — Import Workflow
1. Open n8n at `localhost:5678`
2. New Workflow → `...` menu → **Import from JSON**
3. Paste contents of `shieldmail_n8n_workflow.json`

### Step 2 — Setup Google Cloud OAuth
1. Go to [console.cloud.google.com](https://console.cloud.google.com)
2. Create project → Enable **Gmail API** + **Google Sheets API**
3. Create OAuth 2.0 Client ID (Web Application)
4. Add redirect URI: `http://localhost:5678/rest/oauth2-credential/callback`
5. Add your Gmail as test user under **Audience → Test Users**

### Step 3 — Configure Credentials in n8n

| Credential | Type | Details |
|-----------|------|---------|
| Gmail OAuth2 | Gmail OAuth2 API | Client ID + Secret from Google Cloud |
| Groq API | HTTP Header Auth | `Authorization: Bearer YOUR_GROQ_KEY` |
| Google Sheets | Google Sheets OAuth2 | Client ID + Secret from Google Cloud |

### Step 4 — Create Log Sheet
1. Create a new Google Sheet named `ShieldMail Logs`
2. Add headers in Row 1:
```
Timestamp | Sender | Subject | Classification | Confidence | Reason
```
3. Copy the sheet URL

### Step 5 — Activate Workflow
1. Open workflow in n8n
2. Update Google Sheet URL in `Log to Google Sheet` node
3. Update your email in `Send Spam Alert` node
4. Click **Publish** → workflow goes live ✅

---

## 📊 Sample Output

### Google Sheets Log
| Timestamp | Sender | Subject | Classification | Confidence | Reason |
|-----------|--------|---------|----------------|------------|--------|
| 2026-05-14 09:23 | promo@deals.com | URGENT: Claim prize! | SPAM | 95% | Urgent language + suspicious link |
| 2026-05-14 09:31 | prof@college.edu | Assignment deadline | CLEAN | 99% | Legitimate academic email |

### Groq AI Response Format
```json
{
  "classification": "SPAM",
  "confidence": 95,
  "reason": "Generic greeting, urgent timeframe, suspicious link to verify account"
}
```

---

## 🧪 Testing

Send a test spam email to your connected Gmail:

**Subject:** `URGENT: You won $1,000,000 — Claim NOW!!!`

**Body:**
```
Click here to claim your prize. Send your bank details.
Limited time offer. Act fast!!!
http://totally-not-spam.com
```

Wait ~60 seconds → check:
- ✅ Gmail label `Spam-AutoDetected` applied
- ✅ New row in Google Sheet
- ✅ Alert email received

---

## 📈 Performance

| Metric | Value |
|--------|-------|
| Avg AI Confidence | 98.4% |
| Classification Speed | < 1 second |
| Polling Interval | 60 seconds |
| AI Model Parameters | 70 Billion |

---

## 🔮 Future Scope

- 📱 Mobile dashboard for live spam stats
- 🧬 Fine-tune model on personal email history
- 🌐 Multi-platform support (Outlook, Yahoo)
- 🔗 VirusTotal integration for link/attachment scanning
- 📊 Weekly spam analytics reports

---

## 👨‍💻 Author

**Aryan Pandey**
GitHub: [@aryanp7974-crypto](https://github.com/aryanp7974-crypto)

---

## 📄 License

This project is licensed under the MIT License.

---

> ⭐ If you found this useful, consider starring the repo!
