# T-002: Spike: run Codex through the app-server

- **Status:** Ready
- **Depends on:** none. Needs the Codex CLI (`@openai/codex`) installed and signed in with ChatGPT.
- **Specs/ADRs:** docs/adr/0001-desktop-stack.md

## Goal

Prove Mothership can run a Codex session through `codex app-server`: stream its events, answer an approval request, and resume a thread. Record the protocol so we can design the shared event model (ADR 0002).

## Context

- How the app-server works: https://openai.com/index/unlocking-the-codex-harness/
- Protocol source and README: https://github.com/openai/codex/tree/main/codex-rs (see `app-server` and `app-server-protocol`)
- Launch command: `codex app-server --listen stdio://`

## In scope

- A standalone TypeScript script in `spikes/codex-app-server/` with its own package.json. It launches the developer's installed `codex` and talks JSON-RPC over stdio.
- Types for only the messages the spike uses, generated from the protocol schema where possible

## Out of scope

- Any UI, database, or code under `src/`
- Reading Codex's auth files, building an OAuth flow, or calling OpenAI endpoints directly. Codex owns sign-in.

## Acceptance criteria

- [ ] The script initializes the connection and identifies itself with name `mothership`, title `Mothership`, and the spike's version.
- [ ] It checks sign-in state through the protocol's account methods. If signed out, it explains how to sign in and exits cleanly.
- [ ] `pnpm start "<prompt>"` starts a thread in a scratch git repo and prints each server message as one JSON line.
- [ ] When the server asks for approval of a command or file change, the script asks y/n in the terminal and sends the answer back.
- [ ] `pnpm start --resume <thread-id> "<prompt>"` continues the same thread, and the agent can recall the first prompt.
- [ ] `docs/research/codex-events.md` lists every request, response, and notification observed, with a short redacted example of each, and shows where usage data appears.

## Verify

Run the spike twice, once new and once resumed, with a real ChatGPT sign-in. Paste the redacted terminal output into the PR.

## Deliverables

`spikes/codex-app-server/`, `docs/research/codex-events.md`
