---
name: create-refined-md
description: Create a new refined Markdown file under refined/ from clean.md for this repository's voice memo workflow. Use when Codex needs to read clean.md, extract and organize its meaning, and write refined/{UTC timestamp}.md as a new polished memo without summarizing existing refined files or appending to any refined file.
---

# Create refined Markdown

## Purpose

Create a new `refined/{timestamp}.md` file from `clean.md` in this repository. This skill turns the cleaned voice memo into a structured Markdown note.

This skill is responsible only for refining the current `clean.md` content. It must not merge, summarize, compare, or consolidate files already under `refined/`.

## Inputs and Output

- Read `AGENTS.md` first, then read `clean.md`.
- Write exactly one new file at `refined/{timestamp}.md`.
- Use a UTC timestamp for `{timestamp}`.
- Use Markdown headings, lists, and short paragraphs when they help clarify the content.
- Preserve the source meaning and intent. Remove incidental spoken-language looseness when organizing, but do not add new facts.

## Workflow

1. Read `AGENTS.md` to confirm repository rules.
2. Read `clean.md`.
3. Extract the meaning, decisions, questions, TODOs, and observations present in `clean.md`.
4. Organize the content into a readable Markdown note.
5. Generate the output filename using the current UTC timestamp.
6. Confirm that the chosen `refined/{timestamp}.md` path does not already exist.
7. Write the new file.
8. Do a final pass to confirm the file is a refined version of `clean.md`, not a summary of `refined/`.

## Refinement Rules

- Summarize and organize `clean.md`; do not merely copy-clean it.
- Keep the content grounded in `clean.md`.
- Preserve ambiguity when the source is ambiguous.
- Separate concrete decisions, tentative ideas, and open questions when possible.
- Use natural Japanese unless `clean.md` clearly uses another language.
- Create concise headings based on the source content.
- Do not invent background, conclusions, examples, deadlines, or action owners.
- Do not include a meta explanation of the cleanup/refinement process unless it is part of the source memo.

## File Rules

- Do not overwrite existing files.
- Do not append to files under `refined/`.
- Do not edit or summarize existing files under `refined/`.
- Do not treat `refined/` as cumulative storage to be consolidated.
- If a generated timestamp path already exists, generate a later UTC timestamp or add enough UTC precision to make a new path.

## Timestamp Format

Prefer this filename format:

```text
YYYYMMDDTHHMMSSZ.md
```

Example:

```text
refined/20260502T031455Z.md
```

## Output Check

Before finishing, verify:

- The output file is newly created under `refined/`.
- The file is based only on `clean.md` and repository rules.
- Existing `refined/` files were not modified, appended to, or summarized.
- The note is structured Markdown and is more organized than `clean.md`.
- No unsupported facts were added.
