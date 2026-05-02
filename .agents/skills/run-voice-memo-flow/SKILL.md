---
name: run-voice-memo-flow
description: "Run this repository's full voice memo workflow end to end: use create-clean-md to generate clean.md from tmp.md, use create-refined-md to create a new refined/{UTC timestamp}.md file from clean.md, then delete clean.md and empty tmp.md only after both generation steps succeed. Use when Codex should process the current tmp.md through AGENTS.md's unified flow while preserving memo data on failures."
---

# Run voice memo flow

## Purpose

Run the complete voice memo workflow defined in `AGENTS.md`.

This skill coordinates the existing skills:

- `$create-clean-md` at `.agents/skills/create-clean-md/SKILL.md`
- `$create-refined-md` at `.agents/skills/create-refined-md/SKILL.md`

The cleanup steps are intentionally last. This is a core safety property: keep `tmp.md` and `clean.md` available until both generated outputs exist, so a failure does not erase the source memo or the intermediate text needed for debugging.

## Required Order

Execute these steps in order:

1. Use `$create-clean-md` to read `tmp.md` and create `clean.md`.
2. Use `$create-refined-md` to read `clean.md` and create one new `refined/{UTC timestamp}.md` file.
3. Delete `clean.md`.
4. Empty `tmp.md` while keeping the `tmp.md` file itself.

Do not skip, reorder, parallelize, or combine these steps.

## Failure Handling

If any step fails or cannot be confidently verified:

- Stop the workflow immediately.
- Do not run any later steps.
- Do not delete `clean.md`.
- Do not empty `tmp.md`.
- Tell the user which step failed and why, if known.
- Tell the user to manually perform steps 3 and 4 later if they still want cleanup after debugging.

Treat uncertainty as failure. For example, if it is unclear whether the refined file was created correctly, stop before deleting or emptying anything.

## Step Details

### 1. Create clean.md

Use the `$create-clean-md` skill exactly for this responsibility. It should read `AGENTS.md` and `tmp.md`, then write `clean.md` as a cleanup result without summarizing or restructuring the memo.

After this step, verify that `clean.md` exists and contains the cleaned memo content.

### 2. Create refined output

Use the `$create-refined-md` skill exactly for this responsibility. It should read `AGENTS.md` and `clean.md`, then write one new `refined/{UTC timestamp}.md` file.

After this step, verify that a new `refined/*.md` file was created and that it is based on the current `clean.md`.

Do not summarize, append to, or consolidate existing files under `refined/`.

### 3. Delete clean.md

Only after steps 1 and 2 have succeeded and been verified, delete `clean.md`.

This deletion is part of the successful final cleanup only. Never perform it after a failed or uncertain generation step.

### 4. Empty tmp.md

Only after `clean.md` has been deleted successfully, clear the contents of `tmp.md` while preserving the file.

Do not delete `tmp.md`. It must remain as an empty file for the next voice memo input.

## Safety Checks

Before deleting or emptying anything, verify:

- `clean.md` was created from the current `tmp.md`.
- A new refined Markdown file was created from the current `clean.md`.
- No existing refined file was appended to or overwritten.
- The workflow is not in a failed or uncertain state.

After cleanup, verify:

- `clean.md` no longer exists.
- `tmp.md` exists and is empty.
- The new refined file still exists.

## User-Facing Summary

When finished, report:

- The new refined file path.
- Whether `clean.md` was deleted.
- Whether `tmp.md` was emptied and preserved.

When stopped due to failure, report:

- The failed step.
- Which files were intentionally left in place.
- That the user should manually run cleanup steps 3 and 4 after debugging if desired.
