# Output Format

Read this file at the reporting step - `SKILL.md` Workflow step 6 (`--scan`) or step 8 (edit path).

## Edit path (default, no `--scan`)

```
HUMANIFY REPORT: [target file/text]

BANNED PHRASES FOUND: [N]
- "[phrase]" at [location] -> [suggested fix or "removed"]

AI VOCABULARY FLAGGED: [N]
- "[word]" at [location] -> [replacement]

STRUCTURAL ISSUES: [N]
- [check name]: [location] -> [what was done]

CHECKS SKIPPED: [none, or list - e.g. "punctuation-personality, list-variety (short-form input, <300 words)"]

UNCHANGED (flagged but left as-is, with reason):
- [item]: [reason - e.g. "part of author's established voice", "protected span"]

SUMMARY: [N] changes made across [N] categories.
```

## Scan mode (`--scan`)

No edits are applied; report every finding with a recommendation, plus a density gauge so the count means something regardless of document length.

```
HUMANIFY SCAN: [target file/text]
LENGTH: [N] words

BANNED PHRASES FOUND: [N]
- "[phrase]" at [location] -> [recommended fix]

AI VOCABULARY FLAGGED: [N]
- "[word]" at [location] -> [recommended replacement]

STRUCTURAL ISSUES: [N]
- [check name]: [location] -> [recommendation]

CHECKS SKIPPED: [none, or list - e.g. "punctuation-personality, list-variety (short-form input, <300 words)"]

DENSITY: [N] findings per 1,000 words - [band]
  Bands: 0-3 Clean · 4-8 Light · 9-15 Moderate · 16+ Heavy

SUMMARY: [N] total findings across [N] categories. [band] density - [one-line recommendation, e.g. "worth a revision pass" / "minor cleanup only"].
```
