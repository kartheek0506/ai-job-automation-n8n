# 📌 AI Job Automation Workflow (n8n)

## 🧠 Overview

This workflow automates:
- Fetching AI-related job listings from Remotive API
- Filtering relevant AI / Machine Learning roles
- Sending job alerts via Gmail
- Running automatically on a daily schedule

---

## ⚙️ Workflow Architecture

Schedule Trigger → HTTP Request → Split Job Posts → Loop Over Items → IF Filter → Gmail → Loop Back


---

## 🧱 Node-by-Node Breakdown

---

### 1. ⏰ Schedule Trigger (`Daily at 9:00 AM`)

#### 📌 Purpose
Triggers the workflow automatically at a fixed time every day.

#### ⚙️ Configuration
- Trigger Type: Interval
- Hour: `18` (6 PM in 24-hour format)

#### 🧠 Notes
- Node name says "9:00 AM", but actual execution is **6 PM**
- Timezone depends on n8n settings (recommended: Asia/Kolkata)

---

### 2. 🌐 HTTP Request

#### 📌 Purpose
Fetches job data from Remotive API.

#### ⚙️ Configuration
- Method: `GET`
- URL:

https://remotive.com/api/remote-jobs?search=ai

- Query Parameters:
- `search = ai engineer jobs`

#### 🧠 Output Structure
```json
{
"jobs": [
  {
    "title": "AI Engineer",
    "company_name": "Company X",
    "url": "..."
  }
]
}

3. 🔀 Split Job Posts (splitOut)
📌 Purpose

Converts the jobs array into individual job items.

⚙️ Configuration
Field to Split:
jobs

🧠 Transformation

Before:
{
  "jobs": [job1, job2, job3]
}

After:
{ job1 }
{ job2 }
{ job3 }

4. 🔁 Loop Over Items (splitInBatches)
📌 Purpose

Processes each job one at a time.

⚙️ Configuration
Mode: Loop Over Items
Batch Size: Default (1 item per iteration)
🧠 Behavior
Sends one job to IF node
Waits for processing
Continues to next job until all are processed
5. 🔍 IF Node (Filtering Logic)
📌 Purpose

Filters only relevant AI-related job roles.

⚙️ Conditions

Combinator: OR

Condition	Field	Operation	Value
1	title	contains	AI
2	title	contains	Machine Learning
🧠 Notes
Case Sensitive = TRUE (may miss lowercase matches like "ai")
Only checks job title
Does not consider description or tags
6. ✉️ Gmail Node (Send a message)
📌 Purpose

Sends email notifications for filtered jobs.

⚙️ Configuration

To:

vadlakarthik9876@gmail.com

Subject:

New AI Job Found 🚀
📩 Message Template
Job Title: {{ $('Loop Over Items').item.json.title }}

Company: {{ $('Loop Over Items').item.json.company_name }}

Link: {{ $('Loop Over Items').item.json.url }}
🧠 Notes
Uses node reference instead of $json
Requires Gmail OAuth2 authentication
🔁 Loop Back Mechanism
📌 Flow
Gmail → Loop Over Items
🧠 Purpose
Ensures continuous iteration
Processes all job items sequentially
🔄 Data Flow Summary
Scheduler triggers workflow
HTTP request fetches jobs
Jobs array is split into individual items
Loop processes each job
IF filters AI-related roles
Gmail sends email
Loop continues until completion
⚠️ Current Limitations
❌ Case Sensitivity
"ai engineer" will not match "AI"
May miss valid jobs
❌ Limited Filtering
Only uses job title
Ignores:
description
tags
skills
❌ No Fresher Detection

No filtering for:

Junior
Intern
Entry-level roles
❌ Duplicate Emails
Same jobs may be sent daily
No tracking or deduplication
🚀 Recommended Improvements
🔧 Improve Filtering Logic

Add keywords:

AI, ML, NLP, LLM, Deep Learning
🔧 Add Fresher Detection

Check in:

title
description

Keywords:

junior, intern, entry, graduate, trainee
🔧 Disable Case Sensitivity
Set IF node to case-insensitive
Prevent missing valid matches
🔧 Add Deduplication

Store job IDs in:

Google Sheets
Database
Cache
🔧 Improve Email Formatting
Job Title: ${$json.title}
Company: ${$json.company_name}
Apply Here: ${$json.url}
🔧 Multi-Source Integration

Combine APIs:

Remotive
RapidAPI
Other job boards
🧠 Key Learnings
APIs often return nested arrays → must split
Loop node handles iteration
IF node applies filtering logic
Gmail node requires OAuth authentication
Expressions enable dynamic data usage
📦 Versioning Strategy
Version	Features
v1	Basic workflow
v2	AI filtering
v3	Fresher detection
v4	Multi-source APIs
v5	Production-ready system
🎯 Final Outcome

An automated system that:

Runs daily
Fetches AI jobs
Filters relevant roles
Sends email alerts
📌 Conclusion

This workflow demonstrates:

API integration
Data transformation
Conditional filtering
Automation scheduling
Email notification pipeline

It serves as a strong foundation for building scalable, production-grade AI job automation systems.