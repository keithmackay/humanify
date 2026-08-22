humanify - Detect and rewrite AI writing tells in any long-form text.

USAGE
  /humanify                Resolves the target text (confirms it if a file was
                            already named, asks if not), reads it in full, runs
                            every check (banned
                            phrases, AI vocabulary, copula substitution, participle
                            endings, elegant variation, list variety, punctuation
                            personality), applies fixes while preserving the author's
                            argument and deliberate stylistic choices, and reports
                            what changed.

  /humanify --scan         Same checks, but reports findings only - no edits are
                            applied. Useful for a quick read on how "AI" a draft
                            sounds before deciding whether to revise.

  /humanify --help          Prints this summary. Takes no other action.

See the full skill file for the complete banned-phrase list, AI-vocabulary tiers,
and structural check definitions.
