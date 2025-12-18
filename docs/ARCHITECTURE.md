# Architecture – Sociomatic (Dev Community LinkedIn Co‑Pilot)

## Overview

The system is a simple web app with:

- A **React frontend** for the UI.
- A **FastAPI backend** for auth, business logic, and AI integration.
- A **relational database** (SQLite/Postgres) for persistence.
- A pluggable **AI provider** using the **Google Gemini API** for text generation (abstracted so we can swap models later).

## Components

### Frontend (React)

- **Pages**
  - `Login` / `Register`
  - `Dashboard` (posts list)
  - `CreateIdea`
  - `Settings`
- **Key UI Components**
  - `IdeaForm`
  - `PostVariants` (AI output)
  - `PostsList` (grouped by date)
  - `PostEditorModal`

Communicates with the backend via JSON over HTTP, using a JWT for auth.

### Backend (FastAPI)

- **Modules/routers**
  - `auth`: registration, login, JWT management.
  - `ideas`: CRUD for ideas.
  - `posts`: CRUD for posts/schedule.
  - `ai`: endpoints that call the AI provider.

- **AI Integration (Gemini‑first, pluggable)**
  - `ai_client.py`:
    - Reads settings from env (`AI_PROVIDER`, `GEMINI_API_KEY`).
    - For MVP, `AI_PROVIDER = "gemini"` and all generations use **Google Gemini API**.
    - Exposes:
      - `generate_linkedin_posts(idea, tone) -> List[str]`.
  - Responsibilities:
    - Build prompts optimized for LinkedIn (hook in first lines, hashtags, CTA).
    - Enforce max length and handle truncation.
    - Return **2–3 variants** per request.

### Data Model (Simplified)

- **User**
  - `id`
  - `email`
  - `password_hash`
  - `created_at`

- **Idea**
  - `id`
  - `user_id`
  - `title`
  - `description`
  - `type` (`event`, `launch`, `update`, `education`)
  - `link` (optional)
  - `audience` (free text)
  - `tags` (comma‑separated string or JSON)
  - `created_at`

- **Post**
  - `id`
  - `idea_id` (FK to Idea)
  - `platform` (fixed to `linkedin` for v0)
  - `content` (text)
  - `tone`
  - `scheduled_at` (nullable)
  - `status` (`draft`, `scheduled`, `manual-posted`)
  - `created_at`
  - `updated_at`

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

- **AI**
  - `POST /ai/generate-post`
    - Input: `ideaId`, optional `tone`.
    - Output: array of post text variants.

## Future Extensions

- Add **LinkedIn OAuth** and automated posting via a background worker.
- Introduce **Workspace** and **Member** tables for multi‑tenant support.
- Add **metrics** and analytics tables for performance tracking.
- Extend `platform` to support `x`, `instagram`, and introduce media entities for images/video.


