---
name: write-user-docs
description: Use when writing, restructuring, or evaluating end-user documentation for a product, package, or API, including deciding what to include or cut, comparing drafts, and revising documentation as the product changes.
---

# Write User Docs

Judge documentation by what a reader can do after reading it, not by how much of the system it describes. These rules govern the wording, not the scope of an edit. When other guidance limits that scope, apply these rules to whatever the edit adds.

- **Document the contract, not the implementation.** Cover only what users call, configure, run, implement, and observe. Never name internal classes, jobs, middleware, or lifecycle details. Documenting an internal detail turns it into part of the API and encourages readers to depend on it.

- **Teach configuration through behavior, not validation.** Tell readers which change produces the desired effect, not which values pass validation.

- **Omit internal sequencing that does not change what the reader does or expects.**

- **Keep a sentence only if it changes what the reader does or expects.** Truth alone is not a reason to include it. Most true facts about a system must be left out. If deleting the sentence would not alter any reader's action, delete it.

- **When behavior has a complex resolution order, teach the intended usage and the one surprising constraint.** Skip the full decision table. Readers who need the full matrix can read the source.

- **State each fact once, where the reader first needs it, and link from everywhere else.** Duplicate statements must be maintained separately and make every page they touch longer.

- **Explain "why" only when it lets the reader predict undocumented behavior.** Keep rationale that deepens that understanding. Rationale that merely praises the design is noise.

- **Mention a missing capability only when a reader would confidently assume it exists and be harmed by that assumption.** Do not list everything a feature doesn't do.

- **Explain concepts in prose. Use lists for actions the reader performs and options the reader chooses between.** A numbered walkthrough of an internal pipeline is a spec fragment, not documentation.

- **Evaluate documentation, including your own, by simulating reader tasks rather than inspecting the text.** Pick concrete tasks such as first success, a routine change, and recovery from a failure. Check that readers can complete each task and count how much they must read. Coverage, rigor, and familiar style are not quality signals. Completeness metrics always favor the longest docs, and your own style will always seem clearest to you.

- **Keep documentation corrections narrowly scoped.** When feedback targets one problem, preserve unaffected wording and examples unless they create a concrete reader problem.

- **Show complete configuration once.** When documenting a variation, show only the setting that changes instead of repeating the surrounding configuration.

- **Lead with the path most readers should use.** Include supported alternatives only when they serve a concrete reader task, and introduce them where the choice becomes relevant.

- **Assume the intended reader understands the ecosystem's ordinary conventions.** Explain package-specific behavior, required choices, and surprising constraints.

- **Keep upgrade guides separate.** Installation and feature pages describe the current contract. Put version-to-version breaking changes and required migration steps in a dedicated upgrade guide. Create or update one only for a transition between released versions or when the user explicitly requests it.

The goal is signal, not brevity. Cutting a passage that changes what a reader does is just as harmful as keeping one that does not.
