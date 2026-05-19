# LegalEase Pakistan

**Bridging the Gap Between Constitutional Rights and Citizen Awareness Through Intelligent Legal Technology**

LegalEase Pakistan is a full-stack AI-powered legal assistance platform that makes Pakistani constitutional law accessible to every citizen — regardless of income, education level, or location. It combines an AI legal chatbot, document analysis, lawyer discovery, and a legal education portal into a single web application.

---

## Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Local Setup](#local-setup)
- [Environment Variables](#environment-variables)
- [Running the App](#running-the-app)
- [Deployment](#deployment)
- [API Reference](#api-reference)
- [Database Overview](#database-overview)
- [Team](#team)

---

## Features

### AI Legal Chatbot
- Ask legal questions in **English, Urdu, or Roman Urdu**
- Answers backed by Pakistan's Constitution and legal code via **RAG (Retrieval-Augmented Generation)**
- Hybrid search: ChromaDB vector search + BM25 keyword search + CrossEncoder reranking
- Built-in guardrails: blocks dangerous requests, detects prompt injection, redacts PII from logs
- Cites sources with links to pakistancode.gov.pk

### Document Upload & Analysis
- Upload **PDFs, DOCX, TXT, or images** directly in chat
- OCR processing for scanned documents (pytesseract)
- Automatic document classification and plain-language explanation
- Suggests next steps based on document content

### Legal Contract Analyzer
- Automated risk assessment for contracts, tenders, and agreements
- Identifies missing clauses and compliance issues under Pakistani law
- Designed for organizations and legal professionals

### Lawyer Connect Portal
- Directory of verified legal professionals across Pakistan
- Filter by specialization, location, language, rating, and availability
- View detailed profiles and contact information

### Know Your Law — Education Portal
- Structured courses on Pakistani constitutional and civil law
- Categories: Family Law, Banking Law, Property Law, Police Law, Religious Law
- Rich lessons with key points, real-world examples, and quizzes
- Progress tracking and completion certificates

### Accessibility
- Multilingual UI (English / Urdu)
- Voice reply support
- Offline access for cached statutes and FAQs
- Mobile-responsive design

---

## Tech Stack

### Frontend
| Technology | Version | Purpose |
|-----------|---------|---------|
| React | 19.2.0 | UI framework |
| Tailwind CSS | 4.1.9 | Styling |
| Radix UI | Latest | Accessible UI components |
| React Hook Form | 7.60.0 | Form handling |
| React Markdown | 10.1.0 | Render AI responses |
| Zod | 3.25.76 | Schema validation |

### Backend
| Technology | Version | Purpose |
|-----------|---------|---------|
| FastAPI | 0.115.0+ | Web framework |
| Uvicorn | 0.30.0+ | ASGI server |
| ChromaDB | 0.5.0+ | Vector database for legal embeddings |
| Groq API (LLaMA) | Latest | LLM inference engine |
| SentenceTransformers | Latest | Text embeddings (all-MiniLM-L6-v2) |
| rank-bm25 | 0.2.2 | BM25 keyword search |
| PyPDF2 | 3.0.0 | PDF text extraction |
| python-docx | 1.1.0 | Word document extraction |
| pytesseract + Pillow | Latest | OCR for scanned documents |
| slowapi | 0.1.9 | Rate limiting |
| pydantic | 2.0.0+ | Request validation |

### Infrastructure
| Tool | Purpose |
|------|---------|
| Docker | Multi-stage build (Node + Python) |
| Railway | Primary deployment platform |
| Vercel | Alternative frontend deployment |

---

## Project Structure

```
LEGAL5.0/
├── App.py                    # Main FastAPI backend (2,200+ lines)
├── lawyers_router.py         # Lawyer directory API router
├── education_router.py       # Legal education API router
│
├── src/                      # React frontend source
│   ├── App.js                # All pages and components (2,800+ lines)
│   ├── App.css               # Global styles
│   ├── index.js              # React entry point
│   └── index.css             # Base styles
│
├── public/                   # Static assets
├── build/                    # Production React build (generated)
│
├── chroma_db/                # Pre-indexed vector database
├── chroma_db_old/            # Legacy vector index (backup)
│
├── family_laws.json          # Family law acts and sections
├── Banking_Laws.json         # Banking and financial regulations
├── Land-Property_Law.json    # Property and land acquisition laws
├── Police_Laws.json          # Police Act, CrPC sections
├── Religious_Laws.json       # Islamic and religious laws
├── Essential_laws.json       # Core constitutional laws
│
├── lawyers_db.json           # Lawyer profiles database
├── lawyer_reviews_db.json    # Lawyer reviews and ratings
│
├── edu_courses_db.json       # Educational courses
├── edu_assessments_db.json   # Quiz questions
├── edu_library_db.json       # Recommended legal resources
├── edu_progress_db.json      # User progress tracking
│
├── requirements.txt          # Python dependencies
├── package.json              # Node dependencies
├── Dockerfile                # Multi-stage Docker build
├── Procfile                  # Railway/Heroku process config
├── railway.toml              # Railway deployment config
├── vercel.json               # Vercel deployment config
└── .env.example              # Environment variable template
```

---

## Prerequisites

Make sure you have the following installed:

- **Python** 3.11+
- **Node.js** 18+ and npm
- **Tesseract OCR** (for document image processing)
- A free **Groq API key** — get one at [console.groq.com](https://console.groq.com)

### Install Tesseract OCR

**Windows:**
Download and install from [github.com/UB-Mannheim/tesseract/wiki](https://github.com/UB-Mannheim/tesseract/wiki), then add it to your PATH.

**macOS:**
```bash
brew install tesseract
```

**Ubuntu/Debian:**
```bash
sudo apt install tesseract-ocr
```

---

## Local Setup

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/legalease-pakistan.git
cd legalease-pakistan
```

### 2. Set Up Environment Variables

```bash
cp .env.example .env
```

Open `.env` and fill in your values:

```env
GROQ_API_KEY=your_groq_api_key_here
```

### 3. Install Python Dependencies

```bash
pip install -r requirements.txt
```

### 4. Install Node Dependencies

```bash
npm install
```

---

## Environment Variables

| Variable | Required | Description |
|----------|----------|-------------|
| `GROQ_API_KEY` | Yes | Groq API key for LLM inference. Get it free at console.groq.com |
| `REACT_APP_API_URL` | No | Backend API URL for frontend. Defaults to `http://localhost:8000` |
| `PORT` | No | Port for the backend server. Defaults to `8000` |

---

## Running the App

### Option 1 — Run Backend and Frontend Separately (Recommended for Development)

**Start the backend:**
```bash
uvicorn App:app --reload --port 8000
```

**In a separate terminal, start the frontend:**
```bash
npm start
```

The frontend dev server runs on `http://localhost:3000` and proxies API requests to `http://localhost:8000`.

---

### Option 2 — Run as a Single Server (Production Mode)

First build the React frontend:
```bash
npm run build
```

Then start the backend (which serves the built frontend):
```bash
uvicorn App:app --host 0.0.0.0 --port 8000
```

Open `http://localhost:8000` in your browser.

---

### Option 3 — Run with Docker

```bash
docker build -t legalease-pakistan .
docker run -p 8000:8000 --env-file .env legalease-pakistan
```

Open `http://localhost:8000` in your browser.

---

## Deployment

### Deploy to Railway

1. Push your code to GitHub
2. Go to [railway.app](https://railway.app) and create a new project from your GitHub repo
3. Add environment variable: `GROQ_API_KEY`
4. Railway uses the `Dockerfile` automatically — no additional config needed

### Deploy to Vercel (Frontend Only)

1. Push your code to GitHub
2. Import the project on [vercel.com](https://vercel.com)
3. Vercel will detect `vercel.json` automatically
4. Set `REACT_APP_API_URL` to your deployed backend URL
5. Build command: `npm run build` | Output directory: `build`

---

## API Reference

### Authentication

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/auth/register` | Register a new user |
| POST | `/auth/login` | Login and receive session token |
| POST | `/auth/logout` | Logout and clear session |
| GET | `/auth/session` | Validate current session |

**Register / Login Request Body:**
```json
{
  "email": "user@example.com",
  "password": "yourpassword",
  "name": "Your Name"
}
```

---

### Chat

| Method | Endpoint | Description | Rate Limit |
|--------|----------|-------------|------------|
| POST | `/chat` | Send a message to the AI legal chatbot | 10 req/min per IP |

**Request Body:**
```json
{
  "message": "What are my rights if I receive an FIR?",
  "conversation_history": [],
  "document_context": ""
}
```

**Response:**
```json
{
  "response": "Under Section 154 of the CrPC...",
  "sources": ["The Code of Criminal Procedure, 1898 - pakistancode.gov.pk"],
  "category": "Criminal Law"
}
```

---

### Document Analysis

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/extract-document` | Upload and extract text from PDF, DOCX, TXT, or image |
| POST | `/analyze-contract` | Analyze a contract for risks and missing clauses |

**Extract Document:** Send as `multipart/form-data` with a `file` field.

---

### Lawyers

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/lawyers` | Get all lawyers (supports filters) |
| GET | `/api/lawyers/{id}` | Get a specific lawyer profile |
| POST | `/api/lawyers/{id}/reviews` | Submit a review for a lawyer |

**Query Parameters for GET `/api/lawyers`:**
- `specialization` — e.g. `Family Law`, `Criminal Law`
- `city` — e.g. `Lahore`, `Karachi`
- `language` — e.g. `Urdu`, `English`
- `min_rating` — e.g. `4.0`

---

### Education

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/education/courses` | Get all available courses |
| GET | `/api/education/courses/{id}/lessons` | Get lessons for a course |
| GET | `/api/education/assessments/{course_id}` | Get quiz questions |
| POST | `/api/education/progress` | Save user progress |

---

### System

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/health` | Health check |
| GET | `/collections` | ChromaDB collection metadata |

---

## Database Overview

### Vector Database (ChromaDB)
Stored in `chroma_db/`. Pre-indexed embeddings using `all-MiniLM-L6-v2` (384-dimensional vectors). Collections cover:
- Family Laws
- Banking & Financial Laws
- Land & Property Laws
- Police & Criminal Laws
- Religious & Islamic Laws
- Constitutional & Essential Laws

To rebuild the index from JSON law files:
```bash
python load_all_chroma.py
```

### JSON Databases
All data is stored as local JSON files for simplicity. Key files:

| File | Contents |
|------|----------|
| `lawyers_db.json` | Lawyer profiles with specialization, location, rating, contact |
| `edu_courses_db.json` | Courses with lessons, key points, real-world examples |
| `edu_assessments_db.json` | Quiz questions per course |
| `edu_progress_db.json` | Per-user course completion tracking |

To seed the lawyers database with default entries:
```bash
python seed_lawyers.py
```

---

## Team

| Name | Role | ID |
|------|------|----|
| Jawad Ahmed | Data & Backend | 22I1852 |
| Hassan Rizwan | AI, Database & Backend | 22I1926 |
| Muhammad Salman | Frontend & Mobile | 22I1885 |

**Supervisor:** Dr. Muhammad Asif Naeem
**Co-Supervisor:** Muhammad Aamir Gulzar

---

## License

This project was developed as a Final Year Project at FAST-NUCES. All rights reserved.
