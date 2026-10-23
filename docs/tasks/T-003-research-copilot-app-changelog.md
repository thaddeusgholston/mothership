# T-003: Research: mine the GitHub Copilot app changelog

- **Status:** Ready
- **Depends on:** none
- **Specs/ADRs:** feeds `docs/spec/`

## Goal

Turn the GitHub Copilot app's public changelog into a feature inventory and a risk checklist we can use to write Mothership's spec.

## Context

- Changelog: https://github.com/github/app/blob/main/changelog.md (4,400+ lines; the app's source code is not public)
- Product page: https://github.com/features/ai/github-app
- Known so far: the app is built with Tauri 2 and React, and runs the Copilot CLI as its only agent.

## In scope

- The whole changelog, not just recent releases

## Out of scope

- Downloading, decompiling, or inspecting the app itself

## Acceptance criteria

- [ ] `docs/research/reference-app.md` groups features by area: sessions, worktrees, issues and PRs, review and merge, customization (MCP, skills, plugins), automations, models and providers, settings and onboarding, platforms and updates.
- [ ] Each feature has a one-line description and a suggested Mothership tier: MVP, V1, V2, or skip.
- [ ] A risk checklist lists bugs they fixed that we should design against, each with the lesson for us. Example: routine worktree cleanup deleted uncommitted work while a session was still active.
- [ ] Everything is written in our own words, with no copied changelog passages.
- [ ] Open questions are listed at the end.

## Deliverables

`docs/research/reference-app.md`
