# Enterprise Knowledge & Support Copilot

A full-stack GenAI knowledge assistant that helps employees find answers from internal company documents such as SOPs, FAQs, product guides, and policies. It uses Retrieval-Augmented Generation (RAG) to retrieve relevant document context before generating answers.

**Project Status:** Functional full-stack GenAI application featuring document ingestion, RAG-based question answering, authentication, conversation history, an admin dashboard, and a Docker-based development environment.

## Features

- **Knowledge Chat:** Ask questions about internal company documents.
- **RAG Pipeline:** Extracts, chunks, embeds, and retrieves relevant document passages before generating answers.
- **Source-Aware Answers:** Provides responses grounded in retrieved document context, with available source information.
- **Document Management:** Supports uploading and managing PDF, DOCX, TXT, and Markdown files.
- **Authentication:** JWT-based authentication, password hashing, and role-aware access control.
- **Conversation History:** Stores conversations and scopes them to authenticated users.
- **Admin Dashboard:** View document ingestion status and manage the knowledge base.
- **REST API:** FastAPI endpoints with interactive Swagger documentation.
- **Vector Search:** PostgreSQL with pgvector for persistent vector storage and similarity search.
- **Containerization:** Docker Compose configuration for the frontend, backend, and database.
- **Testing and CI:** Pytest test suite and GitHub Actions workflow.

## Tech Stack

| Area | Technologies |
|---|---|
| Programming Languages | Python, TypeScript, SQL |
| Frontend | Next.js, React, TypeScript, Tailwind CSS |
| Backend | FastAPI, Python, REST APIs, Pydantic |
| GenAI / RAG | Google Gemini, LangChain, LangGraph, RAG |
| Embeddings and Retrieval | Text embeddings, vector similarity search |
| Database | PostgreSQL, pgvector, SQLAlchemy |
| Database Migrations | Alembic |
| Security | JWT authentication, password hashing, role-based access control |
| Document Processing | PDF, DOCX, TXT, Markdown extraction and chunking |
| DevOps | Docker, Docker Compose, GitHub Actions |
| Testing | Pytest |
| API Documentation | Swagger UI, OpenAPI |
| Version Control | Git, GitHub |

## How It Works

1. An administrator uploads a policy, FAQ, product guide, or other knowledge document.
2. The backend extracts the document text and splits it into overlapping chunks.
3. The embedding service converts text chunks into vectors and stores them in PostgreSQL with pgvector.
4. A user submits a question through the web chat interface.
5. The retrieval layer searches for relevant document passages.
6. Google Gemini generates a response using the retrieved context.
7. The frontend displays the answer and available source information.

## Architecture

```text
                Next.js + React Frontend
                          |
                          | REST API
                          v
                    FastAPI Backend
                          |
             +------------+------------+
             |            |            |
             v            v            v
        JWT Auth     Document       RAG Pipeline
                     Processing          |
             |       Extraction          |
             |       Chunking            v
             |       Embeddings     Google Gemini
             |            |               |
             +------------+---------------+
                          |
                          v
                 PostgreSQL + pgvector
                          |
             +------------+------------+
             |            |            |
          Documents     Users      Conversations
                          |
                       Vectors
```

## Repository Structure

```text
Enterprise-RAG-Copilot/
├── app/                  # Backend routes, services, models, and RAG logic
├── alembic/              # Database migrations
├── documents/            # Sample policies and FAQs
├── frontend/             # Next.js web application
├── scripts/              # Admin and document utilities
├── tests/                # Automated tests
├── deploy/               # Deployment configuration
├── .github/workflows/    # GitHub Actions CI
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
├── pyproject.toml
├── alembic.ini
├── .env.example
└── README.md
```

## Getting Started

### Prerequisites

- Docker Desktop with Docker Compose
- Google Gemini API key
- Git
- Node.js 20+ if running the frontend separately
- Python 3.12+ if running the backend separately

### 1. Clone the Repository

```bash
git clone https://github.com/ShrutiMawalkar/Enterprise-RAG-Copilot.git
cd Enterprise-RAG-Copilot
```

### 2. Configure Environment Variables

On Windows PowerShell, run:

```powershell
Copy-Item .env.example .env
```

Open `.env` and configure the required environment variables, including:

- `POSTGRES_PASSWORD` — a strong, unique database password
- `JWT_SECRET_KEY` — a securely generated random secret
- `EMBEDDING_API_KEY` — the Gemini API key, according to the application's configuration

Generate a random JWT secret using:

```powershell
python -c "import secrets; print(secrets.token_urlsafe(48))"
```

Use the actual variable names defined in `.env.example` and follow any additional configuration required by the application.

**Security:** Never commit `.env` files, API keys, passwords, or other credentials to GitHub.

### 3. Start the Application with Docker

```powershell
docker compose up --build -d
docker compose ps
```

Once the services are running, open:

| Service | URL |
|---|---|
| Frontend | http://localhost:3000 |
| Backend Health Check | http://localhost:8000/health |
| FastAPI Swagger UI | http://localhost:8000/docs |

Swagger documentation availability depends on the application configuration.

### 4. View Application Logs

```powershell
docker compose logs --tail=100 backend
docker compose logs --tail=100 frontend
docker compose logs --tail=100 postgres
```

### 5. Stop the Application

```powershell
docker compose down
```

To stop the services without deleting the database volume, do not add the `-v` flag.

## Running Without Docker

### Backend

Create and activate a Python virtual environment, install the project dependencies, configure environment variables, apply the database migrations, and start the FastAPI application using the repository's configured Uvicorn entry point.

### Frontend

Install Node.js 20+ and run the following commands in PowerShell:

```powershell
cd frontend
Copy-Item .env.example .env.local
npm install
npm run dev
```

Configure the frontend environment variables to point to the local backend API. Use the environment template supplied with the frontend.

## Testing and Continuous Integration

The project includes Pytest tests and a GitHub Actions CI workflow.

Run the backend test suite in an environment with the required dependencies installed:

```powershell
pytest
```

Additional integration and end-to-end tests can help evaluate document ingestion, retrieval quality, answer relevance, source attribution, and authentication behavior.

## Security Considerations

- Keep API keys, database passwords, JWT secrets, and access tokens out of source control.
- Use strong, unique secrets for local and deployed environments.
- Restrict database access and avoid exposing PostgreSQL publicly.
- Use HTTPS and a secure secrets manager for deployed environments.
- Apply appropriate authorization checks to document management and administrative operations.

## Future Enhancements

- Expand automated integration and end-to-end test coverage.
- Improve monitoring, structured logging, and performance evaluation.
- Enhance retrieval quality and evaluate answer relevance across diverse enterprise documents.
- Add scalable document storage and production cloud deployment.
- Introduce rate limiting and durable background processing for larger workloads.

## Author

**Shruti Mawalkar**

Built as a portfolio project exploring full-stack development, Generative AI, RAG, REST APIs, authentication, vector databases, and containerized application workflows.

## Project Links

- **GitHub Repository:** https://github.com/ShrutiMawalkar/Enterprise-RAG-Copilot

