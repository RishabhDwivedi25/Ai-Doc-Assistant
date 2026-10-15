# 🤖 AI Document Assistant

[![Next.js](https://img.shields.io/badge/Next.js-16.2-black?style=for-the-badge&logo=next.js)](https://nextjs.org/)
[![React](https://img.shields.io/badge/React-19.2-61DAFB?style=for-the-badge&logo=react)](https://react.dev/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.100+-009688?style=for-the-badge&logo=fastapi)](https://fastapi.tiangolo.com/)
[![Express.js](https://img.shields.io/badge/Express-5.2-lightgrey?style=for-the-badge&logo=express)](https://expressjs.com/)
[![Prisma](https://img.shields.io/badge/Prisma-6.19-2D3748?style=for-the-badge&logo=prisma)](https://www.prisma.io/)
[![Qdrant](https://img.shields.io/badge/Qdrant-Vector_DB-red?style=for-the-badge&logo=qdrant)](https://qdrant.tech/)
[![Groq](https://img.shields.io/badge/Groq-LPU_Inference-f55036?style=for-the-badge)](https://groq.com/)
[![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?style=for-the-badge&logo=docker)](https://www.docker.com/)

An intelligent, multi-tenant **Retrieval-Augmented Generation (RAG)** platform for conversing with PDF documents in real-time. Built with a decoupled microservice architecture featuring **Next.js 16**, **Express.js API Gateway**, and **FastAPI RAG Engine** powered by **Qdrant Vector Database**, **Groq Cloud LLMs**, and **Tavily Web Search**.

---

## 📑 Table of Contents

- [Overview](#-overview)
- [Key Features](#-key-features)
- [Architecture](#-architecture)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
  - [Prerequisites](#prerequisites)
  - [Environment Configuration](#environment-configuration)
  - [Option A: One-Command Run with Docker Compose](#option-a-one-command-run-with-docker-compose-recommended)
  - [Option B: Manual Local Development](#option-b-manual-local-development)
- [Database Migrations](#-database-migrations)
- [API Reference](#-api-reference)
- [Production Deployment](#-production-deployment)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🌟 Overview

**AI Document Assistant** transforms dense PDF documents into actionable insights. Users can upload research papers, business reports, legal contracts, or manuals and instantly ask nuanced questions. 

Unlike conventional chatbots, answers are ground-truth verified with exact page numbers and context source citations. When questions require real-time information or go beyond the uploaded document context, the assistant automatically falls back to live web search via Tavily.

---

## 🚀 Key Features

- 📄 **Multi-PDF Parsing & Chunking**: Automatic page-wise text extraction and semantic chunking with metadata tagging.
- ⚡ **High-Speed Vector Search**: High-dimensional semantic embeddings stored and queried in **Qdrant Vector Database**.
- 🧠 **Lightning-Fast LLM Reasoning**: Sub-second answers powered by **Groq Cloud LPUs** (Llama 3 / Mixtral).
- 🏷️ **Citations & Source Pill Navigation**: Every generated response includes reference links back to specific document pages and chunks.
- 🌐 **Automated Web Search Fallback**: Seamless fallback to **Tavily Web Search** when documents do not contain the answer or when explicitly requested.
- 👥 **Multi-Tenant User Isolation**: Safe document separation per user ID with secure database relations.
- 🔐 **Robust Authentication**:
  - Email & Password with OTP verification (via Brevo API & Nodemailer)
  - Google OAuth 2.0 social sign-in via Passport.js
  - Session cookies and secure JWT authorization
- 🖥️ **Interactive Modern UI**:
  - Split-pane workspace with document viewer and real-time chat
  - Document Hub for browsing, previewing, and deleting uploaded files
  - Dark mode and light mode theming
- 🐳 **Production-Ready Microservices**: Fully containerized with Docker Compose, Vercel frontend config, and Render Blueprint orchestration.

---

## 🏗 Architecture

The system is split into three modular tiers:

```
┌─────────────────────────────────────────────────────────────┐
│                      Client Browser                         │
│       Next.js 16 (React 19, TypeScript, Tailwind CSS)       │
└──────────────────────────────┬──────────────────────────────┘
                               │
               HTTP Requests   ▼   CORS / Auth
┌─────────────────────────────────────────────────────────────┐
│                   Backend API Gateway                       │
│      Node.js / Express 5 • Prisma ORM • Passport / JWT      │
└──────────────┬───────────────────────────────┬──────────────┘
               │                               │
               ▼                               ▼
    ┌──────────────────────┐       ┌──────────────────────────┐
    │  PostgreSQL Database │       │    FastAPI RAG Engine    │
    │  (Neon / RDS / Local)│       │  PyTorch & Embeddings    │
    └──────────────────────┘       └─────────────┬────────────┘
                                                 │
                                 ┌───────────────┴───────────────┐
                                 ▼                               ▼
                      ┌────────────────────┐          ┌────────────────────┐
                      │ Qdrant Vector DB   │          │  Groq Cloud LLM    │
                      │  Dense Embeddings  │          │  & Tavily Search   │
                      └────────────────────┘          └────────────────────┘
```

---

## 🛠 Tech Stack

| Layer | Technologies |
|---|---|
| **Frontend** | [Next.js 16](https://nextjs.org/) (App Router), [React 19](https://react.dev/), [TypeScript](https://www.typescriptlang.org/), [Tailwind CSS v4](https://tailwindcss.com/) |
| **API Gateway** | [Node.js](https://nodejs.org/), [Express.js 5](https://expressjs.com/), [Prisma ORM](https://www.prisma.io/), [Passport.js](https://www.passportjs.org/), [JWT](https://jwt.io/) |
| **RAG Microservice** | [Python 3.11+](https://www.python.org/), [FastAPI](https://fastapi.tiangolo.com/), [Uvicorn](https://www.uvicorn.org/), [PyMuPDF](https://pymupdf.readthedocs.io/), [Sentence-Transformers](https://www.sbert.net/) |
| **Vector DB** | [Qdrant](https://qdrant.tech/) (Vector similarity search with payload filtering) |
| **Relational DB** | [PostgreSQL](https://www.postgresql.org/) (Managed via [Neon](https://neon.tech/) or local Docker) |
| **LLM & Search** | [Groq Cloud](https://groq.com/) (Llama 3 / Mixtral inference), [Tavily Search API](https://tavily.com/) |
| **Containerization**| [Docker](https://www.docker.com/), [Docker Compose](https://docs.docker.com/compose/) |
| **Deployment** | [Vercel](https://vercel.com/) (Frontend), [Render](https://render.com/) (Backend & RAG Engine via Blueprint) |

---

## 📂 Project Structure

```bash
ai-doc-assistant/
├── backend/                  # Node.js / Express API Gateway
│   ├── prisma/               # Prisma schema & migrations
│   ├── src/
│   │   ├── config/           # Database & Passport OAuth setup
│   │   ├── controllers/      # Auth & User business logic
│   │   ├── middleware/       # JWT & session verification
│   │   ├── routes/           # Auth, User, and RAG proxy routes
│   │   └── services/         # OTP, Email, and Token services
│   ├── Dockerfile
│   └── package.json
│
├── frontend/                 # Next.js 16 Client Interface
│   ├── src/
│   │   ├── app/              # App Router pages (login, auth callback, workspace)
│   │   ├── components/       # DocumentHub, Workspace, ChatInput, Header, Sidebar
│   │   ├── context/          # Auth context provider
│   │   └── lib/              # Client API helpers
│   ├── Dockerfile
│   └── package.json
│
├── rag-service/              # Python FastAPI RAG Service
│   ├── agent/                # ReAct agent, tools, and prompts
│   │   └── tools.py          # Tavily web search integration
│   ├── database/             # Qdrant client & vector operations
│   ├── services/             # Embeddings, Groq LLM, PDF text extraction
│   ├── utils/                # Text chunker with metadata preservation
│   ├── Dockerfile
│   ├── main.py               # FastAPI entry point & endpoints
│   └── requirements.txt
│
├── docker-compose.yml        # Orchestration for local full-stack execution
├── render.yaml               # Render Blueprint deployment specification
├── deployment.md             # In-depth production deployment guide
├── vercel_render_deploy.md   # Step-by-step Vercel + Render guide
├── .env.example              # Sample environment variables
└── package.json              # Root package scripts
```

---

## 🏁 Getting Started

### Prerequisites

- [Docker & Docker Compose](https://www.docker.com/) (Recommended for fastest setup)
- Or for local manual setup:
  - [Node.js](https://nodejs.org/) (v18 or v20+) & `npm`
  - [Python](https://www.python.org/) 3.11+
  - [PostgreSQL](https://www.postgresql.org/) database
  - Active API keys for **Groq Cloud** and **Tavily Search**

---

### Environment Configuration

Create a `.env` file in the root directory by copying `.env.example`:

```bash
cp .env.example .env
```

Populate the required credentials:

```ini
# PostgreSQL connection string (e.g., Neon or local postgres)
DATABASE_URL="postgresql://user:password@localhost:5432/ai_doc_db?sslmode=prefer"

# AI & Search APIs
GROQ_API_KEY="gsk_your_groq_api_key_here"
TAVILY_API_KEY="tvly-your_tavily_api_key_here"

# Service URLs
FRONTEND_URL="http://localhost:3000"
NEXT_PUBLIC_API_URL="http://localhost:5000"
NEXT_PUBLIC_RAG_URL="http://localhost:8000"

# Secrets
SESSION_SECRET="your-super-secret-session-key"
JWT_SECRET="your-super-secret-jwt-key"

# Optional: Google OAuth
GOOGLE_CLIENT_ID=""
GOOGLE_CLIENT_SECRET=""
GOOGLE_CALLBACK_URL="http://localhost:5000/api/auth/google/callback"

# Optional: Email Service (Brevo API or SMTP)
BREVO_API_KEY=""
SENDER_EMAIL=""
SMTP_USER=""
SMTP_PASS=""
```

---

### Option A: One-Command Run with Docker Compose (Recommended)

Start the frontend, backend, and RAG service together with persistent storage:

```bash
docker compose up --build
```

- **Frontend**: [http://localhost:3000](http://localhost:3000)
- **Backend API**: [http://localhost:5000](http://localhost:5000)
- **RAG Service & Swagger Docs**: [http://localhost:8000/docs](http://localhost:8000/docs)

To stop services:
```bash
docker compose down
```

---

### Option B: Manual Local Development

If you prefer running services independently:

#### 1. Start RAG Service (Python)
```bash
cd rag-service
python -m venv .venv

# On Linux/macOS:
source .venv/bin/activate
# On Windows:
.venv\Scripts\activate

pip install -r requirements.txt
uvicorn main:app --reload --port 8000
```

#### 2. Start Backend API Gateway (Node.js)
```bash
cd backend
npm install
npx prisma generate
npx prisma migrate dev
npm run dev
```

#### 3. Start Frontend Client (Next.js)
```bash
cd frontend
npm install
npm run dev
```

Visit [http://localhost:3000](http://localhost:3000) to open the application.

---

## 🗄 Database Migrations

The backend uses **Prisma ORM** for type-safe PostgreSQL interactions.

```bash
cd backend

# Generate Prisma Client
npx prisma generate

# Create and apply migrations in development
npx prisma migrate dev --name init

# Apply migrations in production
npx prisma migrate deploy

# Open Prisma Studio GUI
npx prisma studio
```

---

## 📡 API Reference

### 🔐 Authentication (`/api/auth`)
| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/auth/send-otp` | Sends an email verification OTP for login/signup |
| `POST` | `/api/auth/verify-otp` | Verifies OTP and generates auth tokens / sessions |
| `POST` | `/api/auth/login` | Email and password authentication |
| `GET` | `/api/auth/google` | Initiates Google OAuth 2.0 flow |
| `GET` | `/api/auth/google/callback` | Google OAuth callback URL |
| `POST` | `/api/auth/logout` | Clears user session and cookies |

### 🤖 RAG Service (`/` or proxied through `/api/rag`)
| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/documents` | Returns all indexed documents for the authenticated user |
| `POST` | `/upload` | Uploads and processes a PDF file (chunking, embedding, vector indexing) |
| `POST` | `/search` | Queries Qdrant vectors and generates an LLM response with citations |
| `GET` | `/documents/{filename}` | Retrieves all extracted pages and chunks for a specific document |
| `DELETE`| `/documents/{filename}` | Removes document vectors and payloads from Qdrant |

---

## 🚀 Production Deployment

Comprehensive guides are available in the repository:

- 📖 **Full Cloud Deployment (Vercel + Render + Neon)**: See [vercel_render_deploy.md](file:///c:/projects/ai-doc-assistant/vercel_render_deploy.md)
- 🐳 **VPS / Docker Deployment Guide**: See [deployment.md](file:///c:/projects/ai-doc-assistant/deployment.md)
- ⚙️ **Render Blueprint Configuration**: See [render.yaml](file:///c:/projects/ai-doc-assistant/render.yaml)

### Quick Summary

1. **Database**: Create a serverless PostgreSQL instance on [Neon.tech](https://neon.tech) and set `DATABASE_URL`.
2. **Backend & RAG Engine**: Connect the repo to [Render](https://render.com) using the blueprint [`render.yaml`](file:///c:/projects/ai-doc-assistant/render.yaml) which deploys both web services.
3. **Frontend**: Import the `/frontend` directory to [Vercel](https://vercel.com) and configure `NEXT_PUBLIC_API_URL` and `NEXT_PUBLIC_RAG_URL`.

---

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!

1. Fork the repository.
2. Create your feature branch (`git checkout -b feature/amazing-feature`).
3. Commit your changes (`git commit -m 'feat: add amazing feature'`).
4. Push to the branch (`git push origin feature/amazing-feature`).
5. Open a Pull Request.

---

## 📄 License

This project is licensed under the [ISC License](LICENSE).
