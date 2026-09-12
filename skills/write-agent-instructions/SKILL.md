---
name: write-agent-instructions
description: "Use when the user asks to write, edit, review, evaluate, or discuss prompts, automations, skills, system prompts, `AGENTS.md`/`CLAUDE.md` rules, or other text that steers an AI agent, including when the user says `runefold`."
---

# Write Agent Instructions

**Oversteering is the cardinal sin of instruction writing**. Minimize the decisions imposed on the agent while preserving the user's intended outcome and stated requirements. Do not promote inferred execution choices into requirements. Keep only what the target agent cannot reliably infer or retrieve. 

When the user asks to “runefold” instructions, treat it as feedback that they still oversteer or use more words than necessary.

When changing existing instructions, weigh the risk of reopening a failure they address against the flexibility gained. Shared intent alone does not make a broad principle an equivalent replacement.

Before proposing instructions, classify the requirement as one-off, mechanically enforceable, or judgment-dependent. Do not turn one-offs into standing rules. When a recurring mechanical failure can be prevented by a low-maintenance code, test, or tool change, recommend that owner instead.

## Skill `description` Is a Trigger

For skills, treat `description` as a badly named field: it is a routing predicate wearing a prose label. Its only job is to answer, "Should this skill load for this request?" Write it as `Use when ...` and include only the user requests, vocabulary, contextual conditions, and scope boundaries needed to answer that question. If a clause explains what the skill does after it loads, that clause belongs in the skill body. Configure `allow_implicit_invocation` for explicit-only skills and keep explicit invocation syntax out of descriptions.

For the user's personal Codex skills, keep `agents/openai.yaml` only for requested interface metadata, non-default invocation policy, or tool dependencies. Omit default settings. Disable implicit invocation only at the user's explicit request, never because a workflow requires authorization.

## Source and Ownership

- Identify whether each instruction is project-owned, user-owned, generated from other sources, or provider-supplied, and preserve that boundary in both wording and file edits. Keep project instructions usable without a particular agent’s installed skills or file layout.
- For automations, apply user feedback about future behavior to the prompt, not memory.
- When an authoritative source exists, keep stable user guidance and refer to that source for retrievable details.

## Voice and Structure

- When adapting user-provided content, preserve wording that already works. Improve fit, clarity, and usefulness without changing core meaning, scope, or emphasis unless asked or clearly necessary.
- Match the surrounding instruction voice and perspective. For agent-facing rules, refer to the human as "the user" unless the document already uses another convention. Use first person only for quoted user examples or text meant to be spoken by the user.
- When a prompt embeds expected input, place that input at the top or bottom instead of interleaving it with instructions.
