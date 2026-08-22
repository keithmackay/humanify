# Report Formats

Two templates. Use the edit-mode template for a normal run, the dry-run template for `/humanify --dry-run`.

## Edit mode

```
HUMANIFY REPORT: [target file/text]

BANNED PHRASES FOUND: [N]
- "[phrase]" at [location] -> [suggested fix or "removed"]

AI VOCABULARY FLAGGED: [N]
- "[word]" at [location] -> [replacement]

STRUCTURAL ISSUES: [N]
- [check name]: [location] -> [what was done]

PROTECTED CONTENT (flagged, not modified):
- "[phrase]" at [location] - [why protected, e.g. "inside a direct quotation"]

CHECKS SKIPPED:
- [check name] - [reason, e.g. "input under 300 words"]

UNCHANGED (flagged but left as-is, with reason):
- [item]: [reason - e.g. "em-dash retained - no --voice sample provided to confirm authorial intent"]

SUMMARY: [N] changes made across [N] categories.
```

## Dry-run mode

Nothing is edited. Report every finding with a recommendation, and give the normalized density figure.

```
HUMANIFY DRY RUN: [target file/text]
Word count: [N]

BANNED PHRASES FOUND: [N]
- "[phrase]" at [location] -> [recommended fix]

AI VOCABULARY FLAGGED: [N]
- "[word]" at [location] -> [recommended replacement]

STRUCTURAL ISSUES: [N]
- [check name]: [location] -> [recommendation]

PROTECTED CONTENT (flagged, would not be modified):
- "[phrase]" at [location] - [why protected]

CHECKS SKIPPED:
- [check name] - [reason]

DENSITY: [N] findings per 1,000 words - [band label]
No edits applied.
```

**Bands:**

| Findings per 1,000 words | Reads as |
| --- | --- |
| 0-2 | human-authored |
| 3-7 | some tells present |
| 8+ | heavily AI-flavored |

These bands are a rough heuristic for orienting the author, not a validated AI detector. Do not present the figure as a probability, a score, or evidence that text was or was not machine-generated.
