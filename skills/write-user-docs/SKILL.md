---
name: write-user-docs
description: Use when writing, restructuring, or evaluating end-user documentation for a product, package, or API, including deciding what to include or cut, comparing drafts, and revising documentation as the product changes.
---

# Write User Docs

Documentation is judged by what a reader can do after reading it, not by how much of the system it describes. These rules govern the words you write, not how much you may change; where other guidance constrains an edit's scope, apply them to whatever that edit adds.

- **Document the contract, not the implementation.** Cover only surfaces the user touches: what they call, configure, run, implement, and observe. Never name internal classes, jobs, middleware, or lifecycle mechanics — documenting an internal turns it into API and invites coupling to it.

- **Omit internal sequencing that does not change what the reader does or expects.**

- **Admit a sentence only if it changes what the reader does or expects.** Being true is not a reason to include; most true facts about a system must be left out. If deleting the sentence would not alter any reader's action, delete it.

- **When behavior has an intricate resolution order, teach the intended usage and the one surprising constraint** — not the full decision table. Readers who need the matrix are reading the source anyway.

- **State each fact once, where the reader first needs it, and link from everywhere else.** Duplicated statements fork maintenance and bloat every page they touch.

- **Explain "why" only when it lets the reader predict undocumented behavior.** Rationale that extends the reader's model earns its place; rationale that admires the design is noise.

- **State a non-capability only where a reader would confidently assume otherwise and be harmed.** Do not enumerate what a feature doesn't do.

- **Concepts get prose; lists are for actions the reader performs and options the reader chooses between.** A numbered walkthrough of an internal pipeline is a spec fragment, not documentation.

- **Evaluate docs — anyone's, including your own — by simulating reader tasks, never by inspecting the text.** Pick concrete tasks (first success, a routine change, a failure recovery), check each succeeds, and count how much reading it required. Coverage, rigor, and stylistic familiarity are not quality signals; completeness metrics always crown the longest docs, and your own style always reads as the clearest.

- **Keep documentation corrections narrowly scoped.** When feedback targets one problem, preserve unaffected wording and examples unless they create a concrete reader problem.

- **Show complete configuration once.** When documenting a variation, show only the setting that changes instead of repeating the surrounding configuration.

- **Lead with the path most readers should use.** Include supported alternatives only when they serve a concrete reader task, and introduce them where the choice becomes relevant.

- **Assume the intended reader understands the ecosystem's ordinary conventions.** Explain package-specific behavior, required choices, and surprising constraints.

- **Keep upgrade guides separate.** Installation and feature pages describe the current contract. Put version-to-version breaking changes and required migration steps in a dedicated upgrade guide. Create or update one only for a transition between released versions or when the user explicitly requests it.

The target is signal, not brevity. Cutting a passage that changes what a reader does is the same failure as keeping one that does not.
