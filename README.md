# Mothership

Mothership is a desktop app for running AI coding agents in parallel on GitHub work. Pick an issue, pull request, or prompt; Mothership starts a session in its own git worktree; you review the diff and open or merge the pull request.

It drives the official Claude Code, Codex, and GitHub Copilot agents, so you use your own Claude, ChatGPT, and Copilot subscriptions. Mothership never handles your sign-in for those providers.

## Status

Phase 0: repo setup. There's no app to run yet. The stack is Tauri 2 with a React frontend ([ADR 0001](docs/adr/0001-desktop-stack.md)), and the scaffold is the next task.

## Where things are

- [CLAUDE.md](CLAUDE.md): rules, stack, layout, commands, and how to work a task
- [docs/tasks/](docs/tasks/): the task queue, one file per task
- [docs/adr/](docs/adr/): architecture decisions

## License

[MIT](LICENSE)
