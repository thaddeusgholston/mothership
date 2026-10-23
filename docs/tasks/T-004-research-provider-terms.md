# T-004: Research: provider rules for subscription use

- **Status:** Ready
- **Depends on:** none
- **Specs/ADRs:** none yet

## Goal

Write down what Anthropic, OpenAI, and GitHub currently allow for an app like Mothership, so we build within the rules. The owner must review this document before anyone relies on it.

## Context

- Anthropic credential rules: https://code.claude.com/docs/en/legal-and-compliance
- Anthropic plan usage with the Agent SDK: https://support.claude.com/en/articles/15036540-use-the-claude-agent-sdk-with-your-claude-plan
- OpenAI, Codex app-server with ChatGPT plans: https://developers.openai.com/siwc/token-sharing-open-source/codex-app-server
- OpenAI, app-server background: https://openai.com/index/unlocking-the-codex-harness/
- GitHub: find the current terms for the Copilot CLI and Copilot SDK when used by a third-party app, including how premium requests are counted. Link what you find.

## Acceptance criteria

- [ ] `docs/research/provider-terms.md` covers, for each provider: how subscription sign-in may be used, what third-party apps may and may not do, and how usage is billed or limited.
- [ ] Every claim links to its source and shows the date it was checked.
- [ ] A "What this means for Mothership" section lists concrete do's and don'ts for the codebase.
- [ ] Anything ambiguous is listed as an open question instead of guessed.
- [ ] The doc ends with an unchecked "Reviewed by owner" box, and the PR doesn't merge until the owner checks it.

## Deliverables

`docs/research/provider-terms.md`
