# Architecture – Sociomatic (Dev Community LinkedIn Co‑Pilot)

## Overview

The system is a web app with:

- A **React frontend** for the UI.
- A **NestJS (Node.js + TypeScript) backend** for auth, business logic, AI integration, and future agents.
- A **relational database** (PostgreSQL in practice) for persistence.
- A pluggable **AI provider** using the **Google Gemini API** for text generation (abstracted so we can swap models later).

Over time this will evolve into an **agent‑aware architecture**, where some backend services are promoted into specialized AI agents (see `AGENTIC_SYSTEM.md`).

## Components

### Frontend (React)

- **Pages**
  - `Login` / `Register`
  - `Dashboard` (posts list and schedule)
  - `CreateIdea`
  - `Settings`
- **Key UI Components**
  - `IdeaForm`
  - `PostVariants` (AI output)
  - `PostsList` (grouped by date)
  - `PostEditorModal`

The frontend communicates with the backend via JSON over HTTP, using a JWT for auth.

### Backend (NestJS)

- **Modules**
  - `AuthModule`: registration, login, JWT management.
  - `IdeasModule`: CRUD for ideas.
  - `PostsModule`: CRUD for posts/schedule.
  - `AiModule` / `ContentAgentModule`: endpoints that call the AI provider to generate LinkedIn drafts.
  - Future: `AnalyticsModule`, `ReportingAgentModule`, `CreativeAgentModule` (see `AGENTIC_SYSTEM.md`).

- **AI Integration (Gemini‑first, pluggable)**
  - A dedicated **Gemini client/service**:
    - Reads settings from env (`AI_PROVIDER`, `GEMINI_API_KEY`).
    - For MVP, `AI_PROVIDER = "gemini"` and all generations use **Google Gemini API**.
    - Exposes:
      - `generateLinkedinPosts(idea, tone) -> string[]`.
  - Responsibilities:
    - Build prompts optimized for LinkedIn (hook in first lines, hashtags, CTA).
    - Enforce max length and handle truncation.
    - Return **2–3 variants** per request.

### Data Model (Simplified)

- **User**
  - `id`
  - `email`
  - `passwordHash`
  - `createdAt`

- **Idea**
  - `id`
  - `userId`
  - `title`
  - `description`
  - `type` (`event`, `launch`, `update`, `education`)
  - `link` (optional)
  - `audience` (free text)
  - `tags` (string array / JSON)
  - `createdAt`

- **Post**
  - `id`
  - `ideaId` (FK to Idea)
  - `platform` (fixed to `linkedin` for v0)
  - `content` (text)
  - `tone`
  - `scheduledAt` (nullable)
  - `status` (`draft`, `scheduled`, `manual-posted`)
  - `createdAt`
  - `updatedAt`

- **AgentRun** (future, for agentic workflows)
  - `id`
  - `agentType` (`content`, `data`, `analyst`, `creative`, `reporting`)
  - `status` (`pending`, `running`, `succeeded`, `failed`)
  - `inputRef` (reference to Idea, time range, etc.)
  - `outputRef` (reference to generated drafts, insights, or reports)
  - `startedAt`
  - `finishedAt`

## API Endpoints (MVP)

- **Auth**
  - `POST /auth/register`
  - `POST /auth/login`

- **Ideas**
  - `GET /ideas`
  - `POST /ideas`

- **Posts**
  - `GET /posts?from=&to=&status=`
  - `POST /posts`
  - `PATCH /posts/{id}`

- **AI / Content agent**
  - `POST /ai/generate-post`
    - Input: `ideaId`, optional `tone`.
    - Output: array of post text variants.

## Future Extensions

- Add **LinkedIn OAuth** and automated posting via a background worker.
- Introduce **Workspace** and **Member** tables for multi‑tenant support.
- Add **metrics** and analytics tables for performance tracking.
- Extend `platform` to support `x`, `instagram`, and introduce media entities for images/video.
- Promote key backend services into **specialized agents** (Data, Analyst, Content, Creative, Reporting) orchestrated by dedicated workflows (see `AGENTIC_SYSTEM.md`).

