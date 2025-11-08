# Telegram Appointment Scheduler

This repository contains an automated appointment scheduling system that allows patients to **book, reschedule, and cancel appointments directly through Telegram**, while keeping **Google Calendar** and **Google Sheets** synchronized in real-time.

**Live Bot:**[Appointment Bot]"https://web.telegram.org/k/#@sriram_appoint_bot"

---

## ✨ Features

| Action | Google Calendar | Google Sheets |
|-------|-----------------|----------------|
| **New Appointment** | Creates a new event | Appends a new row |
| **Reschedule Appointment** | Creates new event + deletes old one | Old row → `Rescheduled`, new row appended |
| **Cancel Appointment** | Deletes event | Row marked `Cancelled` |

Additional Highlights:
- Prevents double-booking
- Enforces clinic operating hours
- Maintains complete appointment history
- Understands natural language through Telegram chat

---

## 🧱 System Architecture

| Component | Purpose |
|----------|---------|
| **Telegram Bot** → `@sriram_appoint_bot` | Patient-facing conversation |
| **n8n Workflow (AI Agent)** | Coordinates scheduling logic and tool calls |
| **Google Calendar** | Stores booked appointments and availability |
| **Google Sheets** | Permanent appointment ledger |
| **OpenAI GPT Model** | Understands user messages and context |

---

## 🕒 Clinic Operating Rules

| Days | Hours |
|------|------|
| Monday–Friday | 9:00 AM – 8:00 PM |
| Saturday | 9:00 AM – 1:00 PM |
| Sunday | Closed |

- Appointment duration: **60 minutes**
- Break time: **15 minutes**

---

## 💾 Required Google Sheet Format

The Google Sheet must contain **exactly** these headers:

```
Patient_Name | Date_and_Time | Phone_Number | Appointment_Status | Event_ID
```

Sheet Tab Name:
```
Sheet1
```

---

## 🔧 Setup Guide

### 1) Create Telegram Bot
1. Open Telegram → Search **BotFather**
2. Run `/newbot`
3. Copy bot token
4. Paste into Telegram Trigger & Send Message nodes in n8n

### 2) Connect Google Calendar
- Ensure your Google API credentials in n8n have calendar read/write access

### 3) Configure Google Sheets
1. Create sheet with required headers
2. Copy Sheet ID from URL
3. Insert Sheet ID in all Google Sheets nodes

### 4) Add OpenAI API Key
- Insert key into OpenAI Chat Model credentials in n8n

### 5) Import Workflow
```
n8n → Workflows → Import → Select JSON file
```

---

## 🔁 Workflow Logic

### ✅ New Booking
```
Creat event → Add data (append)
```

### 🔄 Reschedule
```
Creat event (new)
→ Delete old event
→ Mark old row Rescheduled
→ Add data (append) (new record)
```

### ❌ Cancel
```
Delete event
→ Mark row Cancelled
```

---

## 🧪 Testing

| Action | Expected Outcome |
|-------|----------------|
| Book | Event appears in Calendar + new row added |
| Reschedule | Old event removed + sheet row Rescheduled + new event + new row |
| Cancel | Event removed + sheet row Cancelled |

---

## 📄 License
MIT License

---

## 🤝 Contributions Welcome
PRs and Issues accepted.
