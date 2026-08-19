---
name: control-macos-apps
description: Use for local macOS app tasks that require creating, inspecting, changing, or verifying app state; choosing how to automate or control the app; or following the user's specified interaction mode, including direct UI control.
---

# Control macOS Apps

Prefer a first-class non-GUI interface when one is available, such as an official MCP tool, official CLI, official app integration, supported API, Shortcuts action, or documented import/export path. Otherwise, use Computer Use.

For unattended or scheduled tasks, you may recommend a suitable non-GUI implementation when no first-class non-GUI interface is available and Computer Use cannot be expected to work reliably in that execution context. Implement or use that approach only when the user explicitly requests or approves it.

After the user specifies an interaction mode, stay within it unless they explicitly approve a switch.

When using Computer Use, treat the visible app state as authoritative. Verify required fields and settings before reporting completion. If a required control is inaccessible or unreliable, try a small number of reasonable UI alternatives. If you still cannot verify it, report exactly what you completed and what remains unverified.
