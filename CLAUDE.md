# Mothership

Mothership is a desktop app for running AI coding agents in parallel on GitHub work. You pick an issue, pull request, or prompt; Mothership starts a session in its own git worktree; you review the diff and open or merge the pull request. It drives the official Claude Code, Codex, and GitHub Copilot agents, so people use their own Claude, ChatGPT, and Copilot subscriptions.

**Status:** Phase 0 (repo setup). Work only on the task you were given.

## Rules that are never broken

- Never read, copy, store, or forward Claude, ChatGPT, or Copilot credentials, and never build an OAuth flow for any of those providers. Mothership launches the official `claude`, `codex`, and `copilot` programs, and they own sign-in. Never call a provider's model API directly with a user's token. If a task seems to need credentials, stop and ask.
- Never commit secrets, tokens, or `.env` files.
- Don't edit `docs/spec/` or accepted ADRs in `docs/adr/` without asking. Propose changes in your PR description instead.
- No destructive git commands: no force pushes, no rewriting shared history, no deleting branches you didn't create.
- Don't add a dependency without explaining why in the PR description.

## Stack

Decided in `docs/adr/0001-desktop-stack.md`:

- Tauri 2 with a Rust core
- React, TypeScript (strict mode), Vite for the frontend
- A bundled Node sidecar, `agent-host`, in TypeScript, for every agent integration
- SQLite through rusqlite in the Rust core, with numbered migrations
- Vitest and `cargo test` for unit tests, WebdriverIO with `tauri-driver` for end-to-end tests
- pnpm for packages, the Tauri bundler for installers, the Tauri updater plugin for auto-update

## Layout

```text
src/               React frontend
src-tauri/         Rust core: window, git, files, database, GitHub API
  src/git/         worktrees and branches
  src/github/      GitHub API and sign-in for issues and pull requests
  src/db/          schema and migrations
  src/agent_host/  starts and supervises the agent-host sidecar
agent-host/        Node sidecar: AgentSession interface, one adapter per agent
  src/claude/      Claude Agent SDK
  src/codex/       codex app-server
  src/copilot/     Copilot CLI and SDK
shared/            TypeScript types used by the frontend and agent-host (agent events, IPC contracts)
spikes/            throwaway experiments with their own package.json; never imported by src/ or agent-host/
docs/spec/         requirements: numbered stories with acceptance criteria
docs/adr/          architecture decisions
docs/tasks/        the task queue, one file per task
docs/research/     research notes
```

## Commands

The scaffold task (T-000) creates these. Keep this table accurate when scripts change.

| Command | What it does |
| --- | --- |
| `pnpm install` | Install dependencies |
| `pnpm dev` | Run the app in development (`tauri dev`) |
| `pnpm test` | Unit tests (Vitest and `cargo test`) |
| `pnpm test:e2e` | End-to-end tests. Runs on Linux and Windows; `tauri-driver` doesn't support macOS. |
| `pnpm lint` | ESLint, Prettier, `cargo fmt --check`, and `cargo clippy` |
| `pnpm typecheck` | TypeScript check |
| `pnpm build` | Build the sidecar and an installer for the current OS |
| `pnpm verify` | Lint, typecheck, unit, and end-to-end tests. Must pass before every PR. |

## How to work a task

1. Read the task file in `docs/tasks/` and every spec or ADR it links to.
2. Write or extend tests from the acceptance criteria first, and confirm they fail.
3. Implement until `pnpm verify` passes.
4. Open a PR titled `T-001: short summary`. In the description, map each acceptance criterion to the change that meets it, and list anything left undone.
5. If the task is unclear or blocked, stop and write your questions in the PR instead of guessing.

## Code conventions

- Privileged work (files, git, processes, network) happens only in the Rust core or `agent-host`. The frontend calls Tauri commands and listens for Tauri events, typed through `shared/`.
- The core and `agent-host` talk over stdio with newline-delimited JSON.
- Grant the webview only the Tauri capabilities it needs. Never give it shell, fs, or http plugin permissions, and keep a strict CSP.
- Every agent integration goes through the `AgentSession` interface in `agent-host`. The frontend and the Rust core never talk to an agent directly.
- Treat every repository Mothership opens as untrusted. Never run its scripts or apply its config without the user's approval.
- Keep PRs small and focused, ideally under 400 changed lines.
- Commit messages follow Conventional Commits (`feat:`, `fix:`, `docs:`, `test:`, `chore:`).

## Notes for Claude Code

- For changes to `agent-host/` or the database schema, use plan mode and get the plan approved before editing.
- For anything involving Claude Code or the Claude Agent SDK, read the current docs (https://code.claude.com/docs/llms.txt) rather than relying on memory. The SDK changes often.
