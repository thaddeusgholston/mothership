# ADR 0001: Desktop stack

- **Status:** Accepted (2026-10-22)
- **Date:** 2026-10-22 (revised from Electron, 2026-10-10)

## Decision

Build Mothership with Tauri 2 and a React + TypeScript frontend built by Vite. A Rust core owns the window, git, files, and SQLite. A bundled Node sidecar, `agent-host`, runs every agent integration in TypeScript.

## Context

- Mothership drives three agents: Claude Code through the Claude Agent SDK (`@anthropic-ai/claude-agent-sdk`, a TypeScript library), Codex through `codex app-server` (JSON-RPC over stdio), and GitHub Copilot through the official Copilot CLI and its SDK.
- The Agent SDK only runs in a JavaScript runtime, and a Tauri backend is Rust, so the SDK needs a separate Node process.
- Putting every agent adapter in that one sidecar keeps the `AgentSession` interface in a single language, so adding an agent means adding an adapter there. The T-001, T-002, and T-005 spikes are TypeScript, so they carry over directly.
- The reference app, the GitHub Copilot app, uses Tauri 2 with React.

## Consequences

- Three parts instead of one: the React frontend, the Rust core, and the `agent-host` sidecar. The frontend talks to the core through Tauri commands and events. The core talks to `agent-host` over stdio with newline-delimited JSON.
- The toolchain adds Rust (cargo, clippy, rustfmt) alongside Node and pnpm.
- The sidecar ships as a standalone executable per target platform through Tauri's `externalBin`. T-000 proves this with a stub sidecar before any agent code exists.
- Installers are smaller and memory use is lower than an Electron app.
- Each platform's own webview is used (WebKit on macOS, WebView2 on Windows, WebKitGTK on Linux). The Copilot app's changelog shows a trail of fixes for these (WebKitGTK on Linux, Wayland launch crashes, WebView2 crash recovery on Windows), so UI bugs may only reproduce on one OS.
- End-to-end tests run through `tauri-driver` (WebDriver), which supports Linux and Windows but not macOS. macOS gets unit tests plus manual checks.

## Alternatives considered

- **Electron, with the Agent SDK in the main process.** No sidecar and one language, but larger installers, higher memory use, and native modules that must be rebuilt for Electron's Node version.
- **Tauri with the Codex adapter in Rust and a sidecar only for Claude.** This splits `AgentSession` across two languages.
- **Tauri with no sidecar, driving the `claude` CLI's stream-json output from Rust.** This skips the Agent SDK, so tool approvals and resume would have to be rebuilt on the raw CLI protocol.
