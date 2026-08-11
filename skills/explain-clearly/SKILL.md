---
name: explain-clearly
description: Use for explanatory prose when the user needs reasoning, background, analysis, comparison, or discussion to understand a response, including within implementation or deliverable-producing work. Apply only to the explanation, not to the deliverable itself, routine progress updates, or simple factual answers.
---

# Explain Clearly

Shape explanatory prose so the user understands the reasoning on the first read, without having to reconstruct missing steps.

## Scope

Apply this style only to explanatory prose, including within tasks that produce or edit a deliverable. Do not apply it to the deliverable itself; routine progress updates, notes about your own work, and simple factual answers keep the default brevity.

## Explanation Standard

Optimize for clarity, completeness, and ease of understanding rather than minimum length. Completeness means the reader can follow the reasoning without reconstructing missing context; it does not mean repeating the same point in several forms.

When writing explanatory prose:

1. Choose the explanation path that fits the user's apparent knowledge and is easiest for them to follow, then draft one clear line of reasoning along it.
2. Give each paragraph or bullet a distinct job.
3. Add background, definitions, assumptions, cause and effect, caveats, examples, or tradeoffs only where they fill a specific gap — and place them where the reader needs them, not as a second explanation elsewhere.
4. Answer each reader question exactly once: merge or delete passages that answer the same question, and refer back to earlier concepts with brief anchors instead of re-explaining them, unless the new section adds a new role, consequence, caveat, or distinction.
5. Stop expanding when the answer is self-contained, easy to follow, and non-repetitive.

Do not decide explanation length from the size of the patch, diff, command, or conclusion. Decide from how much reasoning the reader would otherwise have to reconstruct.

## Voice

Write in plain, practical language, like a competent engineer explaining something in chat. Treat a sentence made mostly of abstract labels as unfinished: rewrite it to say who or what does what and what happens as a result. Keep technical terms when they add precision, but do not use a label as a summary that the next sentence has to translate. Before sending, remove wording that sounds sophisticated without adding information.

Do not turn explanations into source maps: keep file paths, symbols, and line references sparse, and include them only when they materially support a specific claim.
