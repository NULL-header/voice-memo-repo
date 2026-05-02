---
name: create-clean-md
description: Create clean.md from tmp.md for this repository's voice memo workflow. Use when Codex needs to read voice-input text in tmp.md and produce clean.md by removing transcription noise, typos, fillers, stutters, and only clearly meaningless repetition while preserving the original wording, tone, order, and nearly all content without summarizing or restructuring.
---

# Create clean.md

## Purpose

Create `clean.md` from `tmp.md` in this repository. This skill performs text cleanup only. It must not refine, summarize, reorganize, or convert the memo into polished Markdown.

## Inputs and Output

- Read `AGENTS.md` first, then read `tmp.md`.
- Write `clean.md` as the cleanup result.
- Treat `tmp.md` as voice-recognition text. It normally does not use Markdown intentionally and may contain misrecognized words, typos, filler, stutters, and accidental repetition.
- Preserve the source order, wording, sentence shape, and tone as much as possible.

## Workflow

1. Read `AGENTS.md`, then read `tmp.md`.
2. Identify obvious noise from voice input:
   - fillers such as "えー", "あの", "その", "まあ", "なんか" when they do not carry meaning
   - stutters and false starts that do not affect meaning
   - clearly accidental repeated words, phrases, or clauses
   - obvious typos or speech-recognition conversion errors inferred from nearby context
3. Produce `clean.md` with the same content flow as `tmp.md`.
4. Do a final pass to confirm no summarization, reordering, or style rewriting was introduced.

## Editing Rules

- Do not change the writing style, tone, or level of formality.
- Do not summarize, compress, extract topics, add headings, or restructure into Markdown.
- Do not improve prose beyond cleanup. Awkward but meaningful phrasing should remain.
- Correct typos and misrecognitions only when the surrounding context makes the intended wording clear.
- Preserve ambiguous wording rather than guessing.
- Remove repetition only when it is clearly meaningless repetition from speech input.
- Keep repetitions that may be corrections, emphasis, contrast, or meaningful restatements.
- Keep self-corrections when deleting them would remove the final intended meaning.
- Do not add facts, examples, explanations, or transitions.
- Do not overwrite unrelated existing files.

## Repetition Judgment

Use a conservative rule: if removing a repeated phrase could change the meaning, keep it.

Remove:

```text
今日は今日は会議について話します。
```

as:

```text
今日は会議について話します。
```

Keep:

```text
来週、いや再来週に提出します。
```

because the second phrase corrects the first.

Keep:

```text
これは重要です。重要なので先に確認します。
```

because the repetition adds emphasis and connection.

## Output Check

Before finishing, verify:

- `clean.md` contains nearly all meaningful content from `tmp.md`.
- The text is cleaner but still recognizably the same memo.
- No refined summary, outline, or Markdown hierarchy was created.
- Any correction made for a typo or misrecognition is supported by context.
