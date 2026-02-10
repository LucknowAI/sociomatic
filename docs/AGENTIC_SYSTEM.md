## Agentic System – Sociomatic

This document describes how Sociomatic can evolve into an **agentic system** composed of specialized AI agents that collaborate to support community‑driven marketing.

The goal is not to build all agents in v0, but to **design the system so existing services can be promoted into agents over time**.

### High‑Level Overview

- **Today (v0)**: A single **Content service** powered by Gemini helps generate LinkedIn post drafts from Ideas.
- **Future**: Multiple specialized agents, each with a narrow responsibility, coordinated by orchestrated workflows:
  - **Data agent**
  - **Analyst agent**
  - **Content agent**
  - **Creative agent**
  - **Reporting agent**

Agents interact via **typed inputs/outputs** persisted in the database and tracked through `AgentRun` records (see `ARCHITECTURE.md`).

### Agents and Responsibilities

#### Data Agent

- **Purpose**: Aggregate and normalize structured and unstructured data about performance and the market.
- **Primary inputs**
  - Time ranges (e.g., last 7 days, last month).
  - Platform credentials/config (LinkedIn, later X/Instagram).
  - Optional filters (workspace, campaign, tags).
- **Primary outputs**
  - Normalized metrics snapshots (impressions, reactions, comments, shares).
  - Basic derived features (growth rates, engagement rates, best times to post).
  - Optional raw text feeds (e.g., competitor posts, notable news headlines) for downstream analysis.
- **Roadmap alignment**
  - Starts as part of analytics ingestion in **v0.3**.
  - Becomes its own agent as more sources are added (multi‑platform, external feeds).

#### Analyst Agent

- **Purpose**: Turn raw data and metrics into **actionable insights and summaries**.
- **Primary inputs**
  - Metrics and events snapshots produced by the Data agent.
  - Context: workspace strategy, upcoming campaigns, target audiences.
  - Historical performance (e.g., top ideas, recurring themes).
- **Primary outputs**
  - One‑page insight reports (e.g., “What happened last week?”).
  - Highlight lists (top posts, underperformers, topics gaining traction).
  - Suggested themes or angles for the upcoming week.
- **Roadmap alignment**
  - Extends the **Reporting** and **Analytics & Insights** work in **v0.3**.
  - Feeds the Content agent with strategy and themes.

#### Content Agent

- **Purpose**: Turn ideas, themes, and insights into **high‑quality content drafts**.
- **Primary inputs**
  - Individual Ideas (v0).
  - Weekly or campaign themes and constraints (later).
  - Brand voice and style guidelines for a workspace.
  - Insights from the Analyst agent (what’s working, what to double down on).
- **Primary outputs**
  - 2–3 LinkedIn post drafts for a given Idea (v0).
  - Multi‑day content plans (e.g., a week of posts) with suggested posting times (future).
  - Platform‑specific variants for LinkedIn, X, Instagram (future).
- **Roadmap alignment**
  - Exists in v0 as the **Gemini‑backed AI integration**.
  - Grows into a full Content agent as we support campaigns and multiple platforms.

#### Creative Agent

- **Purpose**: Provide **visual and media support** for posts.
- **Primary inputs**
  - Approved or draft social copy.
  - Creative guidelines and brand assets.
  - Campaign or weekly themes.
- **Primary outputs**
  - Image concepts, prompts, or generated assets.
  - Short video script outlines or storyboards.
- **Roadmap alignment**
  - Directly tied to **v0.4 – Additional Platforms & Media**.
  - Initially may only suggest prompts; later can call external image/video models.

#### Reporting Agent

- **Purpose**: Explain what happened, why it matters, and what to do next.
- **Primary inputs**
  - Metrics and events snapshots (from Data agent).
  - Historical decisions (e.g., which ideas were prioritized).
  - Calendar context (campaigns, events, launches).
- **Primary outputs**
  - Weekly and monthly performance reports.
  - Narrative summaries for community managers.
  - Recommended experiments or adjustments.
- **Roadmap alignment**
  - Builds on **v0.3 – Analytics & Insights** weekly summaries.
  - Provides higher‑level, narrative reporting over time.

### Orchestration and Workflows

Agents are coordinated via backend workflows. Each workflow is a composition of **AgentRuns** with explicit dependencies.

#### Example: Weekly Insights & Planning Workflow

1. **Data agent run**
   - Triggered on a schedule (e.g., Monday 05:00).
   - Fetches metrics and signals for the previous week.
   - Persists a metrics snapshot and an `AgentRun` record (`agentType = "data"`).
2. **Analyst agent run**
   - Consumes the metrics snapshot from step 1.
   - Produces a one‑page insight document and key highlights.
   - Persists the insight and an `AgentRun` record (`agentType = "analyst"`).
3. **Content agent run**
   - Uses the insight, workspace strategy, and upcoming events.
   - Proposes a content plan and drafts posts for the week.
   - Creates Ideas or PostDrafts and an `AgentRun` record (`agentType = "content"`).
4. **Creative agent run**
   - Suggests visuals or media prompts for planned posts.
   - Persists prompts/links and an `AgentRun` record (`agentType = "creative"`).
5. **Reporting agent run**
   - Wraps up the previous week’s results and the upcoming plan in a single report.
   - Can trigger an email summary to managers.

Each step can fail or be retried independently, and the UI can surface the status of each run.

### Implementation Strategy

- **Phase 1 – Services, not agents (v0–v0.2)**
  - Implement everything as standard NestJS modules/services with clear boundaries.
  - Use a single Content service to integrate with Gemini for post generation.
  - Begin using `AgentRun`‑like records only where they add immediate value (e.g., for AI generation logs).

- **Phase 2 – Light agents (v0.3–v0.4)**
  - Introduce Reporting and Creative “agents”:
    - Treat them as specialized services with stronger contracts and dedicated endpoints.
    - Start modeling runs explicitly for reporting jobs and content generation sessions.
  - Add more telemetry and logging so each run’s inputs/outputs are traceable.

- **Phase 3 – Full agentic workflows (v1.0+)**
  - Implement orchestrated workflows for weekly insights and planning.
  - Allow some agent runs to be triggered by events (e.g., a successful campaign, new product launch).
  - Add human‑in‑the‑loop UX so users can approve, reject, or tweak outputs at each stage.

### Non‑Goals and Guardrails

- **Non‑goals**
  - Building a general‑purpose agent framework.
  - Allowing agents to modify infrastructure or configs autonomously.
  - Fully autonomous posting without any human guardrails.

- **Guardrails**
  - Human approval remains required for:
    - Publishing posts.
    - Significant changes to content strategy.
  - All agent actions should be:
    - Logged (via `AgentRun`).
    - Inspectable in the UI.
    - Reproducible where possible (inputs and prompts recorded).

### Related Documents

- `FUTURE_PLAN.md` – high‑level future scope and phase plan for Sociomatic.
- `ROADMAP.md` – concrete milestones and versions.
- `ARCHITECTURE.md` – technical architecture, including the `AgentRun` model and backend modules.

