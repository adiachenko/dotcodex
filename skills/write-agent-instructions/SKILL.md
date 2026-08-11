---
name: write-agent-instructions
description: "Write, edit, review, or discuss improvements to agent instructions, including prompts, automations, skills, system prompts, `AGENTS.md`/`CLAUDE.md` rules, and other text that steers an AI agent. Use even when the user is only evaluating or drafting possible instruction wording. Also use when the user says “runefold”."
---

# Write Agent Instructions

Steering text succeeds by leverage per word. Keep only rules whose absence would materially threaten correctness or explicit user expectations. Prefer the durable invariant over a mechanism the agent can choose at runtime.

When the user asks to “runefold” instructions, preserve their intended behavior while aggressively removing machinery and details the target agent can derive or retrieve at runtime.

## Process

Each completion criterion must distinguish done from not-done. Where partial work is a risk, make it exhaustive ("every X accounted for") rather than gestural ("produce a list of X").

1. Establish the instruction surface, owner, and provenance. Leave generated or provider text unchanged unless the user asked to edit it.
2. Isolate the smallest behavior change the agent could not reliably derive from the request, existing instructions, environment, tools, or authoritative sources. Edit the existing owner of that behavior; add a rule only when none exists.
3. Draft one direct version in the surrounding voice. Remove derivable or retrievable detail, runtime mechanics, invented constraints, and alternate variants the user did not request.
4. Reread the result as standalone agent input. Decontextualize edit-history scaffolding, audit every sentence and mixed clause by counterfactual removal, and verify the intended meaning, scope, emphasis, and completion bar.

When discussing proposed changes to existing instructions, reproduce in the response the smallest exact current passage needed to evaluate each change alongside the exact proposed wording or an explicit indication that it is deleted; for insertions, quote neighboring text as the anchor, and reuse shared context across related changes.

## Provenance

Identify whether guidance is project-owned, user-owned, generated, or provider-supplied, and preserve that boundary in both wording and file edits.

For skills, treat the description as the activation surface: include only the user-visible tasks, contextual cues, and necessary scope boundaries that determine whether the skill should activate.

For automations, future-run behavior belongs in the automation prompt. Apply user feedback that should affect future runs to that prompt, and reserve automation memory for run history and changing context.

When an authoritative source exists, record only the stable user intent, preferences, and principles that would improve future reasoning, plus where to find that source. Leave exact, mutable, or readily retrievable details in the source itself.

When one instruction invokes another, keep each requirement with its owner.

Do not move project policy into provider-supplied skills. Put project-specific policy in a project-owned skill or guideline, and reference provider mechanics generically unless the user asks for a provider-specific surface.

Do not make project-owned instructions directly depend on non-owned skills, generated guidance, or provider-specific file layouts. If those resources help, keep the project-owned instruction complete without them and refer generically to installed skills or provider documentation.

## Voice and Shape

When adapting user-provided content, preserve wording that already works. Improve fit, clarity, and usefulness without changing core meaning, scope, or emphasis unless asked or clearly necessary.

Match the surrounding instruction voice and perspective. For agent-facing rules, refer to the human as "the user" unless the document already uses another convention. Use first person only for quoted user examples or text meant to be spoken by the user.

State the chosen behavior directly. Do not preserve discarded alternatives as defensive negative instructions such as "do not add X" or "do not use Y". Keep negative wording only when the prohibition is itself the durable requirement, not because an alternative was considered and rejected during drafting.

When drafting a goal for editing agent instructions, express preservation through review and completion criteria rather than restating the source prompt, unless the user explicitly wants an audit checklist. A prompt for a Codex goal must explicitly tell Codex to create or start the goal; an objective that only sounds goal-like is not enough.

When a prompt embeds expected input, place that input at the top or bottom instead of interleaving it with instructions.

## Derivability and Constraint Audit

Assume the target agent can reason from the request, surrounding instructions, environment, tools, and authoritative sources. Delete anything it can reliably derive or retrieve without changing correctness or explicit user expectations. Commands, variables, paths, mutable facts, routine investigation or safety steps, standard reporting behavior, and restatements of higher-priority guidance usually fail this test. Keep a mechanism only when the user wants it as durable behavior or evidence shows that outcome-oriented guidance is insufficient.

For every sentence—and every clause in a mixed sentence—ask what concrete failure would become materially more likely if it disappeared. Delete it when there is no specific answer; truth, low cost, or protection against a hypothetical mistake is not enough. Before making chat-derived wording durable, read it as a future agent who sees only the final artifact and remove migration notes, edit rationale, or comparisons that depend on the current conversation.

Do not invent counts, ranges, section orders, fixed headings or formats, checklist steps, exhaustive edge cases, examples, style micro-rules, qualifiers, or rationale unless the user asked for them or correctness depends on them. Delete a failed rule instead of softening it; split a mixed sentence only to preserve a justified rule.
