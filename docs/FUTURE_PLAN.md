## Future Plan – Sociomatic

This document captures the **future scope of work** for Sociomatic beyond the current MVP, with a focus on evolving the system into an **agent‑aware marketing co‑pilot** while staying grounded in the existing roadmap.

### Near‑Term (v0–v0.3) – Ship the Core Loop

- **MVP delivery (v0)**
  - Implement the basic idea → AI drafts → post drafts → manual posting loop for a **single LinkedIn workspace**.
  - Use a simple **Content service** backed by Gemini to generate 2–3 LinkedIn variants for an Idea.
  - Keep the code structured so this service can later be treated as a **Content agent**.

- **Automation & reliability (v0.1–v0.2)**
  - Add LinkedIn OAuth and background workers for automated posting.
  - Introduce multi‑workspace and members with simple roles.
  - Start tracking post IDs and basic metrics needed for later analytics.

- **Analytics & reporting foundations (v0.3)**
  - Store impressions, reactions, comments, shares for each post.
  - Build simple dashboards and a basic weekly email summary.
  - Introduce an `AgentRun`/job concept in the backend so that content generation and reporting can be modeled as **discrete runs** with status and logs.

### Mid‑Term (v0.4–v1.0) – Introduce Agents Gradually

- **Media and multi‑platform (v0.4)**
  - Add support for X/Instagram and cross‑platform campaigns derived from a single Idea.
  - Create a **Creative agent (v0)** that suggests image/video concepts or prompts for each post.

- **Analytics‑driven insights (v0.3+ incremental)**
  - Evolve reporting into a **Reporting agent (v0)**:
    - Generates weekly and monthly summaries from stored metrics.
    - Surfaces top‑performing posts, underperformers, and basic recommendations.

- **Agentization of existing services (towards v1.0)**
  - Promote:
    - The Gemini content service → **Content agent**.
    - Analytics + metrics ingestion → **Data agent**.
    - Reporting + recommendations → **Analyst/Reporting agents**.
  - Standardize inputs/outputs and logging of each agent via `AgentRun`.

### Long‑Term (v1.0+) – Orchestrated Agentic Workflows

- **Multi‑agent workflows**
  - Implement orchestrated flows where agents collaborate:
    - Data agent collects and normalizes metrics and external signals.
    - Analyst agent produces a one‑page insight report.
    - Content agent proposes a week’s content plan and drafts posts.
    - Creative agent suggests visuals or generates assets.
    - Reporting agent summarizes impact and closes the loop.

- **Human‑in‑the‑loop review**
  - Provide clear UI surfaces in the app for:
    - Reviewing and editing agent outputs (insights, drafts, creative ideas, reports).
    - Approving or rejecting actions and feeding that feedback back into prompts and configuration.

- **Extensibility**
  - Make it easy to add new agents (e.g., a Challenge agent, Campaign experiment agent) without changing core domain models.
  - Keep agents loosely coupled via typed contracts and shared persistence, as detailed in `AGENTIC_SYSTEM.md`.

### Relationship to Existing Docs

- `PRODUCT.md` defines the **MVP vision and user stories**; this plan keeps those intact and adds a longer horizon.
- `ROADMAP.md` captures feature milestones; this document explains how those milestones evolve into **agentic capabilities**.
- `AGENTIC_SYSTEM.md` provides a **deep dive** into agents, their contracts, and orchestration; it should be read alongside this plan when designing new features beyond v0.3.

