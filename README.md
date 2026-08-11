# dotcodex

Foundational, coding-focused parts of my Codex setup.

Automations and some of the more general-purpose skills are not included.

## Contents

- **`instructions/SYSTEM.custom.md`** is the system prompt I use to customize model behavior. 
  - `instructions/SYSTEM.default.md` is included as a baseline for comparison (see [the diff](https://github.com/adiachenko/dotcodex#view-the-system-prompt-changes) below).
- `instructions/AGENTS.md` is for more personalized rules and gotchas. 
  - The "**User Memory**" it references is not included because it contains personal context. I curate memories manually so I can decide exactly what is retained and loaded into context.
- `skills/` contains my shared skills.
- `config.toml` has the bits of my Codex config that seemed worth sharing.

## Related projects

These projects are also part of this setup but live in dedicated repositories:

- ⛳️ [Questline](https://github.com/adiachenko/questline) provides a workflow for developing software one bounded, reviewable deliverable at a time.
- 🪏 [Knotbane](https://github.com/adiachenko/knotbane) finds and simplifies cyclomatic complexity hotspots in PHP.
- I also have agent-ready Laravel starters for [applications](https://github.com/adiachenko/starter-kit-laravel) and [packages](https://github.com/adiachenko/skeleton-laravel).

## View the system prompt changes

```sh
git diff --no-index -- instructions/SYSTEM.default.md instructions/SYSTEM.custom.md
```
