---
name: land-pr
description: Use when the user explicitly asks to land pull requests.
---

# Land PR

Merge feature and hotfix PRs identified by the request or current task, update the local checkouts, and clean up their merged branches. Leave merges between long-lived branches and their cleanup to the user.

## Merge Readiness

Check the current PR head's checks, reviews, and conversation to identify configured or requested asynchronous reviews and tools. Wait for all applicable merge-relevant work to finish before merging.

Merge only after checks pass and completion is confirmed. If those checks or reviews report actionable findings, report them to the user and wait for direction before merging. Report failed or unverifiable prerequisites as blockers and leave the PR unmerged.

## Merge Strategy

Follow project branching guidance. Otherwise:

- **Features and hotfixes with one target:** squash merge.
- **Hotfixes with multiple targets among `main`, `develop`, and release branches:** merge the same hotfix branch into each target through separate PRs using merge commits.
- **Long-lived version branches:** use squash PRs for every target instead. Land shared fixes in `main` first, then cherry-pick them onto separate branches based on each affected supported version. Preserve version-specific differences without introducing unrelated divergence. If a change lands on a version branch first, carry it into `main` and other affected version branches through separate PRs.
- **User requests preserving individual commits:** use merge commits.

For hotfixes, refresh remote branch information and determine all required targets before the first merge: `main` (or `master`), `develop`, the active release branch, and supported version branches.

Always create an explicit merge commit when that strategy applies. Never rebase.

## Completion

Complete the applicable merge sequence through PRs, applying the same readiness checks to each. Update each local target branch with `--ff-only` and finish on the final target branch.

Delete merged feature and hotfix branches locally and remotely after all intended PRs have landed and after checking for commits added after the merge. For squash merges, verify against the merged PR head.

Leave release-branch cleanup to the user.

Report the result and anything left unfinished.
