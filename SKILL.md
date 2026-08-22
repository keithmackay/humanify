---
name: humanify
description: Use when editing AI-drafted or AI-assisted prose to strip out AI writing tells - banned clichés, AI-favored vocabulary, copula substitution, participle-ending sentences, elegant variation, and other patterns that read as machine-generated. Works on English-language long-form text (articles, posts, emails, docs).
license: MIT
---

# Humanify

Detects and rewrites the linguistic patterns that make text read as AI-generated, without flattening the author's actual voice. This is a redline pass, not a rewrite from scratch: preserve the argument, the structure, and any intentional stylistic choices already present. Only touch what's flagged below.

**Scope:** English-language prose only. If the target text is not primarily English, say so plainly in the report and skip the checks below rather than returning a silent "clean" result.

**Index:** Arguments · Workflow · Banned Phrases · Use Sparingly · AI Vocabulary · Structural Checks. Extended catalogue: `references/tells.md`. Worked examples: `references/examples.md`. Report templates: `references/report-format.md`.

## Arguments

- `/humanify` - run the full pass (see Workflow)
- `/humanify --scan` - findings only, no edits (see Workflow step 6)
- `/humanify --voice <file-or-text>` - calibrate house-style checks to the author's established voice; combine with either mode above
- `/humanify --help` - read and print `help.md` verbatim; take no other action

Full descriptions: `help.md`.

## Workflow

1. **Read the target text in full** before touching anything - patterns like elegant variation and copula substitution require paragraph-level context, not line-by-line editing. Note the approximate word count and whether the input is prose or non-prose (table, changelog, bullet-only outline).

2. **Scope document-level checks.** Punctuation-personality, list-variety, and elegant-variation are document-level checks: they need enough material to judge. If the input is under ~300 words of prose, or is non-prose, skip these three checks and note them in the report as "skipped (short-form/non-prose)" rather than forcing an edit onto text that can't support the check (e.g. injecting a fragment into a four-line email).

3. **Calibrate to the author's voice.** If invoked with `--voice <file-or-text>`, read the reference sample(s) and derive an allow-list of the author's habitual constructions (em-dash use, sentence-fragment tolerance, list-length habits, triads, and similar). The house-style checks below (em-dash, punctuation-personality, rule-of-three limit, symmetrical contrasts) apply only within that allow-list. If `--voice` is not given, default all house-style checks to **skip** - do not guess at a voice that wasn't provided.

4. **Identify protected spans** before running any check: direct quotations and block quotes, cited passages, code blocks and inline code, URLs, proper nouns and product names, and legal/compliance boilerplate. No check or edit may alter content inside these spans. A banned phrase or flagged word found only inside a protected span is reported under `UNCHANGED` with reason "protected span" - never edited.

5. **Run every check below against the unprotected text**, plus the complete catalogue in `references/tells.md` (read unconditionally - it's detection logic, not background context, so a skipped read means silently degraded detection). Collect findings as (phrase, location, suggested fix). For the hardest rewrite categories - copula substitution, elegant variation, tailing negations, false ranges, participle-ending sentences - consult `references/examples.md` for a worked before/after pattern before proposing a fix.

6. If invoked as `/humanify --scan`, stop here and output the findings report using the scan template in `references/report-format.md`.

7. **Otherwise, apply fixes as surgical in-place replacements** at each flagged location - never a whole-document rewrite. Preserve the author's argument and any deliberate stylistic touches already confirmed by voice calibration or evident in the text (informal asides, humor, first-person process notes - these are features, not bugs). After editing, re-check the edited passages to confirm no new tells were introduced: a second run over already-processed text should find near-zero new findings, and the punctuation-personality check must never *add* a new irregularity if one already exists or the author's voice doesn't call for it.

8. Report what changed, grouped by category, using the edit-path template in `references/report-format.md`.

## Banned Phrases

Do not use these, or close variants of them. The entry shown per category is the highest-signal example; the complete list per category is in `references/tells.md`.

**Clichéd pivots and openers:**
- "Here's where it gets interesting:" / "Here's the uncomfortable truth:" - overused; write a pivot specific to the argument

**Weak contrast constructions:**
- "Not only...but also" - one of the most reliable AI detection signals; restructure the contrast

**Significance inflation:**
- "Game-changer" / "game-changing" - overused to meaninglessness

**Copula substitution (AI avoids "is/are"):**
- "Serves as" / "stands as" / "marks" / "represents" when "is" would be simpler - use "is"
- "Boasts" / "features" / "offers" when "has" would be simpler - use "has"
- Test: if replacing the verb with "is" or "has" makes the sentence cleaner, that's the right version

**Persuasive authority ceremony:**
- "At its core" - ceremony before ordinary points

**Vague attribution:**
- "Experts argue" / "Industry reports suggest" / "Observers have noted" / "Some critics argue" / "Research shows" - unnamed authority laundering; cite a real source or cut it

**Marketing speak:**
- "Commitment to" - PR boilerplate

**Throat-clearing:**
- "It is worth noting that" / "It is important to note" - just say the thing

**Condescending confirmatory:**
- "actually" - reads as superior and slightly annoying rather than confirmatory; overused in AI-generated text as a rhetorical pivot ("This actually means...", "You actually need to..."). Cut it or restructure the sentence.

**False ranges:**
- "From X to Y, from A to B" constructions where X and Y aren't on a meaningful scale ("from startup garages to enterprise boardrooms") - list the topics directly

**Generic positive conclusions:**
- "The future looks bright" / "Exciting times lie ahead" / "This represents a major step in the right direction" - replace with something specific

**Faux-revelation transitions:**
- "What this means is..." - just state the implication directly

**Prescriptive imperatives:**
- "Organizations must..." - generic corporate command; name who, specifically, and what they'd actually do

## Use Sparingly

Not banned outright - legitimate but overused in AI text. One per document, at most, and only when it's genuinely the sharpest way to make the point:

- **"It's not X, it's Y"** (unhedged form) - fine occasionally, monotonous as a recurring crutch
- **"As X continues to..."** - vague temporal filler; prefer a concrete development or date
- **"From X to Y" ranges** - even when X and Y are on a meaningful scale, overuse flattens the prose; reserve for when the range itself is the point
- **Rule-of-three constructions** ("adapt, innovate, and transform") - *house-style preference, not a universal rule.* AI defaults to triads reflexively, but some authors' voice genuinely runs this way. Skip this limit unless `--voice` calibration (Workflow step 3) shows the author doesn't use triads, or the user opts in explicitly.
- **Symmetrical contrasts** ("Technology creates possibilities. People create value.") - *house-style preference, not a universal rule.* Punchy the first time, formulaic the third - but some authors build on it deliberately. Skip this limit unless `--voice` calibration shows otherwise.

## AI Vocabulary - Words to Actively Avoid

These words appear at significantly higher rates in AI-generated text, but several are also ordinary technical or domain vocabulary. Flag an occurrence only when the word is functioning as vague praise, filler, or unearned emphasis - not when it's precise, domain-standard usage (e.g. "significant at p<0.05," "comprehensive test coverage," "a robust API"). The full second-tier list is in `references/tells.md`.

**Core AI tells (highest priority):**
vibrant, crucial, significantly (as vague praise), testament, delve, showcase, tapestry, nuanced, multifaceted, pivotal, transformative, robust (as vague praise), leveraging (as filler), comprehensive (as vague praise), innovative (as generic praise), meticulous/meticulously, intricate/intricacies

**Verb attribution tells:**
- Prefer "says" over "notes" or "argues" when simple attribution is accurate
- "Notes" over "says" is an AI signature; humans say "says"

## Structural Checks

- **Copula check** - see Banned Phrases → Copula Substitution above; same rule, no restatement here
- **Participle-ending check** - no sentences closing with "...contributing to / fostering / ensuring / reflecting [vague outcome]"
- **Punctuation personality check** - *house-style preference, not a universal rule.* Skip by default (Workflow step 3). Apply only when `--voice` calibration shows the author's prior writing tolerates irregularity (fragments, ellipses, parentheticals) - grammatically flawless text is not itself an AI tell in every domain (legal filings, academic papers, API docs are meant to be flawless).
- **Elegant variation check** - no synonym chains for the same person or entity within a paragraph (protagonist / main character / central figure / hero); repeat the noun or use a pronoun. Subject to the short-form/non-prose scoping in Workflow step 2.
- **List variety check** - not every bulleted list has exactly three items; vary lengths deliberately (two, four, five, or none). Subject to the short-form/non-prose scoping in Workflow step 2.
- **No fragmented headers** - no section that opens with a one-line restatement of its own heading before real content begins (e.g., "## Performance" followed by "Speed matters." followed by actual content - cut the restatement)
- **Em-dash check** - *house-style preference, not a universal rule.* Skip by default (Workflow step 3). If `--voice` calibration shows the target voice avoids em-dashes (—), replace with colons, periods, parentheses, or a double-hyphen (`--`) used as a deliberate stylistic choice. Skip entirely if the author's established voice uses em-dashes freely.
