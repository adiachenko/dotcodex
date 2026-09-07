---
name: write-user-docs
description: Use when writing, restructuring, or evaluating end-user documentation for a product, package, or API, including deciding what to include or cut and comparing drafts.
---

# Write User Docs

Judge documentation by what the reader can accomplish. Most facts about a system do not belong in its documentation. Keep a sentence only if it changes what the reader does or expects when performing the documented task. Delete the rest. Do not invent edge cases to justify keeping trivia.

- **Document the public contract.** Cover what readers call, configure, run, implement, and observe. Include implementation or lifecycle details only when they affect correct use or explain an observable outcome. Put material limits beside the guarantees they qualify.

- **Assume the intended reader understands the ecosystem's ordinary conventions.** Explain package-specific behavior, required choices, and surprising constraints.

- **Lead with the path most readers should use.** Introduce alternatives where they serve a concrete task or decision. Assume readers know their ecosystem's ordinary conventions, and explain the product-specific concepts and constraints they need.

- **Explain "why" only when it lets the reader predict undocumented behavior.** Rationale that merely praises the design is noise.

- **When behavior has a complex resolution order, teach the intended usage and the one surprising constraint.** Skip the full decision table. Readers who need the full matrix can read the source.

- **Mention a missing capability only when a reader would confidently assume it exists and be harmed by that assumption.** Do not list everything a feature doesn't do.

- **Teach configuration through behavior.** Explain which settings produce the desired effect. Leave out validation rules.

- **Show complete configuration once.** When documenting a variation, show only the setting that changes instead of repeating the surrounding configuration.

- **Keep upgrade guides separate.** Installation and feature pages describe the current contract. Put version-to-version breaking changes and required migration steps in a dedicated upgrade guide. Create or update one only for a transition between released versions or when the user explicitly requests it.

- **State each fact once, where the reader first needs it, and link from everywhere else.** Duplicate statements must be maintained separately and make every page they touch longer.

- **Explain concepts in prose. Use lists for actions the reader performs and options the reader chooses between.** A numbered walkthrough of an internal pipeline is a spec fragment, not documentation.

- **Keep documentation corrections narrowly scoped.** When feedback targets one problem, preserve unaffected wording and examples unless they create a concrete reader problem.

- **Evaluate documentation, including your own, by simulating reader tasks rather than inspecting the text.** Pick concrete tasks such as first success, a routine change, and recovery from a failure. Check that readers can complete each task and count how much they must read. Use these walkthroughs to find both omissions and excess. Coverage, rigor, and familiar style are not quality signals.
