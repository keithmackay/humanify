humanify - Detect and rewrite AI writing tells in English-language long-form text.

USAGE
  /humanify                Reads the target text in full, scopes which checks apply
                            (see SCOPING), identifies protected spans that must never
                            be edited, runs every check (banned phrases, AI vocabulary,
                            copula substitution, participle endings, elegant variation,
                            list variety, punctuation personality), applies fixes as
                            surgical in-place edits while preserving the author's
                            argument and deliberate stylistic choices, and reports
                            what changed.

  /humanify --scan         Same checks, but reports findings only - no edits are
                            applied. Includes a findings-per-1,000-words density
                            gauge, so the count means something regardless of
                            document length.

  /humanify --voice <file-or-text>
                            Calibrates the house-style checks (em-dash use,
                            punctuation personality, rule-of-three limit, symmetrical
                            contrasts) to the author's established voice, sampled
                            from the given reference text. Combine with either mode
                            above. Without --voice, house-style checks default to
                            skip rather than guessing at a voice that wasn't given.

  /humanify --help          Prints this file verbatim. Takes no other action.

SCOPING
  English-language prose only - non-English input gets a plain statement to that
  effect, not a silent "clean" report. Document-level checks (punctuation
  personality, list variety, elegant variation) are skipped on short-form
  (<~300 words) or non-prose (tables, changelogs, outlines) input.

PROTECTED CONTENT
  Direct quotations, block quotes, cited passages, code blocks/inline code, URLs,
  proper nouns/product names, and legal or compliance boilerplate are never edited,
  even if they contain a flagged phrase or word.

See SKILL.md for the complete banned-phrase list, AI-vocabulary tiers, and
structural check definitions; references/tells.md, references/examples.md, and
references/report-format.md for the extended catalogue, worked examples, and
report templates respectively.
