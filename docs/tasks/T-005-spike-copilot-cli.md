# T-005: Spike: run GitHub Copilot through its CLI and SDK

- **Status:** Ready
- **Depends on:** none. Needs the Copilot CLI installed and signed in with a GitHub account that has a Copilot plan.
- **Specs/ADRs:** docs/adr/0001-desktop-stack.md

## Goal

Prove Mothership can run a Copilot session from code: stream its events, answer an approval request, and resume a session. Record every event type so we can design the shared event model (ADR 0002).

## Context

- Copilot CLI: https://github.com/github/copilot-cli
- Copilot SDK: https://github.com/github/copilot-sdk. Read the current TypeScript docs; don't rely on memory.
- Use the official CLI and SDK only. Don't call Copilot's model API directly.

## In scope

- A standalone TypeScript script in `spikes/copilot/` with its own package.json. It drives the developer's installed Copilot CLI, through the SDK if that fits.
- A scratch git repo the script creates in a temp folder, so no real project is touched

## Out of scope

- Any UI, database, or code under `src/` or `agent-host/`
- Reading Copilot or GitHub auth tokens, or building a sign-in flow. The Copilot CLI owns sign-in.

## Acceptance criteria

- [ ] `pnpm start "<prompt>"` starts a session in the scratch repo and prints each event as one JSON line.
- [ ] When the agent wants to edit files or run a command, the script asks y/n in the terminal and honors the answer.
- [ ] `pnpm start --resume <session-id> "<prompt>"` continues the same session, and the agent can recall the first prompt.
- [ ] If the Copilot CLI isn't signed in, the script explains how to sign in and exits cleanly.
- [ ] `docs/research/copilot-events.md` lists every event or message type observed, with a short redacted example of each, and shows where usage or premium-request data appears.

## Verify

Run the spike twice, once new and once resumed, with a real Copilot sign-in. Paste the redacted terminal output into the PR.

## Deliverables

`spikes/copilot/`, `docs/research/copilot-events.md`
