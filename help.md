humanify - Detect and rewrite AI writing tells in any long-form text.

USAGE
  /humanify                Resolves the target text (confirms it if a file was
                            already named, asks if not), reads it in full, marks
                            protected spans (quotes, code, URLs, proper nouns)
                            as off-limits, runs every check (banned phrases, AI
                            vocabulary, copula substitution, participle endings,
                            elegant variation, list variety, punctuation
                            personality), applies targeted edits to the flagged
                            spans while preserving the author's argument and
                            deliberate stylistic choices, and reports what
                            changed.

  /humanify --dry-run      Same checks, but reports findings only - no edits are
                            applied. Includes a findings-per-1,000-words figure
                            so you can gauge how "AI" a draft sounds before
                            deciding whether to revise. The figure is a
                            heuristic, not a validated detector.

  /humanify --voice <file> Reads the given file(s) as a calibration sample of
                            the author's own prior writing, derives an
                            allow-list of habitual constructions (em-dash use,
                            fragment frequency, rule-of-three, symmetrical
                            contrast), and suppresses any house-style check that
                            would flag a construction the author uses
                            deliberately and repeatedly. Combines with the other
                            modes.

  /humanify --help          Prints this summary. Takes no other action.

NOTES
  Checks are English-specific. Documents under ~300 words, and non-prose input
  (tables, changelogs, code, bullet-only outlines), skip the document-level
  checks; the report says which were skipped and why.

See the full skill file for the complete banned-phrase list, AI-vocabulary tiers,
and structural check definitions.
