# humanify

A Claude Code skill that detects and rewrites the linguistic patterns that make AI-drafted prose read as machine-generated - banned clichés, AI-favored vocabulary, copula substitution, participle-ending sentences, elegant variation, and more. Works on any long-form text: articles, posts, emails, docs.

## Highlights

- **Banned phrase list** - clichéd pivots, weak contrast constructions, significance inflation, throat-clearing, faux-revelation transitions, and more
- **AI vocabulary scan** - flags words that appear at significantly higher rates in AI-generated text (vibrant, crucial, delve, tapestry, nuanced, transformative...) with a ranked priority tier
- **Structural checks** - copula substitution, participle endings, elegant variation chains, list-length uniformity, fragmented headers, punctuation personality
- **Non-destructive scan mode** - `--scan` reports findings without editing, so you can gauge how "AI" a draft sounds before committing to a revision

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
/humanify              # scan and rewrite the target text
/humanify --scan       # report findings only, no edits
/humanify --help       # prints the argument summary, takes no other action
```

See `SKILL.md` for the full list of banned phrases, AI vocabulary tiers, and structural checks.

## Contributing

Pull requests are welcome - fork the repo, make your change, and open a PR describing what it does and why.

## License

[MIT](LICENSE)
