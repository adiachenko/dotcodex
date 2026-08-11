---
name: ask-pro
description: Consult ChatGPT Pro through Chrome for a user-requested second opinion on the current task. Use when the user says "ask pro", "ask ChatGPT Pro", explicitly invokes `$ask-pro`, or otherwise clearly requests a ChatGPT Pro consultation.
---

# Ask Pro

Consult ChatGPT through the web UI when another model's perspective is likely to help. This skill is user-initiated: do not start or continue a Pro turn unless the current user explicitly asked for that consultation. Treat it as a second opinion, not as a replacement for reading the code, testing locally, or making the final judgment yourself.

Assume the Pro model starts with zero knowledge of the project. It does not know the codebase, architecture, conventions, constraints, what you already tried, or what kind of answer you want unless you explicitly include that context in the prompt bundle.

## Prerequisites

- Use the installed Chrome control capability and follow its instructions for connecting to Chrome, selecting or opening a tab, interacting with visible page state, and finalizing tabs.
- Use the user's existing Chrome session at `https://chatgpt.com/`; do not create a separate browser profile for this skill.
- Expect the Chrome session to be signed in to an account with Pro access. If it is not, leave the ChatGPT tab open for handoff, ask the user to sign in there, and resume only after confirmation.
- If Chrome control is unavailable or cannot reconnect, stop and report that the Chrome-backed consultation is unavailable. Do not fall back to standalone Playwright, another browser-control surface, or a separate browser profile.

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

Capture the printed absolute path in the working state. Follow `references/chatgpt-web-flow.md` to open ChatGPT in Chrome, select Pro, send the file's contents, and harvest the answer. Explicitly delete that captured path after the answer is harvested or the run is abandoned.

## Workflow

1. Confirm that the task is a good fit.

Use this skill after doing enough local investigation to ask a concrete question. Prefer local reasoning instead when the task is simple, purely mechanical, or already well supported by code, tests, or documentation.

2. Build a focused prompt bundle.

Use `scripts/build_prompt.py` to create a self-contained prompt that assumes ChatGPT knows nothing beyond what you send. Include, at minimum, the pieces that would change the answer:

- a short project briefing
- key local conventions for this task
- the exact question
- a concise task summary
- constraints and assumptions
- what was already tried
- the desired output shape
- the smallest set of files or snippets that materially change the answer

Keep the bundle focused, but do not rely on unstated project knowledge. Do not paste entire subsystems when two files and three bullets would do, but do include enough briefing and conventions that the Pro model can reason without guessing.

Do not attach secrets by default. Avoid `.env` files, key material, auth tokens, or other secret-bearing artifacts unless the user explicitly approved sharing the minimum necessary redacted content. Prefer a distilled summary or a tiny sanitized snippet over copying raw secret-bearing files.

Prefer `build_prompt.py --strict` for difficult tasks. It will warn, or fail in strict mode, when the prompt is likely underspecified or oversized. Prefer line-ranged excerpts such as `--file /absolute/path/to/service.py#L120-L220` over dumping whole files when only one slice matters.

Write the prompt bundle to a unique temp file with restrictive permissions instead of a fixed shared path. Capture its absolute path in the working state, keep the file until the message is confirmed sent and the answer is harvested, then explicitly delete the captured path when the run completes or is abandoned. Do not rely on a shell exit trap across tool calls or accidentally reuse a stale prompt file.

3. Open ChatGPT in Chrome.

Connect to Chrome according to the installed Chrome control instructions. Reuse a clearly relevant ChatGPT tab when one exists; otherwise open a new tab at `https://chatgpt.com/`. Keep the same tab for model selection, prompt submission, and answer collection.

If this skill already started a relevant Pro conversation earlier, treat that thread as reusable context, not as an invitation to keep chatting on your own. Only start a new Pro turn or continue an existing Pro conversation when the user explicitly asks for that Pro consultation. If you think a follow-up question would help, suggest the follow-up and wait for the user's approval before sending anything.

Choose or create the target conversation before selecting the model.

4. Select the current Pro mode.

Inspect the current page, open the Intelligence or model selector, and select the visible general-purpose option labeled `Pro`. Verify that the composer now shows `Pro` before entering the prompt.

Treat `Pro` as the durable selection contract and let ChatGPT resolve the underlying current model version. Do not encode or infer a numbered model name. If the account does not expose a visible Pro option, stop and report that the requested Pro call is unavailable instead of silently substituting another model.

5. Ask the question only when the user has asked for the Pro turn.

Fill the main composer with the generated prompt and submit it only when the current user request explicitly calls for a new Pro turn or a continuation of the prior one. Keep the generated prompt file until the turn is fully sent and the answer is harvested, so you can reuse the exact same bundle if browser control fails mid-run. When continuing an existing Pro conversation, make the follow-up explicit: summarize what changed since the last turn, what answer you need now, and any new files, constraints, or evidence that should override earlier context.

If ChatGPT converts a long paste into a text attachment, use the current visible control to expand it back into the composer. Remove any duplicate attachment or duplicate prompt before submitting.

Immediately before submitting, verify that the intended tab and conversation are active, the selector visibly shows `Pro`, and the composer contains the intended prompt exactly once. After submitting, confirm that exactly one new user message appears in the conversation. Record the resulting chat URL once it exists. If the send state is ambiguous, inspect the current page again before taking another action.

If browser control disconnects, reconnect through Chrome, return to the recorded conversation URL, and reverify the conversation, `Pro` selection, and composer before touching it. If multiple ChatGPT tabs exist, identify the intended one from current tab metadata rather than assuming the active tab is correct.

6. Wait for the response without tearing down the session.

Assume a Pro-tier answer may take a long time. Keep the ChatGPT tab and browser task alive within the active Codex turn and inspect progress on a slow cadence until the response is clearly complete or the user redirects the work.

Treat the response as complete only after the assistant output stops changing and the page no longer shows active generation, research, or tool work. If completion is ambiguous, keep waiting. When the consultation runs in a subagent, a progress update or wait timeout is not completion; keep waiting while ChatGPT is still generating. Record that the visible mode was `Pro` and whether this was a new chat or a follow-up before leaving the page.

If a host or tool limit forces the turn to end, preserve the tab for handoff and the recorded conversation URL, report that answer collection is incomplete, and resume only when the user directs the work again. Do not send another message to Pro without fresh explicit authorization.

7. Reconcile the answer with local evidence.

Read the response, extract the useful reasoning, and then verify it against the repo, tests, docs, and the current task. If ChatGPT conflicts with local evidence, trust the local evidence and explain the mismatch.

## Guardrails

- Do not use this skill instead of reading the repo or running checks.
- Do not assume the Pro model knows anything about the project that you did not explicitly include in the prompt.
- Do not paste secrets, tokens, customer data, or private keys unless the user explicitly authorizes that risk. Redact aggressively by default, and prefer not to attach `.env` files or key files at all.
- Block secret-heavy artifacts by default: `.env*`, private keys, cookie or session exports, credential stores, raw auth-bearing captures, and similar files should stay out of the prompt unless the user explicitly approved the minimum redacted excerpt.
- Do not ask ChatGPT to decide based on local state you did not include in the prompt bundle.
- Do not substitute another model when the Pro option is unavailable.
- Do not close the browser prematurely while waiting on a Pro answer. Slow responses are expected.
- Do not poll the page aggressively while waiting.
- Do not assume a dropped Chrome control session means the ChatGPT thread was lost. Reconnect and verify the recorded chat URL before considering a restart or re-send.
- Do not initiate extra back-and-forth with Pro on your own. If another Pro turn seems useful, propose it and wait for the user to decide.
- Prefer just enough context: fewer files plus a better prompt beat whole-repo dumps.
- Do not retry, regenerate, or continue the conversation automatically after an error or partial answer. Copy any useful partial output, explain the state, and let the user decide whether to spend another Pro turn.
- Summarize the answer back into the working thread in your own words instead of blindly accepting or copying it.

## Resources

- `scripts/build_prompt.py`: generate a structured prompt bundle with project briefing, conventions, constraints, prior attempts, follow-up deltas, desired output, notes, stdin, selected files or line-ranged excerpts, prompt-quality warnings, and default secret redaction/blocking
- `references/chatgpt-web-flow.md`: use Chrome to open ChatGPT, select the current Pro mode, send the prompt, and capture the answer
