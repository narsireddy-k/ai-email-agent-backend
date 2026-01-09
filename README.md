# 🚀 AI Email Agent

**Automated Gmail Classifier & Auto-Reply System**

An intelligent, production-grade email automation backend that connects to **Gmail**, understands incoming emails using **AI**, and generates professional responses automatically.

Built using **Django**, **Django REST Framework**, **LangGraph**, **OpenAI**, **Gmail API**, **Celery**, and **Redis**, this system is designed to scale and integrate seamlessly with a **Next.js dashboard**.

---

## ✨ What This System Does

The AI Email Agent automatically:

- ✔️ Syncs emails from Gmail using OAuth
- ✔️ Classifies email intent using AI
- ✔️ Generates context-aware professional replies
- ✔️ Saves replies as **Gmail Drafts** (not auto-sent)
- ✔️ Stores all email metadata in the database
- ✔️ Exposes REST APIs for frontend dashboards

---

## 🧠 Core Capabilities

### 1️⃣ Gmail OAuth Authentication

- Secure Google OAuth 2.0 login
- User-granted Gmail permissions
- Token refresh & secure storage

---

### 2️⃣ Gmail Email Sync (Background)

- Periodic Gmail inbox sync using **Celery**
- Emails stored with metadata:
  - Subject
  - Sender
  - Body (cleaned)
  - Thread ID
  - Labels

---

### 3️⃣ AI Intent Classification

Each email is classified into one of the following intents:

- `meeting`
- `billing`
- `complaint`
- `follow-up`
- `inquiry`
- `marketing`
- `personal`
- `spam`
- `task`

This classification drives the reply strategy.

---

### 4️⃣ AI Auto-Reply Generation (LangGraph)

A **LangGraph-based agent pipeline** handles reply creation:

**Pipeline Steps:**

1. Email cleaning & normalization
2. Intent understanding
3. Context-aware GPT reply generation
4. HTML email formatting
5. Gmail Draft creation via API
6. Save reply + metadata in database

No emails are auto-sent — drafts require human approval.

---

### 5️⃣ REST API (Frontend Ready)

The backend exposes clean APIs consumable by a **Next.js dashboard**.

---

## 📂 Project Structure

```
backend/
│── accounts/                # Gmail OAuth & token handling
│── emails/                  # Core email logic
│   ├── langgraph/
│   │   ├── nodes/            # Individual agent nodes
│   │   ├── reply_graph.py    # Reply-only agent
│   │   └── full_agent_graph.py # Full inbox agent
│   ├── services.py           # Gmail API wrappers
│   ├── tasks.py              # Celery background tasks
│
│── backend/
│   ├── settings.py
│   ├── celery.py             # Celery configuration
│
│── manage.py
│── requirements.txt
│── .env                      # NOT committed
```

---

## 🛠 Tech Stack

### Backend

- **Django**
- **Django REST Framework**
- **LangGraph** (Agent orchestration)
- **OpenAI API** (LLM reasoning)
- **Gmail API**
- **Celery** (async processing)
- **Redis** (broker)
- **SQLite / PostgreSQL**

### Frontend (Not Included)

- Next.js
- TailwindCSS

---

## ⚙️ Installation & Local Setup

### 1️⃣ Clone the Repository

```bash
git clone https://github.com/narsi-2208/ai-email-agent-backend.git
cd ai-email-agent-backend/backend
```

---

### 2️⃣ Create Python Environment

```bash
conda create -n aiagent python=3.10
conda activate aiagent
```

---

### 3️⃣ Install Dependencies

```bash
pip install -r requirements.txt
```

---

### 4️⃣ Create `.env` File

```env
GOOGLE_CLIENT_ID=xxxx
GOOGLE_CLIENT_SECRET=xxxx
OPENAI_API_KEY=sk-xxxxx
```

⚠️ **Never commit `.env`**

---

### 5️⃣ Run Database Migrations

```bash
python manage.py migrate
```

---

### 6️⃣ Start Redis

```bash
redis-server
```

---

### 7️⃣ Start Celery Worker

```bash
celery -A backend worker -l info -P solo
```

---

### 8️⃣ Start Celery Beat (Scheduler)

```bash
celery -A backend beat -l info
```

---

### 9️⃣ Start Django Server

```bash
python manage.py runserver
```

Backend runs at:
```
http://127.0.0.1:8000
```

---

## ⚡ How the AI Agent Works (Flow)

```
Gmail Sync
   ↓
Intent Classification
   ↓
AI Reply Generation
   ↓
HTML Formatting
   ↓
Create Gmail Draft
   ↓
Save Metadata in DB
```

---

## 📌 API Endpoints

### 🔹 Email List
```
GET /emails/list/
```

### 🔹 Email Detail
```
GET /emails/detail/<id>/
```

### 🔹 Run Reply Agent (Single Email)
```
POST /emails/agent/reply/<id>/
```

### 🔹 Run Full Inbox Agent
```
POST /emails/agent/full/
```

### 🔹 Sync Emails from Gmail
```
GET /emails/sync/
```

---

## 🔐 Security & Best Practices

Do **NOT** commit:

- `.env`
- `db.sqlite3`
- `celerybeat-schedule*`
- `__pycache__/`
- Virtual environments
- OAuth tokens
- API keys

Your `.gitignore` already covers these.

---

## 🚀 Production Recommendations

- PostgreSQL instead of SQLite
- Redis as managed service
- OAuth scopes restricted to Gmail
- HTTPS + secure cookies
- Gunicorn + Nginx

---

## 📜 License

This project is proprietary and intended for internal or client use.

---

## 🤝 Maintained By

**ForgeByte AI**
