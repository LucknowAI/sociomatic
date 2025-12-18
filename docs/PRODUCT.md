# Product Document – Sociomatic (Dev Community LinkedIn Co‑Pilot)

## Vision

Sociomatic helps tech/dev communities **publish consistently on LinkedIn** by turning real community activity  
(events, launches, wins, content) into **high‑quality, ready‑to‑post LinkedIn updates** with minimal manual effort.

## Target Users

- **Primary**: Community managers / organizers of dev communities.
- **Secondary**: Developer advocates, core contributors, and active community members.

## Problem

Community teams typically:

- Run **multiple events, releases, and initiatives** but fail to convert them into regular LinkedIn content.
- Depend on **one or two volunteers** to manually write posts, leading to burnout and inconsistency.
- Use generic scheduling tools that **don’t understand dev context** or community workflows.

Existing tools are:

- Built for agencies/brands, not **small dev communities**.
- Focused on **multi‑platform social media**, not “LinkedIn‑first” thought leadership.
- Focused on **individual creators**, not community pages and shared calendars.

## Goals (MVP, Concrete)

- **G1 – Speed**: A community manager can turn a single brief (Idea) into **at least 2 LinkedIn‑ready drafts** in **< 5 minutes**.
- **G2 – Visibility**: The manager can see **all scheduled posts for the next 7 days** in a single screen.
- **G3 – Effort reduction**: Writing time per post is reduced by **≥ 50%** compared to current manual process.
- **G4 – Adoption**: In your own community, Sociomatic is used to publish **at least 10 real LinkedIn posts** in the first month.

## Non‑Goals (MVP)

- Automated posting via LinkedIn API (planned for **v0.1**).
- Deep analytics and reporting dashboards.
- Multi‑platform support (X, Instagram).
- Media generation (images, video, carousels).

## Key User Stories (MVP)

1. **Create idea from real activity**  
   As a community manager, I can create an Idea from an event/launch/update with fields like title, description, type, date, and link.
2. **Generate LinkedIn draft with Gemini**  
   As a community manager, I can click “Generate LinkedIn posts” on an Idea and get 2–3 AI‑generated drafts tailored for LinkedIn using the Gemini API.
3. **Edit and schedule**  
   As a community manager, I can edit a chosen draft, set a date/time, and save it as a scheduled post.
4. **View next 7 days**  
   As a community manager, I can see all posts for the next 7 days grouped by date with status (draft/scheduled/manual‑posted).
5. **Post manually with one click**  
   As a community manager, I can copy a post’s text and open my LinkedIn page from within Sociomatic, then mark that post as manually posted.

## MVP Scope

### Included

- Email/password auth.
- Single community workspace.
- Idea creation.
- AI‑powered LinkedIn post generation (text only, via Gemini).
- PostDraft editing and scheduling (date/time).
- Simple schedule/list view.
- Manual posting helpers (copy, open LinkedIn, mark as posted).

### Excluded

- Team permissions and complex roles.
- Multi‑community / multi‑tenant support.
- Automated posting and analytics (v0.1+).


