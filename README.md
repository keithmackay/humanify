# humanify

A Claude Code skill that detects and rewrites the linguistic patterns that make AI-drafted prose read as machine-generated - banned clichés, AI-favored vocabulary, copula substitution, participle-ending sentences, elegant variation, and more. Works on English-language long-form text: articles, posts, emails, docs.

## Highlights

- **Banned phrase list** - clichéd pivots, weak contrast constructions, significance inflation, throat-clearing, faux-revelation transitions, and more
- **AI vocabulary scan** - flags words that appear at significantly higher rates in AI-generated text (vibrant, crucial, delve, tapestry, nuanced, transformative...) with a ranked priority tier, applied only when they're vague praise rather than precise technical usage
- **Structural checks** - copula substitution, participle endings, elegant variation chains, list-length uniformity, fragmented headers, punctuation personality
- **Voice calibration** - `--voice <file-or-text>` calibrates house-style checks (em-dash use, punctuation personality, rule-of-three limits) to the author's established writing; without it, those checks default to skip instead of guessing
- **Protected content** - quotations, code, URLs, proper nouns, and legal boilerplate are never edited, even if they contain a flagged phrase
- **Non-destructive scan mode** - `--scan` reports findings without editing, with a findings-per-1,000-words density gauge so you can judge how "AI" a draft sounds regardless of its length

## Getting Started

### Installation

```bash
git clone https://github.com/keithmackay/humanify.git

# Global install (available in all projects)
ln -s "$(pwd)/humanify" ~/.claude/skills/humanify

# Project-local install (available only in one project)
ln -s "$(pwd)/humanify" /path/to/your/project/.claude/skills/humanify
```

## Usage

```
/humanify                          # scan and rewrite the target text
/humanify --scan                   # report findings only, no edits
/humanify --voice <file-or-text>   # calibrate house-style checks to the author's voice
/humanify --help                   # prints help.md verbatim, takes no other action
```

See `SKILL.md` for the full list of banned phrases, AI vocabulary tiers, and structural checks, and `help.md` for the full argument reference.

## Contributing

Pull requests are welcome - fork the repo, make your change, and open a PR describing what it does and why.

## License

[MIT](LICENSE)
