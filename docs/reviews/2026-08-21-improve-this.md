# improve-this review: humanify

**Date:** 2026-08-21
**Scope:** full project (`/Users/Keith.MacKay/Projects/humanify`)
**Project type:** prompt-only Claude Code skill — no source code, no tests, no manifest. Five files: `SKILL.md` (9.7 KB / ~1,490 words), `README.md`, `help.md`, `LICENSE`, `.gitignore`.
**Context:** recently extracted from `writeli` (a personal LinkedIn-writing skill) into a standalone, general-purpose skill. Evaluation weights generalization heavily.

**Categories evaluated:** Clarity & Simplification, Completeness, Accuracy & Consistency, Navigability & Structure, Redundancy, Edge Case Coverage, Token Efficiency & Progressive Disclosure.

---

## Part 1 — Priority List

```
#1   [Impact: High   | Confidence: High]   Completeness — No voice-calibration mechanism, though the skill repeatedly defers to "the author's established voice"
#2   [Impact: High   | Confidence: High]   Accuracy & Consistency — help.md is orphaned; --help has no instruction telling the skill to read it
#3   [Impact: High   | Confidence: High]   Completeness — Zero before/after examples for the hardest checks
#4   [Impact: High   | Confidence: High]   Edge Case Coverage — No protected-content rule (quotes, citations, code, proper nouns)
#5   [Impact: High   | Confidence: Medium] Edge Case Coverage — Short-form and non-prose inputs break several document-level checks
#6   [Impact: Medium | Confidence: High]   Accuracy & Consistency — "Flag and replace any occurrence" is absolutist; no legitimate-use escape hatch
#7   [Impact: Medium | Confidence: High]   Accuracy & Consistency — README promises a "how AI does this sound" gauge that --scan never produces
#8   [Impact: Medium | Confidence: High]   Token Efficiency — Output Format block and Arguments section load on every invocation
#9   [Impact: Medium | Confidence: Medium] Token Efficiency — The banned-phrase and vocabulary catalogues could move to references/, with real tradeoffs
#10  [Impact: Medium | Confidence: Medium] Clarity — Workflow step 4 never says *how* to apply edits, or what happens on a second run
#11  [Impact: Medium | Confidence: Medium] Edge Case Coverage — Residual house-style rules presented as universal (punctuation personality, em-dash)
#12  [Impact: Low    | Confidence: High]   Redundancy — The three-argument list is stated in three files; several terms appear in two lists
#13  [Impact: Low    | Confidence: High]   Navigability — Frontmatter is minimal; no headings index in a 148-line always-loaded file
#14  [Impact: Low    | Confidence: Medium] Completeness — No non-English guidance; no license/version metadata in frontmatter
```

---

## Part 2 — Categorized Breakdown

### Completeness

**#1 — No voice-calibration mechanism (Impact: High, Confidence: High)**

*Observed:* The skill defers to the author's voice in at least four places — `SKILL.md:8` ("without flattening the author's actual voice"), `:21` ("deliberate stylistic touches… are features, not bugs"), `:125` ("Skip this check entirely if the author's established voice uses em-dashes freely"), and the `UNCHANGED` output section whose example reason is literally `"part of author's established voice"`. But nothing in the skill establishes what that voice *is*. There is no `--voice <file>` argument, no convention for a `VOICE.md` or config file, and no workflow step instructing the model to sample the author's prior human-written work before editing.

*Why it matters:* This is the single largest gap created by the generalization from `writeli`. In the original skill, the author's voice was implicit — the skill was written by and for one person. Standalone, every voice-deferring instruction becomes unresolvable: the model must guess, and guessing defaults to flattening. That is precisely the failure mode the opening paragraph promises to avoid. The em-dash check is the clearest casualty — it has two opposite correct behaviors and no way to choose between them.

*Opportunity:* an explicit calibration input (a `--voice` argument pointing at reference samples, or a documented convention the skill reads if present), plus a workflow step that derives an allow-list of the author's habitual constructions before flagging anything.

**#3 — No before/after examples (Impact: High, Confidence: High)**

*Observed:* The skill defines every check in the abstract. Copula substitution (`:48-51`), elegant variation (`:122`), false ranges (`:74-75`), and tailing negations (`:38`) each get one or two lines of description and a parenthetical illustration at most. There is not one worked before/after rewrite anywhere in the repo.

*Why it matters:* Detection is the easy half; the skill's actual output is a *rewrite*. Categories like elegant variation and tailing negations require judgment about what to substitute, and abstract rules under-constrain that. A single before/after pair per hard category would anchor rewrite quality far more effectively than more prose describing the rule. This content is also naturally on-demand — it belongs in `references/examples.md`, consulted when a rewrite is actually being made rather than loaded on every invocation.

**#14 — Non-English text and metadata (Impact: Low, Confidence: Medium)**

*Observed:* Every banned phrase, vocabulary entry, and structural check is English-specific, but nothing says so. A user pointing the skill at Spanish or German prose gets undefined behavior — most likely a near-empty findings report read as "this draft is clean." Separately, the frontmatter carries only `name` and `description`: no `license`, no version, and no `allowed-tools`.

*Why it matters:* A false clean bill of health is worse than a refusal. One line scoping the skill to English (and telling it to say so plainly on non-English input) closes this cheaply. The metadata gap is cosmetic but relevant for a repo positioned for public reuse and contribution.

---

### Accuracy & Consistency

**#2 — `help.md` is orphaned (Impact: High, Confidence: High)**

*Observed:* `help.md` exists and is well-written, but no file references it. `SKILL.md:14` defines `--help` as "Prints this skill's summary and argument list" — pointing at *itself*, not at `help.md`. The Workflow section (`:16-22`) branches on `--scan` at step 3 but never mentions `--help` at all, so the documented no-op behavior has no place in the execution path.

*Why it matters:* This is a live correctness bug, not a doc nit. As written, `/humanify --help` will produce an ad-hoc summary generated from `SKILL.md` while a curated help text sits unused on disk and drifts out of sync. It is also the clearest missed piece of progressive disclosure in the repo: `help.md` is exactly the file that should be read on demand and never otherwise.

**#6 — Absolutist vocabulary rule (Impact: Medium, Confidence: High)**

*Observed:* `SKILL.md:105` instructs "Flag and replace **any** occurrence" across two tiers totaling roughly 40 words. Several are ordinary technical vocabulary: `robust`, `comprehensive`, `significantly`, `align`, `ensuring`, `encompasses`. Two entries are self-hedged (`robust (as vague praise)`, `landscape (abstract use)`) — a tacit admission that the others need the same qualifier and do not have it. In statistical or engineering writing, "significantly" and "comprehensive" are terms of art with no synonym.

*Why it matters:* Blanket replacement produces confident wrong edits in exactly the technical documents the generalized skill now claims to cover. The tension between "any occurrence" and the skill's own opening promise not to flatten voice is unresolved, and an editing skill resolves that ambiguity by editing.

**#7 — `--scan` has no gauge (Impact: Medium, Confidence: High)**

*Observed:* `README.md:10` and `SKILL.md:13` both sell `--scan` as a way to "gauge how 'AI' a draft sounds before deciding whether to revise." The Output Format (`:129-147`) delivers raw counts per category and a change tally — no score, no density normalization, no threshold guidance. `SKILL.md:147` tells the model to "omit the 'changes made' framing" under `--scan` but supplies no alternative header, so scan output is an improvised variant of a template designed for the edit path.

*Why it matters:* The stated decision the user makes with `--scan` output ("is this worth revising?") is not supported by what `--scan` returns. Eleven findings means something very different in a 300-word email than in a 4,000-word article, and nothing normalizes for that. A findings-per-thousand-words figure plus stated bands, or an explicit scan-mode template, would close the gap between promise and output.

---

### Edge Case Coverage

**#4 — No protected-content rule (Impact: High, Confidence: High)**

*Observed:* The skill instructs the model to read the full text and apply fixes, with no carve-out for content that must not be altered: direct quotations, block quotes, cited passages, code blocks and inline code, URLs, proper nouns, product names, and legal or compliance boilerplate. Several banned entries make collisions likely — `"commitment to"` is standard in mission statements the author may be quoting, `"Research shows"` may appear inside a quoted source, and `robust`/`comprehensive` are common in product names and API documentation.

*Why it matters:* Silently rewriting a quotation is a factual integrity failure, not a style preference, and it is unrecoverable from the skill's output alone. This is the highest-severity edge case in the review and among the cheapest to fix: one workflow step establishing untouchable spans before any edits are applied.

**#5 — Short-form and non-prose inputs (Impact: High, Confidence: Medium)**

*Observed:* The frontmatter and README scope the skill to "any long-form text," but the argument list, workflow, and checks impose no minimum and nothing handles a short input. At least three document-level checks degenerate below roughly 300 words. The punctuation-personality check (`:121`) requires "at least one intentional irregularity," which would have the skill inject a fragment into a four-line email. The list-variety check (`:123`) requires varied list lengths across a document that may contain one list. The elegant-variation check (`:122`) operates on synonym chains that need paragraphs to form.

*Why it matters:* Short-form is where users will most often reach for this — Slack messages, email replies, commit messages, bios. The unqualified "any long-form text" reads as an aspiration rather than a boundary, and there is no instruction to skip document-level checks when the input is too short to support them, nor a rule for non-prose inputs like tables, changelogs, or bullet-only outlines.

**#11 — House-style rules presented as universal (Impact: Medium, Confidence: Medium)**

*Observed:* The em-dash check (`:125`) is correctly and explicitly labeled "a house-style preference, not a universal rule" with a skip condition. The punctuation-personality check (`:121`) directly above it is equally a house preference — "grammatically flawless text is itself an AI tell" is defensible for LinkedIn posts and false for legal filings, academic papers, and API reference docs — but carries no such label and no skip condition. Similar residue appears in the rule-of-three limit and the symmetrical-contrast entry under Use Sparingly, both of which are voice judgments stated as document-wide quotas.

*Why it matters:* The em-dash entry proves the author already understands the distinction and applied it once. Extending that same treatment consistently — marking which checks are house style and which are genuine AI tells — is the remaining half of the `writeli` generalization, and it interacts directly with finding #1: house-style checks are exactly the ones a voice-calibration input should govern.

---

### Token Efficiency & Progressive Disclosure

**#8 — Situational content in the always-loaded file (Impact: Medium, Confidence: High)**

*Observed:* `SKILL.md` is loaded in full on every invocation. Three blocks within it are not needed on every invocation:

- **Output Format** (`:127-147`, ~19 lines) — a report template needed only at the reporting step, and only in whichever of the two shapes applies.
- **Arguments** (`:10-14`) — duplicates `help.md` and `README.md`; needed when the user asks how to invoke the skill, not while the skill is running.
- **`--help` behavior** — should be one line delegating to `help.md`, per finding #2.

*Why it matters:* This is the clean, low-risk portion of the progressive-disclosure work. Moving the Output Format to `references/report-format.md` (read at step 5) and collapsing Arguments to a pointer at `help.md` trims roughly 25 lines of always-loaded content with no behavioral risk, since none of it informs detection or rewriting.

**#9 — The catalogues, with genuine tradeoffs (Impact: Medium, Confidence: Medium)**

*Observed:* Banned Phrases (`:24-91`), Use Sparingly (`:93-101`), and AI Vocabulary (`:103-115`) together account for roughly 90 of 148 lines — the bulk of the file. In a conventional progressive-disclosure refactor, exhaustive lookup tables are the canonical candidate for `references/`.

*Why it matters, and why confidence is only Medium:* Unlike setup instructions, these catalogues *are* the detection logic. Every workflow run requires the complete list, so moving them out converts a guaranteed load into a load that depends on the model reliably following a pointer — and a missed reference means silently degraded detection, invisible in the output. The cost of a miss is higher than the cost of the tokens.

The defensible middle path: keep category headings and the highest-signal entries inline (the "Core AI tells" tier, the weak-contrast constructions), move the long tail of second-tier vocabulary and less common phrases to `references/tells.md`, and make Workflow step 2 an unconditional read of that file rather than a discretionary one. Worth noting that a Clarity pass on the remaining always-loaded content (finding #10, plus the redundancies in #12) recovers a meaningful share of the same tokens at zero detection risk, and should be sequenced first.

---

### Clarity & Simplification

**#10 — Under-specified application step (Impact: Medium, Confidence: Medium)**

*Observed:* Workflow step 4 (`:21`) says "apply fixes directly" without specifying mechanism or bounds. Three questions go unanswered: whether edits are surgical replacements or a whole-document rewrite (the opening paragraph says "redline pass, not a rewrite from scratch," but nothing in the workflow enforces that); whether the model re-reads the result to confirm the fixes did not introduce new tells; and what a second run on already-processed text should do.

*Why it matters:* The idempotency question is not hypothetical — the punctuation-personality check *adds* irregularities, so repeated runs could accumulate them, and each rewrite is a fresh opportunity to introduce the very vocabulary the skill bans. A rewriting skill with no verification step and no stopping condition can drift on iteration. The framing constraint ("redline, not rewrite") exists in the prose but has no corresponding operational instruction.

---

### Redundancy

**#12 — Duplicated argument lists and cross-listed terms (Impact: Low, Confidence: High)**

*Observed:* Two distinct duplications.

The three-argument list is written out in full in `SKILL.md:10-14`, `README.md:28-32`, and `help.md` — three copies with three chances to drift, and they have already drifted in tone (`SKILL.md` describes `--help` as printing "this skill's summary," `help.md` as printing "this summary").

Within `SKILL.md`, several terms appear in two lists: `boasts` is banned under Copula Substitution (`:50`) and listed again under second-tier AI Vocabulary (`:111`); `commitment to`, `renowned`, and `groundbreaking` appear under both Marketing Speak (`:61-64`) and AI Vocabulary (`:111`). Copula substitution itself is defined at `:48-51` and restated as a Structural Check at `:119`.

*Why it matters:* Low severity — duplicate flags produce the same edit, and the drift so far is cosmetic. But it inflates the always-loaded file (interacting with #8), and the copula duplication in particular means a future edit to one entry can leave the other stale and contradictory. `README.md` and `help.md` should each own one framing and defer for the rest.

---

### Navigability & Structure

**#13 — Minimal frontmatter, no index (Impact: Low, Confidence: High)**

*Observed:* `SKILL.md` is 148 lines of dense reference material organized as a flat sequence of `##` sections with `**bold**` sub-groupings and no top-level index. The frontmatter carries only `name` and `description`. `README.md:34` points readers to `SKILL.md` "for the full list" with no anchor.

*Why it matters:* Minor for machine consumption, real for the human contributors the README explicitly invites — a contributor adding a phrase must scan the whole file to find the right category and check it is not already listed elsewhere (see #12). The structural fix mostly falls out of the progressive-disclosure work in #8 and #9: once the catalogues live in `references/`, what remains in `SKILL.md` is short enough not to need an index.

---

## Summary

The skill's core content — the banned-phrase taxonomy and vocabulary tiers — is genuinely strong and clearly the product of real editing experience. Nearly every finding above concerns the *scaffolding* around that content rather than the content itself, and the concentration of issues around one theme is telling: the extraction from `writeli` moved the rules out but left behind the implicit context that made them resolvable. The three highest-leverage fixes are the voice-calibration input (#1), wiring up `help.md` (#2), and a protected-content rule (#4).
