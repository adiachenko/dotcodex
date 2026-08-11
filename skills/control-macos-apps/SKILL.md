---
name: control-macos-apps
description: Use when a task involving a local macOS app requires creating, inspecting, changing, or verifying app state; choosing an automation or control surface; or following the user's specified interaction mode, including direct UI control.
---

# Control macOS Apps

Prefer first-class non-GUI interfaces for macOS apps when available, such as official MCP tools, official CLIs, official app integrations, supported APIs, Shortcuts actions, or documented import/export paths. Otherwise use Computer Use.

When a task is intended to run unattended or on a schedule, and no first-class non-GUI interface is available while Computer Use cannot be expected to operate reliably in that execution context, you may recommend a suitable non-GUI implementation. Implement or use that approach only when the user explicitly requests or approves it.

Once the user specifies an interaction mode, stay within that mode unless you get explicit approval to switch.

When using Computer Use, treat the visible app state as the source of truth. Verify required fields and settings before claiming completion. If a required control is inaccessible or unreliable, try a small number of reasonable UI alternatives; if it still cannot be verified, report exactly what was completed and what remains unverified.
