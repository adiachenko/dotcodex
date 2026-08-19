---
name: maintain-project-docs
description: Use when implementing or reviewing code or project changes that may require updates to project-facing documentation covering behavior, structure, setup, usage, workflows, or agent or contributor guidance, including `README.md`, `AGENTS.md`, `CLAUDE.md`, and `.ai/guidelines/**`.
---

# Maintain Project Docs

Use this skill to decide whether project documentation needs changes, choose the right document for the audience, and keep instructions in one durable place.

## Documentation Check

When making or reviewing a project change, inspect the relevant existing documentation and the place where the intended audience would expect to find new information about that change.

Update documentation only when the change makes an existing statement false or when the intended audience needs documentation to discover, understand, use, configure, deploy, migrate, or contribute to what changed. Make the smallest edit that restores accuracy or supplies the missing information. Measure the edit in words changed, not sentences or sections touched.

Every added, removed, or altered word must be traceable to the triggering change. It must correct something the change made false or supply information the audience now needs. Preserve the wording, terminology, style, and structure of everything else, even where you would phrase it differently. A documentation update does not include rewording true statements, aligning vocabulary with new names, or polishing prose.

Before finishing, review the diff and revert any change you cannot justify under these rules. Leave accurate documentation outside the edit unchanged unless the user separately asks to improve it.

## Audience Routing

Keep each document focused on its reader:

- Put product installation and setup instructions in established setup documentation when it exists; otherwise put them in `README.md`.
- Put end-user behavior and product usage in the product's established documentation location, including any workspace-level or external location assigned by project guidance. If none exists, put them under a repository-root `docs/` directory and link to it from `README.md`.
- Put agent-only guidance in agent docs. In Laravel Boost projects, use the project-owned locations identified below rather than generated agent locations.
- Put contributor and maintainer workflows in established contributor or maintainer documentation; otherwise put them in `README.md`.
- Do not mix agent-only instructions into user-facing docs.

Write documentation at its intended reader's level. In end-user documentation, describe the experience, capabilities, setup, and usage they need; avoid explaining implementation machinery unless it is necessary for the user to succeed.

## Laravel Boost

Treat `.ai/guidelines/**` and `.ai/skills/**` as project-owned. Treat Boost-provided skills and generated agent paths such as `.agents/skills/**`, `.claude/skills/**`, `AGENTS.md`, and `CLAUDE.md` as generated or provider-owned artifacts. Leave them unchanged unless the user explicitly asks to edit the artifact.

Treat content inside `<laravel-boost-guidelines>` in `AGENTS.md` or `CLAUDE.md` as generated and managed by Boost. Do not edit that section, even when the user asks to edit the surrounding artifact. Put project-specific guidance in the appropriate project-owned location instead.

## Single Source of Truth

Keep the complete or normative version of changed guidance in one authoritative location. Other documents may retain the smallest audience-specific summary or quick-start detail needed to stand on their own and refer readers to that source.

Reconcile duplicate or conflicting guidance only when the required update touches it. Leave unrelated material unchanged.

## Examples

Keep examples sparse and purposeful. Update a representative existing example instead of adding incidental variants.
