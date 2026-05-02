---
name: answer-from-refined
description: Answer user questions by searching and reading only Markdown files under this repository's refined/ directory. Use when the user asks Codex to answer, recall, look up, or verify something from previously refined voice memo notes, and the answer must be grounded exclusively in refined/*.md without web search, inference, or other repository files.
---

# Answer From Refined

## Purpose

Answer the user's question using only the current repository's `refined/` directory as the information source.

This skill is intentionally source-restricted. Do not use web search, general knowledge, inference beyond the text, or any repository files outside `refined/`.

## Allowed Sources

Use only files matching:

```text
refined/*.md
```

Do not read or use:

- `README.md`, `AGENTS.md`, `tmp.md`, `clean.md`, `.agents/`, `.git/`, or any other repository path
- files outside this repository
- web pages or internet search results
- memory of prior conversation as factual support, unless the same fact is found in `refined/*.md`

It is acceptable to list or search filenames under `refined/` to locate candidate Markdown files.

## Workflow

1. Identify the user's question and important search terms.
2. Search only `refined/*.md` for relevant terms.
3. Read only the candidate `refined/*.md` files needed to answer.
4. Answer only what is supported by those files.
5. Name the refined file or files used as sources.

If the answer cannot be found in `refined/*.md`, say that the refined notes do not contain enough information to answer. Do not guess or fill gaps.

## Answer Rules

- Keep the answer concise and direct.
- Preserve uncertainty when the notes are ambiguous.
- Distinguish clearly between facts explicitly written in the notes and things not present.
- Do not add background knowledge, explanations, or recommendations that are not supported by `refined/*.md`.
- Do not cite or mention files that were not actually used.
- If multiple refined notes conflict, report the conflict and identify the files involved instead of resolving it by inference.

## Suggested Source Search

Prefer fast local search tools scoped to `refined/*.md`, for example:

```bash
rg "keyword" refined --glob "*.md"
```

Use `sed`, `nl`, or another simple reader only on relevant `refined/*.md` files after search results identify candidates.
