# Enterprise Knowledge & Support Copilot

A full-stack GenAI knowledge assistant that helps employees find answers from internal company documents such as SOPs, FAQs, product guides, and policies. It uses Retrieval-Augmented Generation (RAG) to retrieve relevant document context before generating answers.

> **Project status:** The repository contains the application code, frontend, API, database configuration, tests, and Docker setup. Functional full-stack GenAI application with document ingestion, RAG-based question answering, authentication, conversation history, and a Docker-based development setup.

## Features

- **Knowledge chat:** Ask questions about ingested company documents.
- **RAG pipeline:** Extracts, chunks, embeds, and retrieves relevant document passages before answer generation.
- **Source-aware answers:** Chat responses can include retrieved source information.
- **Document management:** Admins can upload and manage supported files (PDF, DOCX, TXT, and Markdown).
- **Authentication:** JWT-based sign-in and registration with role-aware access.
- **Conversation history:** Conversations are stored and scoped to the authenticated user.
- **Admin dashboard:** View document ingestion status and manage the knowledge base.
- **API:** FastAPI endpoints with interactive API documentation available in development.
- **Persistent vector search:** PostgreSQL with pgvector.
- **Container setup:** Docker Compose for the frontend, backend, and database.
- **Quality checks:** Pytest tests and a GitHub Actions CI workflow.

## Tech stack

| Area | Technologies |
|---|---|
| Frontend | Next.js, React, TypeScript, Tailwind CSS |
| Backend | Python, FastAPI, REST APIs |
| GenAI / RAG | LangChain/LangGraph flow, Google Gemini embeddings and generation |
| Database | PostgreSQL, pgvector, SQLAlchemy, Alembic |
| Security | JWT authentication, password hashing, role-based access |
| DevOps | Docker, Docker Compose, GitHub Actions |
| Testing | Pytest |

## How it works

1. An administrator uploads a policy, FAQ, or other knowledge document.
2. The backend extracts the text and splits it into overlapping chunks.
3. The embedding service converts chunks into vectors and stores them in PostgreSQL/pgvector.
4. A user asks a question in the web chat.
5. The retrieval layer finds relevant chunks, and the LLM generates a response grounded in that context.
6. The chat interface presents the answer and available source information.

## Architecture

```text
Next.js + React frontend
          |
          | REST API
          v
FastAPI backend ---- JWT authentication
          |
          +---- Document extraction / chunking
          |
          +---- RAG retrieval + Gemini
          |
          v
PostgreSQL + pgvector
          |
          +---- Documents, users, conversations, vectors
```

## Repository structure

```text
.
├── app/                  # FastAPI routes, services, models, RAG logic
├── alembic/              # Database migrations
├── documents/            # Sample company policies and FAQs
├── frontend/             # Next.js web application
├── scripts/              # Admin and sample-document utilities
├── tests/                # Unit and integration test foundation
├── deploy/               # Example reverse-proxy configuration
├── .github/workflows/    # GitHub Actions CI
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
├── pyproject.toml
└── .env.example
```

## Run locally with Docker

### Requirements

- Docker Desktop with Docker Compose
- A Google Gemini API key
- Git (optional, for cloning)

### 1. Clone your repository

Replace the URL with your own GitHub repository URL:

```bash
git clone https://github.com/YOUR-USERNAME/enterprise-knowledge-copilot.git
cd enterprise-knowledge-copilot
```

### 2. Configure environment variables

**Windows PowerShell:**

```powershell
Copy-Item .env.example .env
```

Open `.env` and set a strong unique `POSTGRES_PASSWORD`, a random `JWT_SECRET_KEY`, and your Gemini API key in `EMBEDDING_API_KEY`.

Generate a JWT secret:

```powershell
python -c "import secrets; print(secrets.token_urlsafe(48))"
```

Do not commit `.env` or any real credentials. `.env.example` is only a template.

### 3. Start the application

```powershell
docker compose up --build -d
docker compose ps
```

Open:
- Frontend: http://localhost:3000
- Backend health check: http://localhost:8000/health
- FastAPI Swagger UI: http://localhost:8000/docs (when enabled in `.env`)

To inspect logs:

```powershell
docker compose logs --tail=100 backend
docker compose logs --tail=100 frontend
docker compose logs --tail=100 postgres
```

To stop the stack:

```powershell
docker compose down
```

### Run without Docker

For the backend, use Python 3.12+ and follow the environment, dependency, migration, and Uvicorn instructions in the repository's deployment notes. For the frontend, use Node.js 20+:

```powershell
cd frontend
Copy-Item .env.example .env.local
npm install
npm run dev
```

## Tests and CI

Run backend tests inside an environment with dependencies installed:

```bash
pytest
```

The repository includes a GitHub Actions workflow. Tests and provider/database-dependent features should be verified in your own environment before release.

## Security notes

- Never commit `.env`, API keys, passwords, JWT secrets, or access tokens.
- Keep PostgreSQL private in production.
- Use HTTPS and a secret manager for deployment.
- The included Docker configuration is a starting point; production deployments should add monitoring, rate limiting, durable background jobs, and cloud/object storage as appropriate.

## Current limitations / next steps

- Run the full test suite and end-to-end checks locally with Docker, PostgreSQL/pgvector, and valid Gemini credentials.
- Verify document upload → ingestion → retrieval → answer → source citation end to end.
- Add automated integration tests for the database and external AI provider.
- For cloud deployment, configure persistent/object storage and secrets rather than relying on local container storage.
- Capture real screenshots from the running application and add them to the README.

## Author

**Shruti Mawalkar**

Built as a portfolio project exploring full-stack development, GenAI, RAG, APIs, authentication, databases, and containerized workflows.

## Project overview visual

The image below is a **concept/portfolio graphic**, not a screenshot captured from a running instance of the application.

![Project overview concept](docs/project-overview-concept.png)
