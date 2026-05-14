# Memo

A Claude skill that turns a legal memo into the right shape for whoever is reading it next.

Three audiences. One output per audience. No format picker. The tool decides the shape.

| Audience | Output |
|---|---|
| **Client** | Plain-English email — decision up top, jargon stripped, actions listed |
| **Junior lawyer** | Research + drafting brief — what to read, what to draft, what to chase |
| **Senior lawyer** | Risk & sign-off note — top risks, what to escalate, what's settled |

Every factual claim in the output cites the source paragraph it came from, with a confidence label (`high` / `med` / `low`).

## Install

This is a Claude Code skill. Drop the repo into your skills directory:

```
git clone https://github.com/b1rdmania/memo ~/.claude/skills/memo
```

Then ask Claude something like:

> Here's a legal memo. Give me the client version.
> ```
> [1] MEMORANDUM ...
> [2] ...
> ```

The skill auto-triggers on legal memo input.

## How it works

1. Paragraph-number the memo before sending it (`[1]`, `[2]`, …). The skill refuses unnumbered input — citations depend on the numbers.
2. The skill returns JSON: `decision`, `rendered`, and a `claims` array mapping each factual claim to its source paragraph(s) plus a confidence score.
3. Render the `rendered` text however you like. Use `claims` to highlight or hyperlink each statement back to the source.

Output contract:

```json
{
  "decision": "one sentence — the decision the audience is being asked to make",
  "rendered": "the output body",
  "claims": [
    { "claim": "...", "source_paragraphs": [7, 8], "confidence": "high" }
  ]
}
```

See [`SKILL.md`](./SKILL.md) for the full contract.

## Examples

The [`samples/`](./samples) directory contains a paragraph-numbered employment memo (`memo-khan.txt`) and the three audience outputs for it:

- [`memo-khan-client.json`](./samples/memo-khan-client.json) — plain-English client email
- [`memo-khan-junior.json`](./samples/memo-khan-junior.json) — junior lawyer research + drafting brief
- [`memo-khan-senior.json`](./samples/memo-khan-senior.json) — senior lawyer risk & sign-off note

Same memo. Three visibly different products.

## Design choices

**No format picker.** Briefly and similar tools ask the user to choose: short email, one-pager, Slack bullets, world map, flowchart. That's the tool's job, not the user's. Pick who's reading; we give you the shape.

**Citations are first-class.** Every claim maps to a paragraph. If a claim can't be cited, the skill won't make it. This kills the hallucination objection up front.

**Confidence labels are honest.** A `high` label means the source states the claim directly. `med` means synthesised across paragraphs. `low` means inferred. Empty `source_paragraphs` with `low` confidence means it's professional judgment, not from the memo — flagged accordingly.

**Plain English pass on the client output.** No Burchell, no Iceland Frozen Foods, no Latin section references. A bright 16-year-old should follow it. Junior and Senior outputs keep legal vocabulary.

## Status

Prototype. Do not use with privileged material. Source paragraph citation accuracy degrades on memos longer than ~50 paragraphs — eval is forthcoming.

## License

MIT.
