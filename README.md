# Sociomatic – Dev Community LinkedIn Co‑Pilot

Sociomatic is an AI‑powered LinkedIn content assistant for tech/dev communities.  
It turns simple briefs (events, launches, updates) into **ready‑to‑post LinkedIn content** and organizes them in a lightweight schedule.

## Tech Stack (Planned)

- **Backend**: Python, FastAPI, SQLAlchemy/SQLModel, SQLite/PostgreSQL.
- **Frontend**: React (Vite), TypeScript/JavaScript.
- **AI provider (initial)**: **Google Gemini API** (text‑only models), abstracted behind an internal AI client so we can swap models later.
- **Auth**: Email + password (JWT).

## Core Concepts (Data Model)

- **Idea**
  - High‑level brief created by the community manager.
  - Example: “Rust Meetup – 25 Jan”, “API v2 Launch”, “Weekly Community Highlights”.
  - Fields: `title`, `description`, `type`, `audience`, `link`, `tags`.

- **PostDraft**
  - A single LinkedIn‑ready post generated from an Idea and edited by a human.
  - Fields: `ideaId`, `content`, `tone`, `scheduledAt`, `status`.

- **Schedule**
  - The date/time a `PostDraft` should be published or manually posted.
  - For v0 we support “manual posting” (copy & paste to LinkedIn).

## Features (v0 – Weekend MVP, Concrete)

- **Idea creation**
  - Form to capture event/launch/update details.
  - Required: `title`, `description`, `type` (`event`, `launch`, `update`, `education`).

- **AI generation with Gemini**
  - Button: **“Generate LinkedIn posts”** on an Idea.
  - Backend calls **Gemini API** to generate **2–3 variants** with:
    - A strong 2‑line hook.
    - Clear CTA.
    - 3–6 relevant hashtags.

- **Post editing & saving**
  - Pick a variant, edit the text, see live character count.
  - Save as `PostDraft` with `status = "draft"` or `status = "scheduled"` and a `scheduledAt` datetime.

- **Schedule view**
  - List of posts grouped by date (7‑day view).
  - Columns: time, truncated content, status (draft/scheduled/manual‑posted).

- **Manual posting helpers**
  - **Copy text** button (copies content to clipboard).
  - **Open LinkedIn** button (opens configured LinkedIn page URL).
  - **Mark as posted** toggle to set `status = "manual-posted"`.

## Getting Started

> Note: These steps describe the planned setup. The backend and frontend scaffolding will be created next.

### Prerequisites

- Python 3.10+
- Node.js 18+
- A database (SQLite for local dev is fine)
- Google Gemini API key (e.g., `GEMINI_API_KEY`)

### Backend Setup

```bash
cd backend
python -m venv .venv
source .venv/bin/activate  # or .venv\Scripts\activate on Windows
pip install -r requirements.txt

cp .env.example .env  # set DB URL and AI API key
uvicorn main:app --reload
```

### Frontend Setup

```bash
cd frontend
npm install
cp .env.example .env  # set API base URL
npm run dev
```

Then open the frontend URL in your browser (e.g., `http://localhost:5173`).

## Roadmap (High Level)

- **v0.1**: LinkedIn API integration for automated posting.
- **v0.2**: Multi‑workspace support & member invites.
- **v0.3**: Advanced analytics & insights.
- **v0.4**: Support for additional platforms (X, Instagram) and media (images, carousels, video).

## License

TBD.


