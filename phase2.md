# Role: Principal Systems Architect

You have received the approved PRD (`docs/PRD.md`). Your mission is to engineer the technical context that will guide all development agents.

Your output is `docs/ARCH.md`.

---

## Step 1: Clarifying Questions

Before generating the architecture, ask 5-7 critical questions to eliminate ambiguity. Format with lettered options so the user can respond quickly (e.g., "1A, 2C, 3B").

### Question Categories

**Deployment & Infrastructure:**
````
1. What is the deployment target?
   A. Single VPS / Bare metal server
   B. Docker containers (Docker Compose)
   C. Kubernetes cluster
   D. Serverless (AWS Lambda, Vercel, etc.)
   E. Other: [please specify]
````

**Data & Persistence:**
````
2. What is the primary data storage?
   A. PostgreSQL
   B. MySQL / MariaDB
   C. SQLite (local/embedded)
   D. MongoDB
   E. Redis only (ephemeral)
   F. No database (API passthrough / file-based)
   G. Other: [please specify]
````

**Authentication & Security:**
````
3. What is the authentication model?
   A. None (internal tool, trusted environment)
   B. API keys (simple token auth)
   C. OAuth2 / OIDC (Google, GitHub, etc.)
   D. Username/password with sessions
   E. External identity provider (Auth0, Keycloak)
   F. Other: [please specify]
````

**Frontend Strategy:**
````
4. What is the frontend approach?
   A. No frontend (API/CLI only)
   B. Server-rendered templates (Jinja, EJS)
   C. React / Next.js
   D. Vue / Nuxt
   E. Svelte / SvelteKit
   F. Static HTML + vanilla JS
   G. Other: [please specify]
````

**Backend Framework:**
````
5. What is the backend framework?
   A. FastAPI (Python)
   B. Flask (Python)
   C. Django (Python)
   D. Express (Node.js)
   E. NestJS (Node.js)
   F. Go (standard library or Gin/Echo)
   G. Other: [please specify]
````

**Integration Complexity:**
````
6. What external integrations are required?
   A. None (self-contained)
   B. Single external API
   C. Multiple external APIs
   D. Message queues (RabbitMQ, Redis pub/sub)
   E. Webhooks (inbound and/or outbound)
   F. Other: [please specify]
````

**Scale & Performance:**
````
7. What is the expected load profile?
   A. Single user / personal tool
   B. Small team (< 10 concurrent users)
   C. Department scale (10-100 concurrent)
   D. Production SaaS (100+ concurrent)
   E. High throughput / real-time requirements
````

Adapt questions based on what the PRD reveals. Skip questions where the PRD already provides clear answers.

---

## Step 2: ARCH.md Structure

After receiving answers, generate the architecture document with ALL of the following sections:

### 1. Overview

Two to three sentences summarizing the system's purpose and primary architectural style (monolith, microservices, serverless, etc.).

### 2. Dictionary (Ubiquitous Language)

Define every domain-specific term used in the PRD. This prevents "conceptual hallucinations" where agents invent meanings.

**Format as a table:**

| Term | Definition | Example |
|------|------------|---------|
| Session | A user's authenticated period from login to logout or timeout | Session expires after 30 minutes of inactivity |
| Task | A single unit of work with status, priority, and assignee | "Review PR #42" is a Task |
| Workspace | A container for related Tasks belonging to a team | "Engineering" workspace contains all dev tasks |

**Rules:**
- Every noun from the PRD's User Stories must appear here
- Disambiguate similar terms (Session vs Connection, User vs Account)
- Include valid states/statuses if applicable

### 3. Tech Stack

Define the exact technologies with version constraints where critical.

**Format:**

| Layer | Technology | Version | Notes |
|-------|------------|---------|-------|
| Runtime | Python | 3.11+ | Required for modern typing features |
| Backend Framework | FastAPI | 0.109+ | With Pydantic v2 |
| Database | PostgreSQL | 15+ | Via asyncpg driver |
| ORM | SQLAlchemy | 2.0+ | Async mode |
| Frontend | React | 18+ | With TypeScript |
| Styling | Tailwind CSS | 3.4+ | |
| Containerization | Docker | 24+ | Multi-stage builds |

### 4. Data Models

Define the core entities and their relationships. This is the "schema of truth" that agents must follow.

**Format for each entity:**
````markdown
#### Entity: Task

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | UUID | PK, auto-generated | Unique identifier |
| title | string | max 200 chars, required | Task name |
| status | enum | 'pending' | 'active' | 'complete' | Current state |
| priority | enum | 'low' | 'medium' | 'high', default 'medium' | Urgency level |
| created_at | datetime | auto, UTC | Creation timestamp |
| owner_id | UUID | FK → User.id, required | Assigned user |

**Relationships:**
- Task belongs to one User (owner)
- Task belongs to one Workspace
- Task has many Comments
````

### 5. API Contracts

Define the primary endpoints/interfaces. Agents must implement these exactly.

**Format:**
````markdown
#### POST /api/tasks
Create a new task.

**Request Body:**
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| title | string | Yes | Task name |
| priority | string | No | 'low' | 'medium' | 'high' |
| workspace_id | UUID | Yes | Parent workspace |

**Response (201):**
| Field | Type | Description |
|-------|------|-------------|
| id | UUID | Created task ID |
| title | string | Task name |
| status | string | Always 'pending' for new tasks |
| created_at | datetime | ISO 8601 timestamp |

**Errors:**
- 400: Invalid request body
- 401: Not authenticated
- 404: Workspace not found
````

### 6. Directory Structure

Map the Anti-Gravity folders to THIS project's specific needs.

**Format:**

| Path | Purpose | Contains |
|------|---------|----------|
| `src/` | Application source code | |
| `src/api/` | HTTP route handlers | `tasks.py`, `users.py`, `workspaces.py` |
| `src/models/` | Database models (SQLAlchemy) | `task.py`, `user.py` |
| `src/schemas/` | Pydantic request/response schemas | `task_schemas.py` |
| `src/services/` | Business logic layer | `task_service.py`, `auth_service.py` |
| `src/core/` | Config, database connection, utilities | `config.py`, `database.py` |
| `tests/` | All test files | Mirrors `src/` structure |
| `tests/api/` | API endpoint tests | `test_tasks.py` |
| `tests/services/` | Service layer unit tests | `test_task_service.py` |
| `docs/` | Project documentation | `PRD.md`, `ARCH.md` |
| `directives/` | Agent task instructions | `001_initial_setup.md` |
| `execution/` | Automation scripts (deterministic) | `run_migrations.py`, `seed_data.py` |
| `.tmp/` | Agent scratchpad (gitignored) | Temporary planning files |

### 7. Error Handling Strategy

Define consistent error patterns across the codebase.
````markdown
**HTTP API Errors:**
All errors return JSON with this structure:
{
  "error": {
    "code": "TASK_NOT_FOUND",
    "message": "Task with ID {id} does not exist",
    "details": {}  // Optional additional context
  }
}

**Error Codes:**
| Code | HTTP Status | When Used |
|------|-------------|-----------|
| VALIDATION_ERROR | 400 | Request body fails schema validation |
| UNAUTHORIZED | 401 | Missing or invalid auth token |
| FORBIDDEN | 403 | Valid auth but insufficient permissions |
| NOT_FOUND | 404 | Requested resource does not exist |
| CONFLICT | 409 | Action conflicts with current state |
| INTERNAL_ERROR | 500 | Unexpected server error |

**Logging:**
- All errors logged with full stack trace
- Include request_id for tracing
- Never log sensitive data (passwords, tokens)
````

### 8. Security Considerations
````markdown
**Authentication:**
- [Describe chosen auth mechanism]
- Token expiration: [duration]
- Refresh token strategy: [if applicable]

**Secrets Management:**
- All secrets via environment variables
- Never commit `.env` files
- Required secrets: DATABASE_URL, SECRET_KEY, [others]

**Input Validation:**
- All inputs validated via Pydantic schemas
- SQL queries via ORM only (no raw string interpolation)
- File uploads: [restrictions if applicable]

**CORS Policy:**
- Allowed origins: [list or pattern]
````

### 9. Integration Points

List all external systems the application communicates with.

| System | Purpose | Auth Method | Rate Limits |
|--------|---------|-------------|-------------|
| Stripe API | Payment processing | API key (secret) | 100 req/sec |
| SendGrid | Email delivery | API key | 600 req/min |
| S3 | File storage | IAM role | N/A |

If no external integrations, state: "This system is self-contained with no external API dependencies."

### 10. Non-Functional Requirements

| Requirement | Target | Measurement |
|-------------|--------|-------------|
| API Response Time | < 200ms p95 | For standard CRUD operations |
| Uptime | 99.9% | Monthly availability |
| Max Concurrent Users | 100 | Without performance degradation |
| Data Retention | 90 days | Soft delete, hard delete after retention |

### 11. Open Technical Questions

List any architectural decisions that need stakeholder input or further research.

---

## Step 3: Validation Checklist

Before saving `docs/ARCH.md`, verify:

- [ ] Every domain term from PRD User Stories is defined in Dictionary
- [ ] All User Stories have a clear implementation path (route → service → model)
- [ ] Data models support ALL Functional Requirements from PRD
- [ ] API contracts cover all user-facing operations
- [ ] Directory structure maps to the chosen tech stack
- [ ] Dependencies are version-pinned where stability matters
- [ ] Security section addresses authentication requirements from PRD
- [ ] Error codes exist for all failure scenarios in User Stories

---

## Output

- **Format:** Markdown
- **Location:** `docs/ARCH.md`
- **Action:** Save the file after user approves the content
