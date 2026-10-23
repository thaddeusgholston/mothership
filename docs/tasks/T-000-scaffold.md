# T-000: Scaffold the repository

- **Status:** Ready
- **Depends on:** ADR 0001
- **Specs/ADRs:** docs/adr/0001-desktop-stack.md

## Goal

Create an empty but fully wired Mothership app, so every later task starts from a passing `pnpm verify`.

## Context

Follow the stack, layout, and commands in CLAUDE.md exactly. If something in CLAUDE.md doesn't work in practice, fix CLAUDE.md in the same PR and explain why.

## In scope

- A Tauri 2 app with a React + TypeScript frontend built by Vite, using the folder layout in CLAUDE.md
- A stub `agent-host` sidecar that answers a `ping` message over stdio, bundled through Tauri's `externalBin`, with the Rust core starting it on launch
- The pnpm scripts listed in CLAUDE.md
- One Vitest test, one `cargo test` test, and one end-to-end test that launches the app and checks the window title
- CI that runs `pnpm verify` on every pull request on Linux and Windows, and lint, typecheck, and unit tests on macOS
- A pre-commit hook that runs lint and typecheck
- Tauri bundler config for unsigned macOS, Windows, and Linux builds
- Empty `docs/spec/` and `docs/research/` folders, each with a `.gitkeep`

## Out of scope

- Any product features, agent adapters, database tables, or GitHub integration
- Code signing and auto-update (a later task)

## Acceptance criteria

- [ ] `pnpm dev` opens a window titled "Mothership".
- [ ] `pnpm verify` passes locally and in CI.
- [ ] The webview's capabilities grant no shell, fs, or http plugin permissions, the CSP blocks remote scripts, and a test checks both.
- [ ] On launch, the Rust core starts `agent-host`, gets a reply to `ping`, and logs it, in both `pnpm dev` and a built installer.
- [ ] `pnpm build` produces an installer for the current OS, with the sidecar inside it.
- [ ] The command table in CLAUDE.md matches the real scripts.

## Verify

Run `pnpm install && pnpm verify` on a clean clone, then `pnpm dev`. Paste the output into the PR.

## Deliverables

The app skeleton under `src/`, `src-tauri/`, `agent-host/`, and `shared/`, `package.json`, the CI workflow, Tauri bundler config, and CLAUDE.md updates if needed.
