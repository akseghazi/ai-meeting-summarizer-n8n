# 🤖 AI Meeting Summarizer & Task Extractor

An AI-powered workflow automation system built with **n8n** that analyzes meeting transcripts, generates a concise meeting summary, extracts actionable tasks with assigned people and deadlines, stores the tasks in **Google Sheets**, and automatically sends a summary email through **Gmail**.

---

## 🚀 Project Overview

Meetings often contain important decisions, responsibilities, and deadlines that can easily be forgotten.

This project automates the process of converting an unstructured meeting transcript into structured and actionable information.

The workflow uses an AI Agent to:

* Generate a descriptive meeting title
* Summarize the meeting
* Extract actionable tasks
* Identify the person responsible for each task
* Identify deadlines
* Store tasks in Google Sheets
* Send an automated meeting summary via Gmail

The entire process is automated using **n8n**.

---

## 🏗️ Workflow Architecture

```text
                    ┌─────────────────────┐
                    │   Meeting Transcript│
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │       Webhook       │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │      AI Agent       │
                    │                     │
                    │  • Meeting Title    │
                    │  • Summary          │
                    │  • Action Items     │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Structured Output   │
                    │      Parser         │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │      Split Out      │
                    │                     │
                    │  Individual Tasks   │
                    └──────────┬──────────┘
                               │
                     ┌─────────┴─────────┐
                     │                   │
                     ▼                   ▼
            ┌─────────────────┐   ┌───────────────┐
            │  Google Sheets  │   │  Code Node    │
            │                 │   │               │
            │ Store Tasks     │   │ Combine Tasks │
            └─────────────────┘   └───────┬───────┘
                                          │
                                          ▼
                                   ┌───────────────┐
                                   │     Gmail     │
                                   │               │
                                   │ Email Summary │
                                   └───────────────┘
```

---

## 📸 Screenshots

### 1. Complete n8n Workflow

Shows the complete automation workflow from the meeting transcript webhook through AI processing, task extraction, Google Sheets, and Gmail.

![Complete n8n Workflow](screenshots/workflow.png)

---

### 2. AI Structured Output

The AI analyzes the meeting transcript and returns a structured meeting title, summary, and action items.

![AI Structured Output](screenshots/ai-output.png)

---

### 3. Google Sheets Task Management

Each extracted action item is automatically stored as a separate row in Google Sheets.

![Google Sheets Output](screenshots/google-sheets.png)

---

### 4. Automated Email Report

The final meeting summary and action items are automatically sent through Gmail.

![Email Output](screenshots/email-output.png)

---

## ✨ Features

### 🧠 AI Meeting Analysis

The AI analyzes an unstructured meeting transcript and generates:

* Meeting title
* Concise meeting summary
* Action items

### 📋 Automatic Task Extraction

The system identifies:

* **Person responsible**
* **Task**
* **Deadline**

If a deadline or responsible person isn't mentioned, the system returns:

```text
Not specified
```

### 📊 Google Sheets Integration

Every extracted action item is automatically added as a separate row in Google Sheets.

Example:

| Meeting                     | Person | Task                 | Deadline      |
| --------------------------- | ------ | -------------------- | ------------- |
| Application Launch Planning | John   | Prepare the database | Friday        |
| Application Launch Planning | Sarah  | Design the frontend  | Not specified |
| Application Launch Planning | Ahmed  | Test the application | Next Monday   |

### 📧 Automated Email Report

After processing the meeting, the workflow automatically sends an email containing:

* Meeting title
* Meeting summary
* Complete list of action items

Example subject:

```text
AI Meeting — Application Launch Planning
```

---

## 🔄 Workflow

### 1. Meeting Transcript

The system receives a meeting transcript through an **n8n Webhook**.

Example:

```text
John will prepare the database by Friday.
Sarah will design the frontend.
Ahmed will test the application next Monday.
The team also discussed launching the application
at the end of the month.
```

### 2. AI Agent

The AI Agent analyzes the transcript and identifies the key information.

### 3. Structured Output

The AI returns structured information:

```json
{
  "meeting_title": "Application Launch Planning",
  "summary": "The team discussed the application launch and assigned development and testing responsibilities.",
  "action_items": [
    {
      "person": "John",
      "task": "Prepare the database",
      "deadline": "Friday"
    },
    {
      "person": "Sarah",
      "task": "Design the frontend",
      "deadline": "Not specified"
    },
    {
      "person": "Ahmed",
      "task": "Test the application",
      "deadline": "Next Monday"
    }
  ]
}
```

### 4. Split Out

The action-item array is split into individual n8n items so each task can be processed separately.

### 5. Google Sheets

Each task is added as a separate row.

### 6. JavaScript Processing

An n8n Code node combines the extracted tasks into a clean format for the email notification.

### 7. Gmail

The final meeting report is automatically sent through Gmail.

---

## 🛠️ Tech Stack

| Technology        | Purpose                         |
| ----------------- | ------------------------------- |
| **n8n**           | Workflow automation             |
| **AI Agent**      | Meeting analysis                |
| **LLM**           | Summarization & task extraction |
| **JavaScript**    | Data transformation             |
| **Google Sheets** | Task storage                    |
| **Gmail**         | Email notifications             |
| **Webhook**       | Input/API endpoint              |

---

## 📦 n8n Nodes Used

1. **Webhook**
2. **AI Agent**
3. **OpenAI Chat Model**
4. **Structured Output Parser**
5. **Split Out**
6. **Google Sheets**
7. **Code**
8. **Gmail**

---

## ⚙️ Setup

### Prerequisites

* n8n instance
* LLM/API credentials
* Google account
* Google Sheets access
* Gmail access

### Google Sheets

Create a sheet with:

```text
Meeting | Person | Task | Deadline
```

### Webhook Input

Send a POST request containing:

```json
{
  "transcript": "John will prepare the database by Friday. Sarah will design the frontend. Ahmed will test the application next Monday."
}
```

---

## 📤 Example Output

### Google Sheets

| Meeting                     | Person | Task                 | Deadline      |
| --------------------------- | ------ | -------------------- | ------------- |
| Application Launch Planning | John   | Prepare the database | Friday        |
| Application Launch Planning | Sarah  | Design the frontend  | Not specified |
| Application Launch Planning | Ahmed  | Test the application | Next Monday   |

### Email

**Subject:**

```text
AI Meeting — Application Launch Planning
```

**Content:**

```text
MEETING: Application Launch Planning

MEETING SUMMARY

The team discussed the application launch and assigned
development and testing responsibilities.

ACTION ITEMS

1. John — Prepare the database — Deadline: Friday
2. Sarah — Design the frontend — Deadline: Not specified
3. Ahmed — Test the application — Deadline: Next Monday
```

---

## 🎯 Use Cases

* Software development teams
* Project management
* Remote teams
* Client meetings
* Business meetings
* Team stand-ups
* Sales meetings
* Product planning meetings

---

## 🔮 Future Improvements

* Automatic audio-to-transcript conversion
* Calendar integration
* Trello/Jira task creation
* Slack/Teams notifications
* Automatic participant identification
* Deadline reminders
* Database storage
* Meeting analytics dashboard
* Webhook authentication

---

## 📚 What I Learned

This project provided practical experience with:

* AI-powered workflow automation
* LLM integration
* Prompt engineering
* Structured JSON output
* n8n data transformation
* JavaScript in n8n
* Webhooks and API-based workflows
* Google Sheets integration
* Gmail automation
* Building practical AI automation systems

---

## 👨‍💻 Project Purpose

This project was built as part of my **AI Automation / AI Engineering portfolio** to demonstrate practical experience with:

**AI + LLMs + n8n + APIs + Automation + Data Processing**

---

## ⭐ Conclusion

The **AI Meeting Summarizer & Task Extractor** transforms unstructured meeting transcripts into structured, actionable information.

Instead of manually reading transcripts, identifying tasks, updating spreadsheets, and sending follow-up emails, the entire process is automated through a single n8n workflow.
