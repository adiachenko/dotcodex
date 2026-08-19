---
name: ask-pro
description: Consult ChatGPT Pro through Chrome for a user-requested second opinion on the current task. Use when the user says "ask pro", "ask ChatGPT Pro", explicitly invokes `$ask-pro`, or otherwise clearly requests a ChatGPT Pro consultation.
---

# Ask Pro

Consult ChatGPT through the web UI when another model's perspective would help. Only start or continue a Pro turn when the user explicitly asks for one. Use the response as a second opinion while you still read the code, test locally, and make the final judgment.

Assume the Pro model starts with no project context. Supply any relevant code, architecture, conventions, constraints, prior attempts, and the kind of answer you need in the prompt.

## Prerequisites

- Use the installed Chrome control capability and follow its instructions to connect to Chrome, choose or open a tab, work with visible page state, and finalize tabs.
- Use the user's existing Chrome session at `https://chatgpt.com/`; do not create a separate browser profile for this skill.
- Expect the Chrome session to be signed in to an account with Pro access. If it is not, leave the ChatGPT tab open for the user, ask them to sign in there, and resume only after they confirm.
- If Chrome control is unavailable or cannot reconnect, stop and report that the consultation is unavailable. Do not switch to standalone Playwright, another browser-control tool, or a separate browser profile.

## Quick Start

```bash
export CODEX_HOME="${CODEX_HOME:-$HOME/.codex}"
umask 077
ASK_PRO_PROMPT="$(mktemp "${TMPDIR:-/tmp}/ask-pro.XXXXXX")" || exit 1

python3 "$CODEX_HOME/skills/ask-pro/scripts/build_prompt.py" \
  --strict \
  --question "Should we keep the event bus or move to direct service calls?" \
  --project-brief "This is a monolith with domain modules and a thin HTTP layer." \
  --task "We are refactoring order completion and need clear ownership boundaries." \
  --convention "Handlers stay thin; business rules belong in services." \
  --constraint "Keep the public API stable during the refactor." \
  --tried "We traced the current flow through the handler and service layer." \
  --desired-output "Recommend one direction, explain why, and name the safest next implementation step." \
  --file /absolute/path/to/service.py#L120-L220 \
  --file /absolute/path/to/handler.py#L20-L90 \
  > "$ASK_PRO_PROMPT" || {
    rm -f -- "$ASK_PRO_PROMPT"
    exit 1
  }

printf '%s\n' "$ASK_PRO_PROMPT"
```

Record the printed absolute path. Follow `references/chatgpt-web-flow.md` to open ChatGPT in Chrome, select Pro, send the file's contents, and collect the answer. Delete the captured file after collecting the answer or abandoning the run.

## Workflow

### 1. Confirm that the task is a good fit

Use this skill after doing enough local investigation to ask a concrete question. Prefer local reasoning instead when the task is simple, purely mechanical, or already well supported by code, tests, or documentation.

### 2. Build a focused prompt bundle

Use `scripts/build_prompt.py` to create a self-contained prompt that assumes ChatGPT knows nothing beyond what you send. Include, at minimum, the pieces that would change the answer:

- a short project briefing
- key local conventions for this task
- the exact question
- a concise task summary
- constraints and assumptions
- what was already tried
- the desired output shape
- the smallest set of files or snippets that materially change the answer

Keep the bundle focused without relying on unstated project knowledge. Do not paste entire subsystems when a couple of files and brief context are enough, but include whatever briefing and conventions the Pro model needs to reason without guessing.

Keep secrets out by default. Avoid `.env` files, key material, auth tokens, and other secret-bearing artifacts unless the user explicitly approves sharing the minimum necessary redacted content. Prefer a distilled summary or a small sanitized snippet over copying raw secret-bearing files.

Prefer `build_prompt.py --strict` for difficult tasks. It will warn, or fail in strict mode, when the prompt is likely underspecified or oversized. Prefer line-ranged excerpts such as `--file /absolute/path/to/service.py#L120-L220` over dumping whole files when only one slice matters.

Write the prompt bundle to a unique temp file with restrictive permissions instead of a fixed shared path. Record its absolute path and keep the file until the message is confirmed sent and the answer is collected. Delete it when the run completes or is abandoned. Do not rely on a shell exit trap across tool calls or accidentally reuse a stale prompt file.

### 3. Open ChatGPT in Chrome

Connect to Chrome according to the installed Chrome control instructions. Reuse a clearly relevant ChatGPT tab when one exists; otherwise open a new tab at `https://chatgpt.com/`. Keep the same tab for model selection, prompt submission, and answer collection.

An earlier relevant Pro conversation can provide context for a user-requested consultation, but it does not authorize another turn. Start a new turn or continue that conversation only for the consultation the user requested. If a follow-up question would help, suggest it and wait for approval before sending anything.

Choose or create the target conversation before selecting the model.

### 4. Select the current Pro mode

Inspect the current page, open the Intelligence or model selector, and select the visible general-purpose option labeled `Pro`. Verify that the composer now shows `Pro` before entering the prompt.

Select the visible `Pro` option and let ChatGPT resolve the underlying model version. Do not encode or infer a numbered model name. If the account does not show a Pro option, stop and report that the requested Pro call is unavailable instead of silently substituting another model.

### 5. Ask the question only when the user has asked for the Pro turn

Fill the main composer with the generated prompt and submit it only when the current request explicitly calls for a new Pro turn or a continuation of the prior one. Keep the prompt file until the turn is fully sent and the answer is collected so you can reuse the exact bundle if browser control fails mid-run. For a follow-up, state what changed since the last turn, what answer you need now, and which new files, constraints, or evidence should override earlier context.

If ChatGPT converts a long paste into a text attachment, use the current visible control to expand it back into the composer. Remove any duplicate attachment or duplicate prompt before submitting.

Immediately before submitting, verify that the intended tab and conversation are active, the selector visibly shows `Pro`, and the composer contains the intended prompt exactly once. After submitting, confirm that exactly one new user message appears in the conversation. Record the resulting chat URL once it exists. If the send state is ambiguous, inspect the current page again before taking another action.

If browser control disconnects, reconnect through Chrome, return to the recorded conversation URL, and check the conversation, `Pro` selection, and composer before touching it. If multiple ChatGPT tabs exist, identify the intended one from current tab metadata rather than assuming the active tab is correct.

### 6. Keep the session open while waiting

Assume a Pro answer may take a long time. Keep the ChatGPT tab and browser task alive within the active Codex turn, checking progress infrequently until the response is clearly complete or the user redirects the work.

Treat the response as complete only after the assistant output stops changing and the page no longer shows active generation, research, or tool work. If completion is ambiguous, keep waiting. When the consultation runs in a subagent, a progress update or wait timeout is not completion; keep waiting while ChatGPT is still generating. Before leaving the page, record that the visible mode was `Pro` and whether this was a new chat or a follow-up.

If a host or tool limit forces the turn to end, preserve the tab for handoff and the recorded conversation URL, report that answer collection is incomplete, and resume only when the user directs the work again. Do not send another message to Pro without fresh explicit authorization.

### 7. Compare the answer with local evidence

Read the response, extract the useful reasoning, and then verify it against the repo, tests, docs, and the current task. If ChatGPT conflicts with local evidence, trust the local evidence and explain the mismatch.

## Guardrails

- Do not use this skill instead of reading the repo or running checks.
- Do not assume the Pro model knows anything about the project that you did not explicitly include in the prompt.
- Keep secrets, tokens, customer data, and private keys out of the prompt unless the user explicitly authorizes sharing the minimum redacted excerpt. By default, block `.env*`, private keys, cookie or session exports, credential stores, raw auth-bearing captures, and similar files.
- Do not ask ChatGPT to decide based on local state you did not include in the prompt bundle.
- Do not substitute another model when the Pro option is unavailable.
- Keep the browser open while waiting for a Pro answer. Slow responses are expected.
- Do not poll the page aggressively while waiting.
- If Chrome control drops, reconnect and verify the recorded chat URL before restarting or resending.
- Do not initiate extra back-and-forth with Pro on your own. If another Pro turn seems useful, propose it and wait for the user to decide.
- Prefer just enough context. Fewer files and a better prompt beat whole-repo dumps.
- Do not retry, regenerate, or continue the conversation automatically after an error or partial answer. Copy any useful partial output, explain the state, and let the user decide whether to spend another Pro turn.
- Summarize the answer back into the working thread in your own words instead of blindly accepting or copying it.

## Resources

- `scripts/build_prompt.py` generates a structured prompt bundle with project briefing, conventions, constraints, prior attempts, follow-up changes, desired output, notes, stdin, selected files or line-ranged excerpts, prompt-quality warnings, and default secret redaction and blocking.
- `references/chatgpt-web-flow.md` explains how to open ChatGPT in Chrome, select the current Pro mode, send the prompt, and capture the answer.
