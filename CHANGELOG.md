# Changelog

All notable changes to the `memo` skill are documented here. Format follows [Keep a Changelog](https://keepachangelog.com); the skill follows [SemVer](https://semver.org).

## [0.1.0] — 2026-05-14

First public release. The skill ships alongside [`memo-app`](https://github.com/b1rdmania/memo-app), a static web demo deployed at [memo-app-eta-tawny.vercel.app](https://memo-app-eta-tawny.vercel.app).

### Added
- `SKILL.md` with the full contract: three audiences (Client, Junior lawyer, Senior lawyer), one output per audience, paragraph-citation rules, confidence calibration, plain-English pass, scope boundaries.
- JSON output contract: `{ decision, rendered, claims[] }` with `source_paragraphs` and `confidence` per claim.
- Required input: paragraph-numbered text (`[1]`, `[2]`, …). Skill refuses unnumbered input.
- Two paragraph-numbered sample memos in `samples/`:
  - **Khan v Acme** — 20-paragraph unfair-dismissal advice note.
  - **TideSync** — 82-paragraph SPA warranty dispute with cross-paragraph citations.
- Six sample outputs (`samples/memo-*-{client,junior,senior}.json`) covering all audiences for both memos.

### Validated
- Citation accuracy: every cited paragraph in the six sample outputs maps to a paragraph that genuinely supports the claim.
- Long-context behaviour: TideSync output cites paragraphs as far apart as 11 and 77 without drift.
- Confidence calibration: one honest `med` on a synthesised claim; one honest `low` with empty `source_paragraphs` on a professional-judgment estimate. Labels are not performative `high`s.

### Known limitations
- Beyond ~80 paragraphs not yet evaluated.
- Self-rated confidence — useful but unverified against an external rubric.
- No drafting, no legal research, no privilege checking — these are out of scope by design.

[0.1.0]: https://github.com/b1rdmania/memo/releases/tag/v0.1.0
