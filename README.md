# 🧠 Spaced Repetition Autonomous Study Assistant (v1.0)

> An automated, multi-platform study tracking system built with **Make.com**, **Google Sheets**, and **Telegram Bot API** leveraging the Ebbinghaus Forgetting Curve model.

![System Architecture]

<img width="1258" height="542" alt="image" src="https://github.com/user-attachments/assets/78a8b011-0c8e-4753-a143-1e91df29f302" />

---

## 📌 Overview

**Spaced Repetition Autonomous Study Assistant** is a daily workflow automation designed to help students optimize knowledge retention. Instead of overwhelming users with future schedules, the system adheres to a **Just-In-Time (JIT)** notification concept—dispatching Telegram alerts only when a specific topic is due for review on the current day.

This repository hosts **Version 1.0**, featuring a rock-solid, single-scenario 3-module architecture.

---

## 🚀 Key Features

* **Automated Next Review Calculation:** Employs dynamic Google Sheets array formulas (`ARRAYFORMULA` + `IFS`) to auto-calculate review intervals based on mastery levels (Level 1 to Level 4).
* **ISO Date Synchronization:** Standardized `YYYY-MM-DD` date filtering across Google Sheets and Make.com to eliminate timezone discrepancies.
* **Smart Notification Dispatch:** Daily automated trigger (08:00 AM) searching for matching due dates and delivering formatted Markdown/HTML study reminders via Telegram.
* **Audit Logging:** Automatically updates `Last Reminder Sent` timestamps in Google Sheets upon successful delivery.

---

## 🛠️ Tech Stack & Integration Architecture

* **Database Engine:** Google Sheets
* **Workflow Automation Engine:** Make.com (formerly Integromat)
* **Notification Interface:** Telegram Bot API (`@BotFather`)
* **Logic Protocol:** Ebbinghaus Forgetting Curve Algorithm

🔧 System Configuration & Setup
### 1. Database Setup (Google Sheets) Create a new sheet named 
`Spaced Repetition Study Tracker` with the following column structure:

| Column | Header Name | Description / Formula |

| :--- | :--- | :--- | 
| **A** | `Subject` | Name of the academic subject |
| **B** | `Topic` | Specific topic or chapter title |
| **C** | `Date Learned` | Initial study date (`YYYY-MM-DD`) | 
| **D** | `Current Level` | Mastery stage (`Level 1`, `Level 2`, `Level 3`, `Level 4`) | 
| **E** | `Next Review Date` | Auto-calculated date via `ARRAYFORMULA` | 
| **F** | `Telegram Chat ID` | Recipient's Telegram Chat ID |
| **G** | `Last Reminder Sent` | Automatically updated by Make.com | 
#### 📐 Next Review Date Formula(`Cell E2`): ```

```
excel 
=ARRAYFORMULA(IFS( ISBLANK(C2:C), "",
D2:D="Level 1", C2:C + 1, 
D2:D="Level 2", C2:C + 3, 
D2:D="Level 3", C2:C + 7, 
D2:D="Level 4", C2:C + 14, 
D2:D="Mastered", "" ))
```
2. Make.com Scenario Architecture (3-Module Pipeline)

Module 1: Google Sheets — Search Rows
Filter Rule: Next Review Date Equal to formatDate(now; YYYY-MM-DD)
Use column headers as IDs: Yes
Limit: Set to 10 or 20 (prevents operation overflow).
Schedule: Every day at 08:00 AM.

Module 2: Telegram Bot — Send a Text Message
Target Chat ID: {{1.Telegram Chat ID}}
Disable Link Previews: Yes

Message Payload:

```
🧠 SPACED REPETITION REMINDER 🧠

📚 Subject: {{1.Subject}}
📖 Topic: {{1.Topic}}
📊 Current Level: {{1.Current Level}}

✏️ Finished reviewing? Click the link below to update your status in Google Sheets: 🔗 [https://docs.google.com/spreadsheets/d/YOUR_SHEET_ID_HERE]
```
Module 3: Google Sheets — Update a Row
Row Number: {{1.Row number}}
Last Reminder Sent: formatDate(now; YYYY-MM-DD)

📱 Screenshots & Demo
<img width="1258" height="542" alt="image" src="https://github.com/user-attachments/assets/e848dd6e-24fb-4df9-af00-0794c5d421c8" />

<img width="502" height="708" alt="image" src="https://github.com/user-attachments/assets/39b4e2e7-ead9-48ae-8a59-2af340f30ac4" />

🔮 Roadmap & Future Enhancements (v2.0)
[ ] Interactive Telegram Inline Buttons: Implement [ Easy ], [ Medium ], [ Hard ] inline keyboard buttons directly in Telegram messages.
[ ] Two-Way Webhook Automation: Receive callback queries from Telegram buttons to auto-update mastery levels and next review dates in Google Sheets without opening the browser.
[ ] Smart Rescheduling Logic: Automatically adjust interval penalties if a review is missed for more than 48 hours.

📄 License & Acknowledgement
Developed as part of Artificial Intelligence Lab Assignment 3  — Autonomous Agent Systems. Built with an emphasis on minimalist, reliable single-scenario execution.

