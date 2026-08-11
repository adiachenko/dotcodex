# ChatGPT Web Flow

Use this reference when the main `SKILL.md` is not enough and you need concrete Chrome-driving heuristics.

Assume ChatGPT Pro starts with zero knowledge of the project unless you explicitly provide that knowledge in the prompt. The browser session preserves login and conversation state, not repo understanding.

## Setup

- Read and follow the installed Chrome control instructions before browser work.
- Connect to the user's existing Chrome session and name the browser task according to those instructions.
- Reuse a clearly relevant ChatGPT tab when one exists; otherwise open a new tab at `https://chatgpt.com/`.
- Keep the chosen tab for the entire Pro turn and finalize it according to the Chrome control instructions when the task ends.
- If Chrome control is unavailable or cannot reconnect, stop and report that the Chrome-backed consultation is unavailable instead of switching browser-control surfaces.

## Open ChatGPT

- Navigate the chosen Chrome tab to `https://chatgpt.com/`.
- Inspect the current visible page before acting.
- If the page redirects to a login screen or shows the logged-out home shell, stop there. Keep the tab open for handoff, ask the user to sign in in that tab, wait for confirmation, and then inspect the page again.
- If the UI does not finish loading, refresh your view of the page before locating controls.

If the skill previously started a relevant Pro conversation, treat that thread as reusable context for a user-requested follow-up, not as permission to keep chatting autonomously. Only continue that thread or start a fresh one when the user explicitly asks for another Pro turn. If you think a follow-up would help, suggest it and wait for approval before sending it.

Choose or create the target conversation now. Do not select Pro in one conversation and then navigate to another before submitting.

## Choose the model

Use current visible page state and follow this sequence:

1. Locate the Intelligence or model selector near the composer; it may show the currently selected mode, such as `Instant`.
2. Open it and inspect the updated menu.
3. Select the visible general-purpose option labeled `Pro`.
4. Confirm that the selector now visibly shows `Pro`.

The `Pro` label is the selection contract. Let ChatGPT map it to its current underlying model rather than hardcoding a numbered model name. If no Pro option is visible, stop and report that the requested Pro call is unavailable.

## Send the prompt

Typical flow:

1. Gather the context ChatGPT cannot infer on its own: project briefing, key conventions, constraints, exact question, what was already tried, desired output, and the smallest set of relevant files.
2. Build the prompt with `scripts/build_prompt.py`, preferably with `--strict` for difficult questions.
3. Save it to a unique temp file with restrictive permissions, capture its printed absolute path in the working state, and keep the file until the turn is fully sent and the answer is harvested.
4. Confirm that the user has explicitly asked for this new or continued Pro turn.
5. Identify the main composer from current visible page state.
6. Paste or fill the prompt into that composer.
7. Submit the message.

When the prompt is large, prefer filling the composer with the complete file contents instead of typing it character by character.
If ChatGPT converts a long paste into a text attachment, use the current visible control to expand it back into the composer and remove any duplicate copy before submitting.
Explicitly delete the captured temp path once the answer is harvested or the run is abandoned. Do not rely on a shell exit trap across tool calls or reuse a stale prompt file.
Do not attach `.env` files, key files, or raw auth-bearing files by default. Redact aggressively and share only what is required for the current question.
Prefer just enough context: a short briefing plus a few relevant excerpts beats dumping large slices of the repo.
Prefer narrow excerpts such as `--file /absolute/path/to/service.py#L120-L220` so the model sees the exact code slice that matters.
Do not rely on the prior conversation alone to carry project facts, constraints, or output expectations. Restate the pieces that matter for the current turn.
When continuing an existing conversation, include a short recap of what changed or what new decision is needed so the follow-up does not rely on the model perfectly inferring the delta.

## Before submit

- Inspect the current page immediately before submitting.
- Confirm that the intended tab and conversation are active.
- Confirm that the selector visibly shows `Pro`.
- Confirm that the composer contains the intended prompt exactly once.
- If the page appears to have inserted or removed content unexpectedly, stop and inspect it again before sending.

## After submit

- Confirm that exactly one new user message appears in the conversation.
- If submit is ambiguous, inspect the page again before pressing Enter or clicking the submit button.
- Record that the visible mode was `Pro`, the conversation URL, and whether this was a new chat or a follow-up before you leave the page.

## Wait for completion

- Expect a Pro answer to take a long time.
- Do not close the Chrome tab or refresh it prematurely while the answer is still in progress.
- Keep the same tab open and inspect progress on a slow cadence within the active Codex turn.
- Treat visible generation, research, or tool activity as still running, even if the answer text has stopped changing. If completion is ambiguous, keep waiting; a delegated progress update or wait timeout is not completion.
- If a host or tool limit forces the turn to end, preserve the tab for handoff and the recorded conversation URL, report that answer collection is incomplete, and resume only when the user directs the work again.

## Read the answer

- Wait for the response to finish streaming before making conclusions.
- Refresh your view whenever the page changes enough that prior element references may be stale.
- Extract the answer into local notes in your own words.
- Preserve only the useful reasoning, tradeoffs, risks, and recommended next step.
- If the UI shows retry, regenerate, network error, or a partial answer, copy any useful partial output first, then stop and let the user decide whether another Pro turn is worth it.

## Troubleshooting

- If element references stop working, inspect the current page again.
- If Chrome control disconnects, reconnect and return to the recorded conversation URL before considering a new chat or a second submit.
- After reconnecting, verify that the selected tab and visible conversation match the intended run and that the selector still shows `Pro` before touching the composer. If multiple ChatGPT tabs exist, identify the intended one from current tab metadata.
- If the wrong mode remains selected, inspect the page again and reopen the picker from current visible controls.
- If one prompt-fill path fails, reuse the saved prompt file and try another complete-fill approach. Do not reconstruct the prompt by hand or send a partial version.
- If the composer rejects a very large prompt, shrink the bundle instead of pasting more repo context.
- If the answer is generic, tighten the question and include only the files that actually matter.
