# dotcodex

This repository shares foundational, coding-focused parts of my Codex setup. 

Automations and some of the more general-purpose skills I use day to day are not included.

## Contents

- **`instructions/SYSTEM.custom.md`** is the system prompt I use to customize model behavior. `instructions/SYSTEM.default.md` is included solely as a baseline for comparison (use the git diff command below).
- `instructions/AGENTS.md` is for more personalized rules and gotchas. 
  - The “**User Memory**” it references is not included because it contains personal context rather than reusable setup. I curate memories manually so I can decide exactly what is retained and loaded into context.
- `skills/` contains my shared skills.
- `config.toml` has the bits of my Codex config that seemed worth sharing.

## Related projects

These projects are also part of this setup but live in dedicated repositories:

- ⛳️ [Questline](https://github.com/adiachenko/questline) provides a workflow for advancing software goals one bounded, agreed, and verified deliverable at a time.
- 🪏 [Knotbane](https://github.com/adiachenko/knotbane) finds cyclomatic complexity hotspots in PHP and includes an agent skill for simplifying them.
- I also have agent-ready Laravel starters for [applications](https://github.com/adiachenko/starter-kit-laravel) and [packages](https://github.com/adiachenko/skeleton-laravel).

## View the system prompt changes

```sh
git diff --no-index -- instructions/SYSTEM.default.md instructions/SYSTEM.custom.md
```
