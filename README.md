# Infosys AI Knowledge Assistant (Enterprise GPT)

An enterprise knowledge assistant that enables employees to search approved organizational documents, retrieve relevant evidence, generate grounded answers with citations, and access selected operational tools through a controlled AI workflow.

The project combines document ingestion, vector search, role-based access control, PostgreSQL-backed application data, MCP-style operational tool integration, grounded response generation, citations, feedback, and analytics.

> **Important:** The documents included for demonstration/testing may contain synthetic project data. They are not official Infosys corporate policies, HR policies, security policies, or business commitments.

---

## 1. Project Overview

Enterprise teams often need to repeatedly search through documents, runbooks, policies, project references, and operational information to answer routine questions.

The Infosys AI Knowledge Assistant provides a centralized interface where users can:

- Ask questions about approved knowledge documents
- Retrieve relevant document evidence using RAG
- Receive grounded answers with source citations
- Upload and index new PDF/DOCX knowledge documents
- Retrieve selected operational information through MCP-style tools
- Submit feedback on generated answers
- View usage and quality analytics
- Manage users, documents, connectors, and governance through admin functionality

The system is designed to reduce unsupported answers by grounding responses in retrieved evidence and returning an insufficient-evidence response when appropriate information is unavailable.

---

## 2. Key Features

### Employee Features

- Secure login using JWT authentication
- Role-based access control
- Enterprise knowledge search
- RAG-based document retrieval
- Grounded AI responses
- Source citations
- Source/document visibility
- Feedback submission
- Operational incident lookup through an integrated tool

### Knowledge Management

- PDF/DOCX document upload
- Document indexing
- Text extraction
- Chunking and embedding
- Vector storage using ChromaDB
- Metadata-aware retrieval
- Retrieval relevance filtering

### AI Workflow

- Query classification
- RBAC classification
- Tool selection
- RAG retrieval
- MCP tool routing
- Grounded synthesis
- Citation generation
- Answer validation

### Administration

- User management
- Document management
- Connector management
- Governance controls
- Audit log access
- Analytics

---

## 3. High-Level Architecture

```text
                         ┌──────────────────────┐
                         │      Next.js UI      │
                         │   React + Tailwind   │
                         └──────────┬───────────┘
                                    │
                                    │ HTTP / REST
                                    ▼
                         ┌──────────────────────┐
                         │      FastAPI API     │
                         │   Authentication     │
                         │   RBAC / Routes      │
                         └──────────┬───────────┘
                                    │
                    ┌───────────────┼────────────────┐
                    │               │                │
                    ▼               ▼                ▼
             ┌────────────┐ ┌──────────────┐ ┌──────────────┐
             │ PostgreSQL │ │ AI Workflows │ │ MCP / Tools  │
             │ Users      │ │ Classify     │ │ Operational  │
             │ Documents  │ │ Retrieve     │ │ Connectors   │
             │ Feedback   │ │ Synthesize   │ │              │
             │ Audit Logs │ │ Validate     │ │              │
             └────────────┘ └──────┬───────┘ └──────────────┘
                                   │
                                   ▼
                            ┌──────────────┐
                            │  ChromaDB    │
                            │ Vector Store │
                            └──────┬───────┘
                                   │
                                   ▼
                            ┌──────────────┐
                            │  Gemini LLM  │
                            │ + Embeddings │
                            └──────────────┘
```

---

## 4. RAG Workflow

A typical knowledge query follows this flow:

```text
User Question
      │
      ▼
Authentication / RBAC
      │
      ▼
Query Classification
      │
      ├───────────────► Operational Tool Query
      │                         │
      │                         ▼
      │                    MCP Tool Lookup
      │
      ▼
Knowledge Query
      │
      ▼
Embedding / Retrieval
      │
      ▼
ChromaDB Vector Search
      │
      ▼
Relevance Filtering
      │
      ▼
Grounded Synthesis
      │
      ▼
Citation Builder
      │
      ▼
Answer Validation
      │
      ▼
Grounded Answer + Sources
```

The workflow is designed so that the language model does not simply answer every question from its general knowledge.

If sufficiently relevant approved evidence cannot be retrieved, the system can return an insufficient-evidence response instead of presenting unsupported information as fact.

---

## 5. AI Workflow Components

The main AI workflow is organized under:

```text
ai_workflows/
├── query_classification/
│   ├── __init__.py
│   ├── query_classifier.py
│   └── rbac_classifier.py
│
├── tool_selection/
│   ├── __init__.py
│   └── tool_selector.py
│
├── rag_retrieval/
│   ├── __init__.py
│   └── retriever.py
│
├── grounded_synthesis/
│   ├── __init__.py
│   └── synthesis_engine.py
│
├── citation_builder/
│   ├── __init__.py
│   └── citation_formatter.py
│
├── answer_validation/
│   ├── __init__.py
│   └── answer_validator.py
│
├── workflow.py
└── README.md
```

### Query Classification

Determines the type of request and helps route the query to the appropriate workflow.

### RBAC Classification

Ensures that user permissions and roles are considered before accessing protected functionality.

### Tool Selection

Determines whether the request should use document retrieval or an operational tool.

### RAG Retrieval

Searches the enterprise knowledge vector store for relevant document chunks and applies relevance filtering.

### Grounded Synthesis

Uses retrieved evidence to construct the response.

### Citation Builder

Formats document/source information associated with the retrieved evidence.

### Answer Validation

Checks the generated response against the available evidence and helps prevent unsupported answers.

---

## 6. MCP / Operational Tool Integration

The application includes an operational incident lookup workflow.

For example, a query containing an incident ID can be routed to the incident status lookup tool rather than being answered solely from the document knowledge base.

Example:

```text
User:
What is the status of incident INC-1001?

        ↓

Query / Tool Selection

        ↓

Incident Status Lookup

        ↓

Incident evidence

        ↓

Grounded response
```

The demonstration environment contains synthetic incident data for testing this workflow.

---

## 7. Authentication and RBAC

The application uses JWT-based authentication.

Supported roles include:

### Employee

Can:

- Query the knowledge assistant
- View sources
- Submit feedback

### Manager

Includes employee capabilities and additional team analytics access.

### Admin

Includes administrative capabilities such as:

- User management
- Document management
- Connector management
- Governance management
- Audit log access
- Team analytics

Access is controlled through backend permissions rather than relying only on frontend visibility.

---

## 8. Technology Stack

### Frontend

- Next.js
- React
- TypeScript
- Tailwind CSS

### Backend

- Python
- FastAPI
- Uvicorn
- Pydantic
- SQLAlchemy

### Database

- PostgreSQL

### Vector Database

- ChromaDB

### AI

- Google Gemini
- Gemini embeddings
- RAG-based retrieval

### Authentication

- JWT
- Role-based permissions

### Document Processing

- PyPDF
- python-docx

### Deployment

- Vercel — frontend
- Render — backend
- Render PostgreSQL — production database

### Version Control

- Git
- GitHub

---

## 9. Repository Structure

```text
infosys-ai-knowledge-assistant/
│
├── ai_workflows/
│   ├── query_classification/
│   ├── tool_selection/
│   ├── rag_retrieval/
│   ├── grounded_synthesis/
│   ├── citation_builder/
│   ├── answer_validation/
│   ├── workflow.py
│   └── README.md
│
├── backend/
│   ├── routes/
│   ├── schemas/
│   ├── services/
│   ├── uploads/
│   ├── main.py
│   ├── requirements.txt
│   └── .env.example
│
├── data/
│
├── deployment/
│   └── README.md
│
├── docs/
│
├── frontend/
│   ├── app/
│   ├── components/
│   ├── contexts/
│   ├── public/
│   └── .env.example
│
├── ingestion_pipeline/
│
├── tests/
│
├── uploads/
│
├── vector_db/
│
├── .gitignore
├── .python-version
└── README.md
```

`uploads/` and `vector_db/` contain runtime/local data and should not be treated as application source code.

Production vector data is populated through the application's document upload and indexing workflow.

---

## 10. Environment Variables

### Backend

The example configuration is available at:

```text
backend/.env.example
```

Example:

```env
GOOGLE_API_KEY=
DATABASE_URL=
JWT_SECRET_KEY=replace-with-a-secure-secret
VECTOR_DB_PATH=vector_db
VECTOR_COLLECTION_NAME=enterprise_knowledge
LLM_MODEL=gemini-2.5-flash
EMBEDDING_MODEL=gemini-embedding-2
```

Do not commit real API keys, database passwords, JWT secrets, or other credentials.

### Frontend

The example configuration is available at:

```text
frontend/.env.example
```

Example:

```env
NEXT_PUBLIC_API_BASE_URL=http://localhost:8000
```

For production, this variable points to the deployed FastAPI backend.

---

## 11. Local Setup

### Prerequisites

Install:

- Python 3.12
- Node.js
- npm
- PostgreSQL
- Git

The repository specifies Python through:

```text
.python-version
```

### Clone the Repository

```bash
git clone https://github.com/pulkitn-analytics/infosys-ai-knowledge-assistant.git
cd infosys-ai-knowledge-assistant
```

### Backend Setup

Create and activate a virtual environment:

```bash
python -m venv .venv
```

Windows:

```powershell
.venv\Scripts\Activate.ps1
```

Install dependencies:

```bash
pip install -r backend/requirements.txt
```

Create the backend environment file:

```text
backend/.env
```

Use `backend/.env.example` as the template and provide the required configuration values.

Start the backend:

```bash
cd backend
PYTHONPATH=.. uvicorn main:app --reload --port 8000
```

On Windows PowerShell, an alternative is:

```powershell
$env:PYTHONPATH=".."
uvicorn main:app --reload --port 8000
```

Backend API documentation is available at:

```text
http://localhost:8000/docs
```

### Frontend Setup

Open a separate terminal:

```bash
cd frontend
npm install
```

Create:

```text
frontend/.env.local
```

Example:

```env
NEXT_PUBLIC_API_BASE_URL=http://localhost:8000
```

Start the frontend:

```bash
npm run dev
```

The application is then available at:

```text
http://localhost:3000
```

---

## 12. Document Upload and Indexing

Authorized users can upload supported knowledge documents through the application.

The workflow is:

```text
Upload document
      ↓
Document stored
      ↓
Text extraction
      ↓
Chunking
      ↓
Embedding generation
      ↓
ChromaDB indexing
      ↓
Document available for retrieval
```

Supported document types include PDF and DOCX through the backend document-processing workflow.

After indexing, users can ask questions about the newly added document.

---

## 13. API Overview

The backend exposes REST APIs for the major application workflows.

### Authentication

```text
POST /auth/login
```

### Current User

```text
GET /users/me
```

### Documents

```text
POST /documents/upload
POST /documents/{document_id}/index
```

### Knowledge Retrieval

```text
POST /retrieval/query
POST /query
```

### Feedback

```text
POST /feedback
```

### Analytics

```text
GET /analytics/overview
```

### Administrative APIs

The backend also provides administrative APIs for:

- Users
- Connectors
- Governance
- Audit logs

Complete interactive API documentation is available through FastAPI Swagger UI at:

```text
/docs
```

---

## 14. Testing

The repository contains automated tests under:

```text
tests/
```

The AI workflow also contains component-level test coverage.

Run the test suite from the project root:

```bash
pytest
```

The test suite covers important workflow behavior including retrieval, routing, validation, and other backend functionality.

---

## 15. Deployment

The application is deployed as a separate frontend and backend.

### Frontend

Platform:

```text
Vercel
```

Production application:

https://infosys-ai-knowledge-assistant.vercel.app

### Backend

Platform:

```text
Render
```

Production backend:

https://infosys-ai-knowledge-assistant-ifkw.onrender.com

Backend health endpoint:

```text
/
```

Backend API documentation:

```text
/docs
```

### Production Database

The deployed backend uses PostgreSQL hosted through Render.

Production secrets and database credentials are configured through deployment environment variables and are not committed to GitHub.

Additional deployment information is available in:

```text
deployment/README.md
```

---

## 16. Demo Accounts

The project contains demonstration accounts for testing.

### Employee

```text
Email: employee@infosys.com
Password: password123
Role: employee
```

### Admin

```text
Email: admin@infosys.com
Password: Admin@12345
Role: admin
```

These are **demo/test credentials only** and must not be treated as real Infosys credentials.

For a production deployment, credentials should be replaced with secure account-management procedures and strong secrets.

---

## 17. Suggested Demonstration Flow

A concise project demonstration can follow this sequence:

### 1. Login

Log in as an employee.

### 2. Knowledge Query

Ask a question covered by an indexed document.

### 3. Grounded Response

Show the generated answer and its supporting source/citation.

### 4. Source Verification

Open/view the cited source to demonstrate grounding.

### 5. Operational Tool

Ask an incident-status question such as:

```text
What is the status of incident INC-1001?
```

Demonstrate that the request is routed to the operational incident lookup workflow.

### 6. Knowledge Upload

Log in as an administrator and upload a PDF/DOCX document.

### 7. Indexing

Index the uploaded document.

### 8. Query the New Document

Ask a question about the newly indexed document and verify that the answer contains supporting evidence.

### 9. Feedback / Analytics

Submit feedback and demonstrate the analytics/quality dashboard.

---

## 18. Grounding and Unsupported Questions

The assistant is designed to avoid treating general model knowledge as approved enterprise evidence.

For example, if the knowledge base does not contain an approved source for a question, the system may return an insufficient-evidence response rather than inventing an organizational policy.

This is particularly important for questions involving:

- HR policies
- Compensation
- Benefits
- Security procedures
- Client information
- Internal business data
- Corporate policies
- Operational procedures

The synthetic documents included with the project are intended for demonstrating this workflow and should not be interpreted as official Infosys policy.

---

## 19. Security Considerations

The project follows several basic security practices:

- Secrets are stored through environment variables
- `.env` files are excluded through `.gitignore`
- JWT authentication is used for protected APIs
- Role-based permissions are enforced in the backend
- Administrative functionality is permission-controlled
- Production credentials should not be committed to source control
- Sensitive deployment values should be configured through hosting-platform environment settings

Before transferring the project to a production environment, credentials and secrets should be rotated and managed through an appropriate secret-management process.

---

## 20. Current Limitations

This project is a demonstration/prototype implementation and has several limitations.

- The knowledge base depends on documents that have been uploaded and indexed.
- Synthetic documents are used for several demonstration scenarios.
- The operational MCP connector currently demonstrates a controlled incident lookup workflow.
- Production vector data is environment-specific.
- Enterprise-scale document governance, observability, and security controls would require additional production hardening.
- The system should not be treated as an authoritative source for real Infosys corporate policies unless those policies are explicitly provided through an approved knowledge source.

---

## 21. Future Improvements

Potential future improvements include:

- Additional enterprise connectors
- More MCP tools
- Advanced document-level permissions
- Improved retrieval evaluation
- Reranking models
- Automated document lifecycle management
- More detailed observability
- Conversation history management
- Enterprise SSO integration
- Stronger production secret management
- Expanded automated evaluation datasets
- Human review workflows for sensitive answers

---

## 22. Project Status

Current implementation includes:

- [x] Next.js frontend
- [x] FastAPI backend
- [x] PostgreSQL database
- [x] JWT authentication
- [x] Role-based access control
- [x] Document upload
- [x] Document indexing
- [x] ChromaDB vector retrieval
- [x] Grounded AI responses
- [x] Citations
- [x] Answer validation
- [x] MCP-style incident lookup
- [x] Feedback
- [x] Analytics
- [x] Admin functionality
- [x] Automated tests
- [x] GitHub repository
- [x] Render backend deployment
- [x] Vercel frontend deployment

---

## 23. Repository

GitHub:

https://github.com/saymodak28/infosys-ai-knowledge-assistant.git

Live application:

https://infosys-ai-knowledge-assistant.vercel.app

Backend:

https://infosys-ai-knowledge-assistant-ifkw.onrender.com

---

## 24. Disclaimer

This project was developed as an educational/project demonstration of an enterprise AI knowledge assistant.

Any synthetic documents, example employee data, incident records, policies, operational procedures, or other demonstration content included in the project are created for testing and demonstration purposes and do not represent official Infosys documentation or policy.

## 25 Group Members 

| No. | Name                                            |
| --: | ------------------------ 
|   1 | M.S. Pavan Shankar       
|   2 | Chandra Akash Kiran      
|   3 | Shruti Vishwas Deshpande 
|   4 | Soumyakanta Mishra       
|   5 | Sayan Modak         
|   6 | Pulkit Narang            
|   7 | Subhansu Bose           
|   8 | Sanket Arun Patil       

## 26 Demo Video

[Watch the Project Demo](PASTE-YOUR-DEMO-VIDEO-LINK-HERE)
