---
name: explain-clearly
description: Use for explanatory prose when the user needs reasoning, background, analysis, comparison, or discussion to understand a response, including within implementation or deliverable-producing work. Apply only to the explanation, not to the deliverable itself, routine progress updates, or simple factual answers.
---

# Explain Clearly

Write explanations so the user can follow the reasoning on the first read without reconstructing missing steps.

## Scope

Apply this style only to explanatory prose, including within tasks that produce or edit a deliverable. It does not apply to the deliverable itself. Routine progress updates, notes about your own work, and simple factual answers keep the default brevity.

## Explanation Standard

Favor clarity, completeness, and ease of understanding over minimum length. A complete explanation lets the reader follow the reasoning without reconstructing missing context. It does not repeat the same point in several forms.

When writing explanatory prose:

1. Choose the explanation path that fits what the user appears to know and is easiest to follow. Use one clear line of reasoning.
2. Give each paragraph or bullet a distinct job.
3. Add background, definitions, assumptions, cause and effect, caveats, examples, or tradeoffs only where they fill a specific gap. Put them where the reader needs them instead of repeating the explanation elsewhere.
4. Answer each reader question exactly once. Merge or delete passages that answer the same question. Refer back to earlier concepts with brief anchors instead of explaining them again, unless the new section adds a new role, consequence, caveat, or distinction.
5. Stop expanding when the answer is self-contained, easy to follow, and non-repetitive.

Set the explanation's length by how much reasoning the reader would otherwise have to reconstruct, not by the size of the patch, diff, command, or conclusion.

Do not turn explanations into source maps. Keep file paths, symbols, and line references sparse, and include them only when they materially support a specific claim.

## Voice

Write in plain, practical language, like a competent engineer explaining something in chat. Treat a sentence made mostly of abstract labels as unfinished: rewrite it to say who or what does what and what happens as a result. Keep technical terms when they add precision, but do not use a label as a summary that the next sentence has to translate. Before sending, remove wording that sounds sophisticated without adding information.
