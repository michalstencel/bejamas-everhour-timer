# Changelog

## 1.1.2 — 2026-11-04

- Prefer verified substantive parent work issues for small Linear subissues, without changing statuses or relationships.
- Avoid generic grouping containers and silent fallbacks when a newly created parent has not synced.
- Preserve direct tracking for substantial independent Design/Dev work.

## 1.1.1 — 2026-11-04

- Treat Linear platform ID resolution as a mapping, not proof that the task has synced to Everhour.
- Check exact task availability before starting; preserve the current timer when a newly created issue is missing.
- Document project-level **Resync Tasks**, verification after resync, and non-duplication safeguards.
- Update setup and troubleshooting guidance for delayed Linear integration sync.

## 1.1.0 — 2026-11-04

- Require a fresh skill load and live Everhour check before the first actual work request in each new conversation.
- Report missing tools, authorization, or ambiguous targets instead of silently skipping tracking.
- Accept project authorization without asking again in each chat.
- Treat sequential conversations as handoffs; require one controller only for explicitly simultaneous agent work.
- Explain that GitHub updates do not update installed or uploaded copies automatically.

## 1.0.0 — 2026-11-04

Previously shared skill with automatic task matching, verified timer switching, OAuth setup, project instructions, and first-use onboarding.
