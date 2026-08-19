---
name: design-tests
description: Use when planning, creating, editing, fixing, organizing, or reviewing tests or test coverage. This includes interpreting mutation-testing results; choosing test boundaries, scenarios, test data, mocks, or assertions; and removing redundant checks or checks that mirror the implementation. Use alongside project- or framework-specific testing skills.
---

# Design Tests

Write tests as executable examples for maintainers. Make each test a readable, coherent demonstration of one supported behavior, not a spell checker for the production code.

One behavior does not mean one assertion or an artificially small test. Use every observation needed to establish one rule, transition, or outcome. Split rules that can change independently or need different explanations when they fail.

## Plan from behavior and risk

- For behavior changes and bug fixes, prefer starting with a failing test that captures the expected outcome, then implement the change.
- Prioritize by the likelihood and impact of a regression, not by how easy a case is to generate or assert.
- Do not test language or framework behavior by itself. Test project-specific integration only when the wiring is a material risk.
- Do not create unsupported internal states merely to exercise defensive branches. Test invalid input where it crosses a supported boundary and the response is part of the contract.

## Work from the outside in

- Choose the test level by the behavior's owner, not by how much framework infrastructure the test boots. Prefer a supported public boundary when it exposes the rule; otherwise test the owning component. Assert observable outcomes rather than how internal objects interact.
- Give each guarantee one primary proof across the suite. Public-boundary and lower-level proofs are redundant only when the same plausible incorrect implementation would make both fail.
- Use lower-level tests mainly for concentrated logic, boundary cases, or contract details that would make the public-boundary test too long or obscure its main story.
- Lower-level tests written during implementation may be temporary. Before finalizing, remove or consolidate any made redundant by a clearer public proof.
- Prefer the project's existing classes and exercise the real application flow. Use a mock only when the dependency performs external I/O, represents a supported extension point, has behavior that is already proved more directly elsewhere, or is impractical to use in the test. Do not mock internal steps merely to assert which methods were called.
- Keep background setup realistically valid and executable, including dependencies the tested path does not exercise. Minimize test-only fixtures across the suite: keep each only when a distinct proof requires it, and reuse it across compatible scenarios.
- If test doubles proliferate, reconsider the test boundary or production design.
- Pin a representation exactly only in the test where that exact representation is part of the contract. Make other tests assert its meaning instead of repeating the full representation.

## Build a discriminating scenario

- Use the smallest deterministic scenario that makes plausible incorrect implementations visibly fail.
- Build rejection and validation cases from a known-valid scenario. Change only the facts needed to cross the rejected boundary. Prove that the intended rule caused the failure, not an unrelated invalid precondition.
- Keep materially different rules in separately named tests.
- Avoid parameterized tests and datasets by default. Use them only for a small set of genuinely equivalent inputs when the table is easier to understand and maintain than separate tests.

## Keep the proof visible

- Keep every decisive identity, relationship, state, value, and time visible at the test call site. Let fixtures, factories, and helpers hide background details, but not the condition that selects the behavior or the outcome being proved.
- Use deterministic, mentally checkable data. Choose simple arithmetic, recognizable identities, and deliberately different before-and-after values. Control time when time affects the rule; never randomly select the business scenario.
- Capture created objects and use their identities. Do not communicate relationships through guessed generated IDs or incidental insertion order.
- Keep the action and decisive observations in the test body. Do not make a reader traverse a fixture DSL, trait, callback, or assertion helper to discover what happened.
- Tolerate local repetition when it preserves the story. Extract stable mechanics or domain vocabulary only when doing so makes the decisive difference easier to see. Apply that extraction bar to every named test-only type, wherever it is declared.
- Use comments to explain non-obvious domain reasoning, arithmetic, chronology, or why a value matters. Do not narrate test phases that the code already makes clear.

## Assert the smallest complete outcome

Assert the smallest complete outcome that distinguishes correct behavior from a plausible wrong result. Prefer meaningful domain values, identities, order, state changes, and effects over counts, presence checks, or field inventories. Keep expected results independent of the implementation under test. When the behavior is rejection, filtering, idempotency, isolation, or a no-op, prove that the relevant state or effect did not change.

## Remove spell-checker coverage

Review the affected tests together, including existing public-boundary tests. Entering through a public boundary does not make every assertion or scenario valuable.

- Remove any assertion, parameterized case, dataset row, scenario, or test that only mirrors available fields, validation rules, guards, configuration branches, or framework behavior without protecting a distinct caller-visible rule.
- Remove repeated proof wherever it appears. In adjacent public-boundary scenarios and lower layers, delete an overlapping assertion, scenario, or test when a clearer public proof already establishes the guarantee and the overlap is not needed to complete a different named outcome.
- Keep several assertions only when they jointly complete the named outcome. Delete checks added only for coverage, symmetry, or because the value is available.

## Mutation testing

In projects that use mutation testing, treat surviving mutations as review evidence, not requirements for new tests. Add or strengthen a test only when a survivor exposes a gap under the same supported-behavior and regression-risk criteria. Ignore a survivor only when no reachable execution can produce a different observable result. Leave other survivors visible in the score, and treat any configured minimum as a regression guard rather than a target.

## Review each test as a whole

Before finalizing:

- Verify that each test's name, decisive setup, action, and outcome describe the same rule.
- Confirm that a plausible wrong implementation cannot pass because of a hidden default, unrelated failure, weak count, or assertion of the wrong record.
- Make sure every remaining assertion and scenario adds evidence to the named outcome instead of repeating proof already established elsewhere.

Refactor or remove any test that fails this review.
