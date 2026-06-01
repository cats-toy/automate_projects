# 💰 Automated Finance Tracker (n8n + Notion)

An automated personal finance logging ecosystem designed to track expenses instantly from anywhere. This project supports dual-input architectures: log expenses on the go using a **Telegram Bot** (with real-time confirmation replies) or via a structured **HTML Form** embedded directly inside your **Notion Workspace**.

All data is dynamically centralized, categorized via a lightweight JavaScript engine, and piped directly into a secure Notion Database ledger.

---

## 🚀 Key Features

* **Dual-Input Frontends:**
    * **Telegram Ingestion:** Text expenses directly to a private Telegram bot. The backend parses the data and instantly triggers a success loop back to your active chat thread.
    * **Embedded Notion Form:** A clean HTML form hosted for free (via Tiiny.host or GitHub Pages) and embedded directly into your workspace as an interactive native block.
* **Zero-Bloat Pipeline:** Uses a single JavaScript runtime node to extract strings and format integers, eliminating the need for repetitive `Set` or `Edit Fields` nodes before ingestion.
* **Strict Type Enforcement:** Automatically casts transaction numbers into proper floating-point data types to prevent payload rejections from Notion's API.

---

## 🛠️ Architecture & Data Flow

```text
┌─────────────────┐       ┌─────────────────┐
│  Telegram Bot   │       │ Hosted HTML Form│
└────────┬────────┘       └────────┬────────┘
         │                         │
         ▼                         ▼
┌───────────────────────────────────────────┐
│           n8n Automation Engine           │
│  (Telegram Trigger Node / Webhook Node)   │
└────────────────────┬──────────────────────┘
                     │
                     ▼
┌───────────────────────────────────────────┐
│          JavaScript Code Node             │
│   - Sanitizes text & category parsing    │
│   - Forces numerical data to Floats       │
└────────────────────┬──────────────────────┘
                     │
                     ▼
┌───────────────────────────────────────────┐
│            Notion Database Node           │
│      Appends validated transactional rows │
└────────────────────┬──────────────────────┘
                     │ (If Telegram entry)
                     ▼
┌───────────────────────────────────────────┐
│         Telegram Send Message Node        │
│   Dispatches real-time success receipt    │
└───────────────────────────────────────────┘
