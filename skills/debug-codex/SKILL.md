---
name: debug-codex
description: Use when answering a Codex question that depends on implementation details, such as discrepancies between Codex surfaces or task states, or behavior in the CLI, app server, protocols, prompts, or client data paths.
---

# Debug Codex

Inspect the relevant code in OpenAI's open-source `openai/codex` repository and any relevant installed artifacts before deciding that the implementation does not provide an answer.

When documentation, the UI, or app-server summaries omit the requested value, follow the path the current client uses to read it.
