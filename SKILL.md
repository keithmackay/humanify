---
name: humanify
description: Use when editing AI-drafted or AI-assisted prose to strip out AI writing tells - banned clichés, AI-favored vocabulary, copula substitution, participle-ending sentences, elegant variation, and other patterns that read as machine-generated. Works on any long-form text (articles, posts, emails, docs).
license: MIT
---

# Humanify

Detects and rewrites the linguistic patterns that make text read as AI-generated, without flattening the author's actual voice. This is a redline pass, not a rewrite from scratch: preserve the argument, the structure, and any intentional stylistic choices already present. Only touch what's flagged below.

This skill's checks are English-specific. If the target text is not primarily English, say so plainly in the report instead of returning an empty findings list.

**Sections:** [Arguments](#arguments) · [Workflow](#workflow) · [Protected Content](#protected-content) · [Banned Phrases](#banned-phrases) · [Use Sparingly](#use-sparingly) · [AI Vocabulary](#ai-vocabulary---words-to-actively-avoid) · [Structural Checks](#structural-checks) · [Output Format](#output-format)

## Arguments

See `help.md` for the full argument list (`--dry-run`, `--voice`, `--help`).

- `/humanify --help` - Do not run any other part of this skill. Read and display the contents of `help.md` (in this skill's folder) verbatim, then stop.

## Workflow

0. **Calibrate voice.** If `--voice` was provided, read the calibration sample(s) first and note which house-style checks (those labelled *house style* below) it overrides. If no sample was provided, apply house-style checks at their documented default (flag them) and say so in the report's UNCHANGED section rationale, e.g. "em-dash retained - no --voice sample provided to confirm authorial intent."
1. **Resolve the target document.** If the user's invocation or the current conversation already names a specific file or pasted block of text, confirm it before proceeding: "Running humanify against `<path or description>` - confirm?" If no target was specified, ask the user which file or pasted text to check. Never guess or scan an entire project for candidate documents.
2. **Read the target text in full** before touching anything - patterns like elegant variation and copula substitution require paragraph-level context, not line-by-line editing.
3. **Check scope and size.** If the target text is under ~300 words, or is not prose (a table, changelog, bullet-only outline, code, or structured data), skip document-level checks that require multiple paragraphs to evaluate: punctuation-personality, list-variety, and elegant-variation. State in the report which checks were skipped and why, rather than silently omitting them or forcing an irregularity into short text.
4. **Identify protected spans** (per [Protected Content](#protected-content)) before collecting findings. Exclude them from edits; still surface them in the report as "found in protected content - not modified."
5. **Read `references/tells.md`** before running checks - do not treat this as optional or skip it under time pressure.
6. **Run every check below** against the text and collect findings (phrase, location, suggested fix).
7. If invoked as `/humanify --dry-run`, stop here and output the dry-run report (see [Output Format](#output-format)).
8. Otherwise, **apply fixes as targeted, minimal edits to the flagged span only** - never a full-document rewrite. Preserve the author's argument and any deliberate stylistic touches (informal asides, humor, first-person process notes - these are features, not bugs, and should never be stripped). After all edits are applied, re-scan the result once to confirm no new banned phrase or AI-vocabulary hit was introduced by the edits themselves. If a second run is invoked on already-processed text, treat it as a fresh pass: only flag remaining issues, and do not add a new punctuation irregularity if the punctuation-personality check already passed in this document.
9. Report what changed, grouped by category, in the format described under [Output Format](#output-format).

## Protected Content

Before applying any edit, identify and never alter:

- direct quotations and block quotes
- cited or paraphrased source material already under citation
- code blocks and inline code
- URLs
- proper nouns and product names
- legal and compliance boilerplate

If a banned phrase or flagged vocabulary word appears inside protected content, list it in the report as "found in protected content - not modified" rather than silently skipping or silently fixing it.

## Banned Phrases

Do not use these, or close variants of them:

**Clichéd pivots and openers:**
- "Here's where it gets interesting:" / "Here's the uncomfortable truth:" - overused; write a pivot specific to the argument
- "Let's dive in" / "Let's explore" / "Let's break this down" / "Here's what you need to know" / "Without further ado" - signposting; just start doing the thing
- "In today's rapidly changing landscape" / "Evolving landscape" - consultant filler
- "At the end of the day" - exhausted idiom

**Weak contrast constructions:**
- "It's not just about X, it's about Y" - weak contrast; all variants prohibited
- "Not only...but also" - one of the most reliable AI detection signals; restructure the contrast
- "Not just X, but Y" / "It's not just X, it's Y" - same pattern, different phrasing; all prohibited
- Tailing negations: clipped "...no guessing", "...no wasted motion" fragments tacked onto sentence ends - rewrite as a full clause

**Significance inflation:**
- "Game-changer" / "game-changing" - overused to meaninglessness
- "Serving as a [noun]" - "serving as a testament", "serving as a beacon", "serving as a reminder" - find a specific image instead
- "Setting the stage for" - inflated transition
- "Key turning point" / "indelible mark" / "deeply rooted" - AI significance puffery
- "[X] underscores the importance of [Y]" - cut or restate directly
- "Moving forward" / "Going forward" - empty transition

**Copula substitution (AI avoids "is/are"):**
- "Serves as" / "stands as" / "marks" / "represents" when "is" would be simpler - use "is"
- "Boasts" / "features" / "offers" when "has" would be simpler - use "has"
- Test: if replacing the verb with "is" or "has" makes the sentence cleaner, that's the right version

**Persuasive authority ceremony:**
- "At its core" - ceremony before ordinary points
- "What really matters" / "The real question is" / "Fundamentally" / "The heart of the matter" - same pattern
- These phrases promise depth; the sentence that follows usually doesn't deliver it

**Vague attribution:**
- "Experts argue" / "Industry reports suggest" / "Observers have noted" / "Some critics argue" / "Research shows" - unnamed authority laundering; cite a real source or cut it

**Marketing speak:**
- "Unlock" used as a verb for abstract things ("unlock your potential", "unlock value")
- "Vibrant" / "groundbreaking" / "renowned" / "breathtaking" as generic praise - find specific language
- "Commitment to" - PR boilerplate

**Throat-clearing:**
- "It is worth noting that" / "It is important to note" - just say the thing
- "In conclusion" / "To summarize" before a closing section - the structure signals it; no announcement needed
- "Additionally" / "Furthermore" / "Moreover" as paragraph openers - find a real transition or cut

**Condescending confirmatory:**
- "actually" - reads as superior and slightly annoying rather than confirmatory; overused in AI-generated text as a rhetorical pivot ("This actually means...", "You actually need to..."). Cut it or restructure the sentence.

**False ranges:**
- "From X to Y, from A to B" constructions where X and Y aren't on a meaningful scale ("from startup garages to enterprise boardrooms") - list the topics directly

**Generic positive conclusions:**
- "The future looks bright" / "Exciting times lie ahead" / "This represents a major step in the right direction" - replace with something specific
- "The future belongs to..." - vague futurism; name the actual mechanism instead
- "The path forward is clear." - false certainty; earn the conclusion or cut the claim
- "One thing is certain." - hedge dressed as conviction; state the point directly

**Faux-revelation transitions:**
- "What this means is..." - just state the implication directly
- "Put simply..." - if it needs saying, just say it simply; don't announce the simplicity
- "In other words..." - restate with new information or cut; don't just rephrase
- "Let that sink in" - tells the reader how to react instead of earning the reaction
- "Here's what struck me most" - manufactured intimacy; describe the specific reaction instead

**Prescriptive imperatives:**
- "Organizations must..." - generic corporate command; name who, specifically, and what they'd actually do

## Use Sparingly

Not banned outright - these constructions are legitimate but overused in AI text. One per document, at most, and only when it's genuinely the sharpest way to make the point:

- **"It's not X, it's Y"** (unhedged form) - fine occasionally, monotonous as a recurring crutch. *(AI tell - always applies.)*
- **"As X continues to..."** - vague temporal filler; prefer a concrete development or date. *(AI tell - always applies.)*
- **"From X to Y" ranges** - even when X and Y are on a meaningful scale, overuse flattens the prose; reserve for when the range itself is the point. *(AI tell - always applies.)*
- **Rule-of-three constructions** ("adapt, innovate, and transform") - one per document at most; AI defaults to triads reflexively, so vary the count. *(House style - overridable by `--voice`. Skip this check entirely if the calibration sample shows the author uses triads deliberately and repeatedly.)*
- **Symmetrical contrasts** ("Technology creates possibilities. People create value.") - punchy the first time, formulaic the third; don't build a document's structure around repeating the pattern. *(House style - overridable by `--voice`. Skip this check entirely if the calibration sample shows the author uses symmetrical contrast as a signature move.)*

## AI Vocabulary - Words to Actively Avoid

These words appear at significantly higher rates in AI-generated text. Flag any occurrence, but confirm it is being used as vague praise or filler rather than as a technical term of art (e.g. "significantly" in a statistics context, "robust" in an engineering-tolerance context, "comprehensive" as a formal document classification) before replacing it. When in doubt, flag it in the report as a judgment call rather than auto-replacing.

**Core AI tells (highest priority):**
vibrant, crucial, significantly, testament, delve, showcase, tapestry, nuanced, multifaceted, pivotal, transformative, robust (as vague praise), leveraging (as filler), comprehensive, innovative (as generic praise), meticulous/meticulously, intricate/intricacies

**Second tier:** see `references/tells.md` (read it in Workflow step 5; it is not optional).

**Verb attribution tells:**
- Prefer "says" over "notes" or "argues" when simple attribution is accurate
- "Notes" over "says" is an AI signature; humans say "says"

## Structural Checks

- **Copula check** - no "serves as / stands as / marks / represents" where "is" would be cleaner. *(AI tell - always applies.)*
- **Participle-ending check** - no sentences closing with "...contributing to / fostering / ensuring / reflecting [vague outcome]". *(AI tell - always applies.)*
- **Punctuation personality check** - the document should contain at least one intentional irregularity (fragment, ellipsis, parenthetical aside); grammatically flawless text is itself an AI tell. *(House style - overridable by `--voice`. Skip this check entirely if the calibration sample shows the author writes uniformly formal, irregularity-free prose.)*
- **Elegant variation check** - no synonym chains for the same person or entity within a paragraph (protagonist / main character / central figure / hero); repeat the noun or use a pronoun. *(AI tell - always applies.)*
- **List variety check** - not every bulleted list has exactly three items; vary lengths deliberately (two, four, five, or none). *(AI tell - always applies.)*
- **No fragmented headers** - no section that opens with a one-line restatement of its own heading before real content begins (e.g., "## Performance" followed by "Speed matters." followed by actual content - cut the restatement). *(AI tell - always applies.)*
- **Em-dash check** - if the target voice avoids em-dashes (—), replace with colons, periods, parentheses, or a double-hyphen (`--`) used as a deliberate stylistic choice. *(House style - overridable by `--voice`. Skip this check entirely if the author's established voice uses em-dashes freely.)*

## Output Format

See `references/report-format.md` for the edit-mode and dry-run report templates; read it before producing the final report.
