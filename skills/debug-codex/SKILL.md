---
name: debug-codex
description: Use when the user asks a Codex question whose answer depends on implementation details, such as discrepancies across Codex surfaces or task states, or the behavior of the CLI, app server, protocols, prompts, or client data paths.
---

# Debug Codex

Inspect the relevant code in OpenAI's open-source `openai/codex` repository and any pertinent installed artifacts before concluding that an implementation-dependent answer is unavailable.

When documentation, UI, or app-server summaries omit the requested value, trace the current client read path.
