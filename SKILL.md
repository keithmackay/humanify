---
name: humanify
description: Use when editing AI-drafted or AI-assisted prose to strip out AI writing tells - banned clichés, AI-favored vocabulary, copula substitution, participle-ending sentences, elegant variation, and other patterns that read as machine-generated. Works on any long-form text (articles, posts, emails, docs).
---

# Humanify

Detects and rewrites the linguistic patterns that make text read as AI-generated, without flattening the author's actual voice. This is a redline pass, not a rewrite from scratch: preserve the argument, the structure, and any intentional stylistic choices already present. Only touch what's flagged below.

## Arguments

- `/humanify` (no args) - Run the full pass on the target text (the file or pasted content the user points to)
- `/humanify --scan` - Report findings only; do not edit the text. Useful for a quick read on how "AI" a draft sounds before deciding whether to revise.
- `/humanify --help` - Do not run any other part of this skill. Read and display the contents of `help.md` (in this skill's folder) verbatim, then stop.

## Workflow

1. **Resolve the target document.** If the user's invocation or the current conversation already names a specific file or pasted block of text, confirm it before proceeding: "Running humanify against `<path or description>` - confirm?" If no target was specified, ask the user which file or pasted text to check. Never guess or scan an entire project for candidate documents.
2. **Read the target text in full** before touching anything - patterns like elegant variation and copula substitution require paragraph-level context, not line-by-line editing.
3. **Run every check below** against the text and collect findings (phrase, location, suggested fix).
4. If invoked as `/humanify --scan`, stop here and output the findings report (see Output Format).
5. Otherwise, apply fixes directly, preserving the author's argument and any deliberate stylistic touches (informal asides, humor, first-person process notes - these are features, not bugs, and should never be stripped).
6. Report what changed, grouped by category, in the Output Format below.

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

- **"It's not X, it's Y"** (unhedged form) - fine occasionally, monotonous as a recurring crutch
- **"As X continues to..."** - vague temporal filler; prefer a concrete development or date
- **"From X to Y" ranges** - even when X and Y are on a meaningful scale, overuse flattens the prose; reserve for when the range itself is the point
- **Rule-of-three constructions** ("adapt, innovate, and transform") - one per document at most; AI defaults to triads reflexively, so vary the count
- **Symmetrical contrasts** ("Technology creates possibilities. People create value.") - punchy the first time, formulaic the third; don't build a document's structure around repeating the pattern

## AI Vocabulary - Words to Actively Avoid

These words appear at significantly higher rates in AI-generated text. Flag and replace any occurrence:

**Core AI tells (highest priority):**
vibrant, crucial, significantly, testament, delve, showcase, tapestry, nuanced, multifaceted, pivotal, transformative, robust (as vague praise), leveraging (as filler), comprehensive, innovative (as generic praise), meticulous/meticulously, intricate/intricacies

**Second tier:**
align/aligns with, bolstered, cultivating, fostering, garnered, interplay, landscape (abstract use), enduring, boasts, nestled, diverse array, renowned, groundbreaking, encompasses, commitment to, highlighting, underscoring, emphasizing, ensuring, reflecting, symbolizing, contributing to

**Verb attribution tells:**
- Prefer "says" over "notes" or "argues" when simple attribution is accurate
- "Notes" over "says" is an AI signature; humans say "says"

## Structural Checks

- **Copula check** - no "serves as / stands as / marks / represents" where "is" would be cleaner
- **Participle-ending check** - no sentences closing with "...contributing to / fostering / ensuring / reflecting [vague outcome]"
- **Punctuation personality check** - the document should contain at least one intentional irregularity (fragment, ellipsis, parenthetical aside); grammatically flawless text is itself an AI tell
- **Elegant variation check** - no synonym chains for the same person or entity within a paragraph (protagonist / main character / central figure / hero); repeat the noun or use a pronoun
- **List variety check** - not every bulleted list has exactly three items; vary lengths deliberately (two, four, five, or none)
- **No fragmented headers** - no section that opens with a one-line restatement of its own heading before real content begins (e.g., "## Performance" followed by "Speed matters." followed by actual content - cut the restatement)
- **Em-dash check** - if the target voice avoids em-dashes (—), replace with colons, periods, parentheses, or a double-hyphen (`--`) used as a deliberate stylistic choice. Skip this check entirely if the author's established voice uses em-dashes freely; it is a house-style preference, not a universal rule.

## Output Format

```
HUMANIFY REPORT: [target file/text]

BANNED PHRASES FOUND: [N]
- "[phrase]" at [location] -> [suggested fix or "removed"]

AI VOCABULARY FLAGGED: [N]
- "[word]" at [location] -> [replacement]

STRUCTURAL ISSUES: [N]
- [check name]: [location] -> [what was done]

UNCHANGED (flagged but left as-is, with reason):
- [item]: [reason - e.g. "part of author's established voice"]

SUMMARY: [N] changes made across [N] categories.
```

If invoked with `--scan`, omit the "changes made" framing and instead report every finding with a recommendation, without applying anything.
