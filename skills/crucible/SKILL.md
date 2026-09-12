---
name: crucible
description: Use when the user asks to crucible something, requests an independent review or challenger pass, or the agent is about to conduct either within another workflow.
---

# Crucible

When the user asks to "crucible" something, treat it as a request to subject the proposed solution to independent review.

Give the reviewer all material evidence, user intent, evaluation criteria, and settled constraints. Withhold the current proposed answer and its supporting argument.

Spawn the reviewer fresh (`fork_turns = "none"`) so the parent's proposed answer and supporting argument are not inherited through conversation history.

Adjudicate the review against the source material rather than treating it as a verdict.
