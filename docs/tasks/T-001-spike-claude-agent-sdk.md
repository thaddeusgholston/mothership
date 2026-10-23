# T-001: Spike: run Claude Code through the Agent SDK

- **Status:** Ready
- **Depends on:** none. Needs Claude Code installed and signed in on the developer's machine.
- **Specs/ADRs:** docs/adr/0001-desktop-stack.md

## Goal

Prove Mothership can run a Claude Code session from code: stream its events, handle a tool approval, and resume it. Record every event type so we can design the shared event model (ADR 0002).

## Context

- Claude Code docs index: https://code.claude.com/docs/llms.txt. Read the current Agent SDK pages; don't rely on memory.
- Package: `@anthropic-ai/claude-agent-sdk`
- Using the Agent SDK with a Claude plan: https://support.claude.com/en/articles/15036540-use-the-claude-agent-sdk-with-your-claude-plan
- Credential rules: https://code.claude.com/docs/en/legal-and-compliance

## In scope

- A standalone TypeScript script in `spikes/claude-agent/` with its own package.json
- A scratch git repo the script creates in a temp folder, so no real project is touched

## Out of scope

- Any UI, database, or code under `src/`
- Reading or handling credentials. The SDK uses the developer's existing Claude Code sign-in.

## Acceptance criteria

- [ ] `pnpm start "<prompt>"` starts a session in the scratch repo and prints each event as one JSON line.
- [ ] When the agent wants to edit files or run a command, the script asks y/n in the terminal and honors the answer.
- [ ] `pnpm start --resume <session-id> "<prompt>"` continues the same session, and the agent can recall the first prompt.
- [ ] If Claude Code isn't signed in, the script explains how to sign in and exits cleanly.
- [ ] `docs/research/claude-events.md` lists every event or message type observed, with a short redacted example of each, and shows where usage or cost data appears.
- [ ] The same doc records the date you read the plan-usage article and what it said.

## Verify

Run the spike twice, once new and once resumed, with a real signed-in account. Paste the redacted terminal output into the PR.

## Deliverables

`spikes/claude-agent/`, `docs/research/claude-events.md`
