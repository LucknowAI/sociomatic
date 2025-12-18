# Sociomatic Roadmap

## v0 – Weekend MVP (LinkedIn text, manual posting)

- **Auth & setup**
  - Email/password auth.
  - Single hard‑coded workspace for your dev community.
- **Ideas**
  - Create Ideas with title, description, type, audience, link, tags.
  - List all Ideas for the logged‑in user.
- **AI (Gemini) integration**
  - Connect to Google Gemini API using an API key.
  - Generate 2–3 LinkedIn post variants for a selected Idea.
- **PostDrafts & scheduling**
  - Create PostDraft from AI output or manual input.
  - Edit content, set scheduled date/time, set status (draft/scheduled/manual‑posted).
- **Schedule view & manual posting**
  - 7‑day list view grouped by date.
  - Copy post content, open LinkedIn page, and mark as “manually posted”.

## v0.1 – LinkedIn Automation & Reliability

- **LinkedIn posting**
  - Integrate LinkedIn API for the community/page.
  - Implement OAuth and token storage.
  - Publish posts automatically at `scheduledAt` using a background worker.
- **Reliability**
  - Job queue with retries and dead‑letter logging.
  - Post status sync (success/fail) and basic error messages in the UI.

## v0.2 – Multi‑Community & Members

- **Multi‑workspace support**
  - Add `Workspace` and `WorkspaceMember` models.
  - Allow multiple communities to onboard.
- **Members**
  - Invite members via email.
  - Assign posts to specific members and show ownership in the schedule.
- **Permissions**
  - Simple roles: owner, manager, member.

## v0.3 – Analytics & Insights

- **Metrics**
  - Store LinkedIn post IDs and fetch impressions, reactions, comments, and shares.
- **Dashboards**
  - Performance over time.
  - Top posts by engagement.
  - Performance by Idea type (event vs launch vs education).
- **Reporting**
  - Weekly email summary to community managers with highlights and suggestions.

## v0.4 – Additional Platforms & Media

- **Platforms**
  - Add X (Twitter) and/or Instagram support.
- **AI media support**
  - Suggest static image concepts and generate prompts for an image model.
  - Generate short video scripts for announcements or recaps.
- **Cross‑platform campaigns**
  - Create a single Idea and generate posts for multiple platforms with different formats.

## v0.5+ – Community Challenges (Optional but high impact)

- **Posting challenges**
  - 30‑day LinkedIn posting challenge for members.
  - Daily prompts + AI drafts for each participant.
- **Gamification**
  - Streak tracking and basic leaderboards.
  - Simple badges/achievements for participants.


