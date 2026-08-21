---
name: write-code
description: Use when writing or reviewing code, including tests. Do not use for planning, explaining, or diagnosing unless the task also changes the implementation. Also use when the user says "unweave" or asks to simplify an existing implementation.
---

# Write Code

## Restraint in Implementation

Reason broadly while planning, including material risks and edge cases. Apply this restraint only to implementation.

**Prefer the smallest clear implementation that is correct for supported, realistic use.** Treat defensive code as overengineering when it rechecks a guarantee already established by earlier code, protects internal code from unsupported use, or makes no difference to required behavior.

Make one part of the code responsible for ensuring each guarantee is true. Check external or otherwise untrusted data when it first enters that part, then let later code rely on the guarantee instead of checking it again. Add a guard or other special handling only when supported behavior requires an outcome different from what would happen without it.

Apply the same restraint to structure: keep a class, interface, or layer of indirection only when it is needed to preserve supported behavior, isolates an actual boundary, satisfies a required property, or makes the implementation clearer than the direct alternative.

**Before finalizing, remove defensive code** that neither performs the initial validation of untrusted input nor implements required behavior, together with tests that exist only for that code.

## Unweave

When the user invokes unweave or asks to simplify an existing implementation, establish supported behavior and authoritative constraints from the request, documentation, recorded agreements and specifications, tests, and actual uses. Do not infer a contract from how the current code is organized. Within the requested scope, choose the simplest clear design that satisfies that behavior and those constraints. Prefer deletion, inlining, merging, or a fitting existing convention over adding replacement code or structure. Finish only when everything left in scope meets the structural rules above. Do not merge readable methods merely to reduce indirection or method count. Do not split a cohesive class to satisfy a formal reading of SRP. Verify the result against the supported behavior and every affected constraint. Revise or remove tests whose assertions encode only discarded implementation details or invented behavior.

## Control Flow

- Prefer guard clauses and straight-line control flow; keep nesting shallow.
- Use separate operations instead of boolean parameters that select distinct behaviors.

## Method Complexity

Methods should have one cohesive purpose and operate at a consistent level of abstraction. The method body should use the vocabulary of that purpose. Extract a cohesive part when its implementation requires the reader to change context or reason at a different level than the surrounding method. This applies even if the method is short or the extracted method is used once. Name extracted methods for the role, outcome, or guarantee they contribute to their caller.

Treat ~20 lines of logic as a review signal, not a threshold: shorter methods may still mix purposes or abstraction levels.

**Other signs a method may mix abstraction levels:**

- Nested closures or callbacks with their own branching
- Multiple try/catch blocks or catch-and-retry patterns
- Representation, protocol, or data-transformation details mixed with higher-level policy or orchestration

## PHP

- Do not declare a PHP class `final` unless it was already `final` before the edit.
- Use `readonly` only where preventing reassignment is a design requirement.
- Call global PHP functions directly; do not import them with `use function`.
- Inline single-use locals when the resulting expression remains clear.
- Use docblocks only when they add information native PHP declarations cannot express.
- Add type declarations wherever compatible with PHP and framework contracts and conventions.
- Treat acronyms as words (`HttpClient`, not `HTTPClient`) unless relevant comparable identifiers consistently use different casing.
- Prefer string interpolation to `sprintf()` and concatenation. Omit braces around interpolated variables unless required.
