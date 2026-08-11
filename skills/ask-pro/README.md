# Ask Pro Skill

`ask-pro` is a Codex skill for getting a second opinion from ChatGPT Pro through the web UI using the Chrome plugin and the user's existing signed-in browser session.

Use it when local investigation is already underway but a second opinion would genuinely help: architecture tradeoffs, stubborn debugging, cross-checking a planned fix, or getting unstuck on a messy engineering problem without dumping the whole repo into the prompt.

Invoke it by saying `ask pro` or explicitly using `$ask-pro`.

## What It Does

1. Opens ChatGPT through Chrome
2. Selects ChatGPT's current `Pro` mode
3. Builds and submits a focused prompt bundle with project briefing, conventions, constraints, prior attempts, and relevant code excerpts
4. Keeps long-running Pro sessions alive without premature teardown
5. Treats extra Pro turns as user-owned, not autonomous

See [`SKILL.md`](./SKILL.md) for the full operating details and guardrails.

## Requirements

- Codex
- Chrome plugin enabled
- ChatGPT account with Pro access

## Install

Simply ask Codex to install this skill globally from this repository.
