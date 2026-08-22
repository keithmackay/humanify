# Implementation Plan: humanify improve-this findings

**Source:** `docs/reviews/2026-08-21-improve-this.md`
**Status of already-fixed findings:** #2 (help.md orphaned) was fixed in the 2026-08-22 `--help` wiring pass — `SKILL.md` now instructs reading `help.md` verbatim, and a target-document resolution step was added to the Workflow. Both are excluded below.

Remaining findings, phased by dependency (content-shaping decisions first, mechanical extraction last, since #9's extraction plan depends on what stays inline after #11 and #1).

---

## Phase 1: Resolve the voice-calibration gap (Finding #1, High/High)

The skill repeatedly defers to "the author's established voice" with no way to determine what that is. This is the single largest gap and blocks a correct fix to #11 (which checks are house-style vs. universal AI tells).

### Task 1.1 — Add a `--voice` argument

In `SKILL.md`'s Arguments section, add:
- `/humanify --voice <file-or-path>` - Before running the pass, read the given file(s) as a calibration sample of the author's own prior writing. Derive an allow-list of habitual constructions (e.g., em-dash use, sentence-fragment frequency, contraction use) from the sample, and suppress any check in this skill that would flag a construction the sample shows the author uses deliberately and repeatedly.

### Task 1.2 — Add a workflow step that consumes it

Insert a new Workflow step 0 (before target-document resolution): "If `--voice` was provided, read the calibration sample(s) first and note which house-style checks (see Task 3.1) it overrides. If no sample was provided, apply house-style checks at their documented default (flag them) and say so in the report's UNCHANGED section rationale, e.g. 'em-dash retained — no --voice sample provided to confirm authorial intent.'"

### Task 1.3 — Update `README.md` and `help.md`

Document `--voice` in both, matching the phrasing style already used for `--scan`.

**Verification:** Manually invoke the skill's logic against two synthetic samples — one with heavy em-dash use, one without — and confirm the em-dash check's behavior differs accordingly in your dry-run trace.

---

## Phase 2: Add a protected-content rule (Finding #4, High/High)

### Task 2.1 — Add a "Protected Content" section to `SKILL.md`, before the Banned Phrases section

State explicitly: before applying any edit, identify and never alter: direct quotations and block quotes, cited/paraphrased source material already under citation, code blocks and inline code, URLs, proper nouns and product names, and legal/compliance boilerplate. If a banned phrase or flagged vocabulary word appears inside protected content, list it in the report as "found in protected content - not modified" rather than silently skipping or silently fixing it.

### Task 2.2 — Reference the rule from the Workflow

Add a step between "read the target text" and "run every check": "Identify protected spans (per Protected Content) before collecting findings. Exclude them from edits; still surface them in the report per Task 2.1."

**Verification:** Run against a synthetic document containing a quoted passage that uses a banned phrase (e.g. a quote containing "game-changer") and confirm the report flags-without-editing.

---

## Phase 3: Mark house-style checks explicitly (Finding #11, Medium/Medium)

Depends on Phase 1 (the `--voice` mechanism these labels point to).

### Task 3.1 — Audit every check in Structural Checks and Use Sparingly

For each of: punctuation-personality check, rule-of-three limit, symmetrical-contrast entry, tag each as either **(house style — overridable by `--voice`)** or **(AI tell — always applies)**. The em-dash check is already correctly labeled; use its phrasing as the template.

### Task 3.2 — Apply the labels inline

Edit each entry to append its label, matching the existing em-dash check's "Skip this check entirely if..." pattern for house-style items.

**Verification:** Re-read the full Structural Checks and Use Sparingly sections and confirm every entry now carries one of the two labels with no ambiguous cases left.

---

## Phase 4: Add edge-case guardrails (Findings #5, #6, #14)

### Task 4.1 — Short-form and non-prose input handling (Finding #5, High/Medium)

Add a rule near the top of the Workflow: "If the target text is under ~300 words, or is not prose (a table, changelog, bullet-only outline, code, or structured data), skip document-level checks that require multiple paragraphs to evaluate: punctuation-personality, list-variety, and elegant-variation. State in the report which checks were skipped and why, rather than silently omitting them or forcing an irregularity into short text."

### Task 4.2 — Escape hatch for legitimate technical vocabulary (Finding #6, Medium/High)

Change "Flag and replace **any** occurrence" (AI Vocabulary section) to: "Flag any occurrence, but confirm it is being used as vague praise/filler rather than as a technical term of art (e.g. 'significantly' in a statistics context, 'robust' in an engineering-tolerance context, 'comprehensive' as a formal document classification) before replacing it. When in doubt, flag it in the report as a judgment call rather than auto-replacing."

### Task 4.3 — Non-English and metadata gap (Finding #14, Low/Medium)

Add one line near the top of `SKILL.md`: "This skill's checks are English-specific. If the target text is not primarily English, say so plainly in the report instead of returning an empty findings list." Add `license: MIT` to frontmatter if the schema supports extra fields (check against other skills' frontmatter conventions in the repo first; skip silently if not supported).

**Verification:** Run against a 50-word Slack-message-length input and confirm document-level checks are skipped with a stated reason; run against a mixed-language paragraph and confirm the skill states its English-only scope instead of returning a false-clean report.

---

## Phase 5: Fix `--scan` gauge promise (Finding #7, Medium/High)

### Task 5.1 — Add a scan-mode output template

In the Output Format section, add a distinct `--scan` variant (not just "omit the changes made framing") with a normalized figure: findings-per-1,000-words, plus stated bands (e.g. "0-2: reads as human-authored; 3-7: some tells present; 8+: heavily AI-flavored"). Pick band thresholds conservatively and note in the skill file that they're a heuristic, not a validated detector.

### Task 5.2 — Align `README.md`'s "gauge" claim

Update the README's `--scan` description to reference the new normalized figure so the promise and the output match.

**Verification:** Confirm the Output Format section now has two clearly distinct named templates (edit-mode, scan-mode) and that both are internally consistent with their respective workflow steps.

---

## Phase 6: Specify the application step (Finding #10, Medium/Medium)

### Task 6.1 — Tighten Workflow step on applying fixes

Replace "apply fixes directly" with: "Apply fixes as targeted, minimal edits to the flagged span only — never a full-document rewrite. After all edits are applied, re-scan the result once to confirm no new banned phrase or AI-vocabulary hit was introduced by the edits themselves. If a second run is invoked on already-processed text, treat it as a fresh pass: only flag remaining issues, and do not add a new punctuation irregularity if the punctuation-personality check already passed in this document."

**Verification:** Trace through a two-pass scenario by hand (run humanify, then run it again on the output) and confirm the instructions prevent both accumulating irregularities and re-flagging resolved issues.

---

## Phase 7: Progressive disclosure and de-duplication (Findings #8, #9, #12, #13)

Do this phase last — it's a token-efficiency and structure pass over content whose correctness was fixed in Phases 1-6, so extracting it earlier would mean re-editing moved content.

### Task 7.1 — Move the Output Format templates to `references/report-format.md` (Finding #8)

Create `references/report-format.md` containing both the edit-mode and scan-mode templates from Phase 5. Replace the inline Output Format section in `SKILL.md` with: "See `references/report-format.md` for the edit-mode and scan-mode report templates; read it before producing the final report."

### Task 7.2 — Collapse the Arguments section (Finding #8)

Replace the full Arguments list with a one-line pointer: "See `help.md` for the full argument list (`--scan`, `--voice`, `--help`)." Keep only the `--help` line's own self-reference, since it must remain reachable without a circular read.

### Task 7.3 — Split the AI Vocabulary catalogue (Finding #9)

Move the "Second tier" vocabulary list to `references/tells.md`. Keep "Core AI tells (highest priority)" and the full Banned Phrases / Use Sparingly sections inline, since they are higher-signal and the review's stated risk (a missed reference silently degrades detection) argues for keeping the most load-bearing content directly in `SKILL.md`. Add an unconditional Workflow step: "Read `references/tells.md` before step 2 (running checks) — do not treat this as optional or skip it under time pressure."

### Task 7.4 — De-duplicate cross-listed terms and argument lists (Finding #12)

- Remove `boasts`, `commitment to`, `renowned`, `groundbreaking` from wherever they're duplicated between Marketing Speak / Copula Substitution and AI Vocabulary; keep each in exactly one place and cross-reference ("see AI Vocabulary" or similar) rather than repeating the full entry.
- Ensure `README.md` states the argument list once, briefly, and defers to `help.md` for the authoritative version; `SKILL.md` remains the source of truth for behavior, `help.md` for the printed text.

### Task 7.5 — Add a lightweight index (Finding #13)

Once 7.1-7.3 shrink `SKILL.md`, add a short table of contents near the top listing the `##` sections, so a contributor can jump to the right category before adding a new phrase.

**Verification:** Confirm `SKILL.md`'s line count has dropped meaningfully (target: under 100 lines) with no loss of detection coverage — cross-check every removed/moved item is still reachable via the new reference files and the unconditional read instruction in Task 7.3.

---

## Sequencing note

Phases 1-6 change *content and correctness*; Phase 7 changes *file organization* around that same content. Doing 7 first would mean re-locating text that Phases 1-6 are still editing. If time is constrained, Phases 1, 2, and 4 are the highest-leverage (voice calibration, protected content, edge cases) and can ship independently of 3, 5, 6, 7.

## Next Steps (beyond this plan)

- Consider a `--diff` output mode showing exact before/after spans rather than a category-grouped summary. [Claude's idea]
- Consider whether `references/examples.md` (worked before/after pairs for the hardest categories — elegant variation, tailing negations) is worth adding once Phase 7's structure is in place; the original review flagged this (its #3) as high-value but it wasn't included above because it's additive content rather than a fix to something broken, and can be scoped separately once voice-calibration (Phase 1) exists to anchor which "after" is correct. [Claude's idea]
