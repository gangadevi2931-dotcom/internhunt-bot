# 🎯 InternHunt Bot

> An automated n8n workflow that hunts down internship listings, extracts structured data using Claude AI, filters out duplicates, and delivers a beautifully formatted daily digest email — straight to your inbox.

![Status](https://img.shields.io/badge/status-active-success)
![n8n](https://img.shields.io/badge/built%20with-n8n-EA4B71)
![AI](https://img.shields.io/badge/AI-Claude%20(Anthropic)-6b46c1)
![License](https://img.shields.io/badge/license-MIT-blue)

---

## 📖 Overview

Job hunting shouldn't mean manually refreshing job boards every morning. **InternHunt Bot** automates the entire process — from fetching fresh internship listings to delivering a clean, readable digest email — so you never miss an opportunity.

Built as an end-to-end automation pipeline using **n8n**, this project combines API integrations, LLM-based data extraction, and email templating into a single reliable workflow.

---

## ✨ Features

| Feature | Description |
|---|---|
| 🔍 **Live Job Fetching** | Pulls fresh internship listings via the JSearch API |
| 🤖 **AI-Powered Extraction** | Uses Claude AI (Anthropic) to convert messy job descriptions into clean structured fields |
| 🧹 **Response Cleaning** | Strips markdown/code-fences from AI output and validates JSON before use |
| 🚫 **Auto-Deduplication** | Google Sheets used as a lightweight database to prevent duplicate entries |
| 📧 **Rich HTML Digest** | Color-coded, card-style email — Paid / Stipend / Unpaid badges at a glance |
| ⚙️ **Fully Automatable** | Can run on a schedule for a hands-free daily digest |

---

## 🏗️ Architecture

```
┌─────────────────────────┐
│  Fetch Internship        │
│  Listings (JSearch API)  │
└────────────┬─────────────┘
             │
             ▼
┌─────────────────────────┐
│      Split Out           │
└────────────┬─────────────┘
             │
             ▼
┌─────────────────────────┐
│  Extract Internship Data │
│  (AI Agent + Claude)     │
└────────────┬─────────────┘
             │
             ▼
┌─────────────────────────┐
│  Clean AI JSON Response  │
│  (Code Node)             │
└────────────┬─────────────┘
             │
             ▼
┌─────────────────────────┐
│  Remove Failed Parses    │
│  (Filter Node)           │
└────────────┬─────────────┘
             │
             ▼
┌─────────────────────────┐
│  Save & Dedup to Sheet   │
│  (Google Sheets)         │
└────────────┬─────────────┘
             │
             ▼
┌─────────────────────────┐
│  Build Email HTML Cards  │
│  (Code Node)             │
└────────────┬─────────────┘
             │
             ▼
┌─────────────────────────┐
│    Send Digest Email     │
│    (Gmail)               │
└───────────────────────────┘
```

---

## 🛠️ Tech Stack

- **[n8n](https://n8n.io/)** — Workflow automation & orchestration
- **[Claude AI](https://www.anthropic.com/)** (Anthropic) — Natural language data extraction
- **[JSearch API](https://rapidapi.com/letscrape-6bRBa3QguO5/api/jsearch)** — Job/internship listing source
- **Google Sheets API** — Lightweight deduplication database
- **Gmail API** — Email delivery

---

## 📦 Extracted Data Schema

Each internship listing is normalized into the following structure:

```json
{
  "company": "string",
  "role": "string",
  "pay_type": "Paid | Unpaid | Stipend - [amount] | Not specified",
  "duration": "string",
  "location": "string",
  "apply_link": "string (unique identifier)",
  "posted_date": "string"
}
```

---

## 🚀 Getting Started

### Prerequisites
- An [n8n](https://n8n.io/) instance (cloud or self-hosted)
- A JSearch API key ([RapidAPI](https://rapidapi.com/))
- An Anthropic API key / n8n's built-in Claude connection
- Google Sheets OAuth credentials
- Gmail OAuth credentials

### Installation
1. Clone this repository
   ```bash
   git clone https://github.com/gangadevi2931-dotcom/internhunt-bot.git
   ```
2. Import `InternHunt-Bot.json` into your n8n instance:
   `Workflows → Import from File`
3. Connect your credentials for each node (JSearch, Claude, Google Sheets, Gmail)
4. Create a Google Sheet with the following columns:
   ```
   company | role | pay_type | duration | location | apply_link | posted_date
   ```
5. Run manually or attach a **Schedule Trigger** for full automation

---

## 📸 Screenshots

| Workflow Canvas | Email Digest |
|---|---|
|![Workflow Canvas](InternHunt%20Bot%20Workflow.png)  | ![Email Digest](Gmail%20-%20%F0%9F%8E%AF%20Daily%20Internship%20.jpeg) |

---

## 🧠 Key Learnings

- LLM outputs are rarely "plug-and-play" — reliable automation requires a dedicated parsing/validation layer to handle markdown-wrapped or malformed JSON responses.
- Deduplication logic (matching on a unique key like `apply_link`) is essential when the same data source is queried repeatedly.
- Table-based HTML (rather than modern CSS) is still the most reliable way to build visually rich emails, since most email clients don't render flexbox/grid.

---

## 🔮 Roadmap

- [ ] Relevance filtering based on user-defined interests (AI-based scoring)
- [ ] Posted-date filtering to surface only recent listings
- [ ] WhatsApp/Telegram delivery as an alternative to email
- [ ] Public dashboard view of all tracked internships

---

## 📄 License

This project is licensed under the MIT License — feel free to fork, modify, and build on it.

---

## 🙋 Author

Built by Ganga.S - Pre-final Year Student passionate about AI & Automation, 
actively seeking internship opportunities in this space..
Feel free to connect on https://www.linkedin.com/in/ganga-s-01415b353?utm_source=share_via&utm_content=profile&utm_medium=member_android(#) or reach out with questions!
