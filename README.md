# humanify

A Claude Code skill that detects and rewrites the linguistic patterns that make AI-drafted prose read as machine-generated - banned clichés, AI-favored vocabulary, copula substitution, participle-ending sentences, elegant variation, and more. Works on any long-form text: articles, posts, emails, docs.

## Highlights

- **Banned phrase list** - clichéd pivots, weak contrast constructions, significance inflation, throat-clearing, faux-revelation transitions, and more
- **AI vocabulary scan** - flags words that appear at significantly higher rates in AI-generated text (vibrant, crucial, delve, tapestry, nuanced, transformative...) with a ranked priority tier
- **Structural checks** - copula substitution, participle endings, elegant variation chains, list-length uniformity, fragmented headers, punctuation personality
- **Non-destructive dry run** - `--dry-run` reports findings without editing and gives a findings-per-1,000-words density figure (with rough bands) so you can gauge how "AI" a draft sounds before committing to a revision
- **Voice calibration** - `--voice <file>` reads a sample of the author's own prior writing and suppresses house-style checks the sample shows are deliberate
- **Protected content** - quotes, citations, code, URLs, and proper nouns are flagged but never edited

## Getting Started

### Installation

#### From the mackayi marketplace (recommended)

```
/plugin marketplace add keithmackay/mackayi
/plugin install humanify@mackayi
```

#### Manual

```bash
git clone https://github.com/keithmackay/humanify.git

# Global install (available in all projects)
ln -s "$(pwd)/humanify" ~/.claude/skills/humanify

# Project-local install (available only in one project)
ln -s "$(pwd)/humanify" /path/to/your/project/.claude/skills/humanify
```

## Usage

```
/humanify                 # check and rewrite the target text
/humanify --dry-run       # report findings only, no edits
/humanify --voice <file>  # calibrate house-style checks against a writing sample
/humanify --version       # prints the installed version and checks for updates
/humanify --help          # reads and displays help.md verbatim, takes no other action
```

`help.md` is the authoritative argument reference; `SKILL.md` is the source of truth for behavior - the full list of banned phrases, AI vocabulary tiers, and structural checks. Report templates live in `references/report-format.md` and the second-tier vocabulary list in `references/tells.md`.

## About

```
Version 1.0.0 · MIT license
Source:  https://github.com/keithmackay/humanify
Author:  Keith MacKay <technofacto@gmail.com>
Update:  git -C ~/.claude/skills/humanify pull
Issues:  https://github.com/keithmackay/humanify/issues
```

## Contributing

Pull requests are welcome - fork the repo, make your change, and open a PR describing what it does and why.

## License

[MIT](LICENSE)
