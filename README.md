# dotcodex

My Codex setup, just the coding parts.

## Contents

- `instructions/SYSTEM.custom.md` is the system prompt I use to customize model behavior. The default prompt casts Codex as an eager-to-please old friend. Most of my changes make that friend think through what each choice buys against what it costs, then use the simplest approach that works.
- `instructions/SYSTEM.default.md` is included as a baseline for comparison. [View the system prompt changes](#view-the-system-prompt-changes).
- `instructions/AGENTS.md` is for more personalized rules and gotchas. The "**User Memory**" it references is not included because it contains personal context, and it's unrelated to coding anyway. I curate memories manually so I can decide exactly what is retained and loaded into context.
- `skills/` contains only the skills I use for coding and related work.
- `config.toml` has the bits of my Codex config that seemed worth sharing.

_Automations are too specific to my workflows and environment, so they're not included here._

## Related projects

These projects are also part of this setup but live in dedicated repositories:

- ⛳️ [Questline](https://github.com/adiachenko/questline) provides a workflow for developing software one bounded, reviewable deliverable at a time.
- 🪏 [Knotbane](https://github.com/adiachenko/knotbane) finds and simplifies cyclomatic complexity hotspots in PHP code.
- I also have agent-ready Laravel starters for [applications](https://github.com/adiachenko/starter-laravel-app) and [packages](https://github.com/adiachenko/starter-laravel-package).

## Automatic skill invocation

My skills are designed to be selected automatically from the request, not treated as a command palette. Explicitly invoking a skill remains available, but it is an override rather than the usual interface. Even workflows like Questline can be driven using special vocabulary like "waypoint" and "compass" rather than slash commands.

## View the system prompt changes

```sh
git diff --no-index -- instructions/SYSTEM.default.md instructions/SYSTEM.custom.md
```
