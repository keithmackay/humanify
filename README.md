# humanify

An agent skill for Claude Code, Codex, Antigravity, and Gemini CLI that detects and rewrites the linguistic patterns that make AI-drafted prose read as machine-generated - banned clichés, AI-favored vocabulary, copula substitution, participle-ending sentences, elegant variation, and more. Works on any long-form text: articles, posts, emails, docs.

Works with: Claude Code · Codex · Antigravity · Gemini CLI

## Highlights

- **Banned phrase list** - clichéd pivots, weak contrast constructions, significance inflation, throat-clearing, faux-revelation transitions, and more
- **AI vocabulary scan** - flags words that appear at significantly higher rates in AI-generated text (vibrant, crucial, delve, tapestry, nuanced, transformative...) with a ranked priority tier
- **Structural checks** - copula substitution, participle endings, elegant variation chains, list-length uniformity, fragmented headers, punctuation personality
- **Non-destructive dry run** - `--dry-run` reports findings without editing and gives a findings-per-1,000-words density figure (with rough bands) so you can gauge how "AI" a draft sounds before committing to a revision
- **Voice calibration** - `--voice <file>` reads a sample of the author's own prior writing and suppresses house-style checks the sample shows are deliberate
- **Protected content** - quotes, citations, code, URLs, and proper nouns are flagged but never edited

## Getting Started

### Installation

humanify is a plain-markdown skill (the open [Agent Skills](https://agentskills.io/home) `SKILL.md` format) with no dependencies or executable code, so it installs the same way on every supported agent.

#### Claude Code

From the mackayi marketplace (recommended), run inside a Claude Code session:

```
/plugin marketplace add keithmackay/mackayi
/plugin install humanify@mackayi
```

Or manually:

```bash
git clone https://github.com/keithmackay/humanify.git

# Global install (available in all projects)
ln -s "$(pwd)/humanify" ~/.claude/skills/humanify

# Project-local install (available only in one project)
ln -s "$(pwd)/humanify" /path/to/your/project/.claude/skills/humanify
```

Invoke with `/humanify`.

#### Codex

Clone the repo, then register it in your Codex plugin marketplace.

**`~/.agents/plugins/marketplace.json`** (create if absent):

```json
{
  "name": "personal",
  "interface": { "displayName": "Personal Plugins" },
  "plugins": [
    {
      "name": "humanify",
      "source": { "source": "local", "path": "/path/to/humanify/" },
      "policy": { "installation": "AVAILABLE", "authentication": "ON_INSTALL" },
      "category": "Productivity"
    }
  ]
}
```

The plugin manifest is `.codex-plugin/plugin.json`; the skill itself is at `skills/humanify/`.

#### Antigravity

```bash
git clone https://github.com/keithmackay/humanify.git

# Global install (all workspaces)
cp -r humanify ~/.gemini/antigravity/skills/humanify

# Workspace install (current project only)
cp -r humanify /path/to/your/project/.agents/skills/humanify
```

Skills are auto-discovered. You can also mention humanify by name to force activation.

#### Gemini CLI

```bash
gemini extensions install https://github.com/keithmackay/humanify
```

To update: `gemini extensions update humanify`. The skill loads through `GEMINI.md` after installation.

## Usage

The examples use the `/humanify` form. On Gemini CLI and Antigravity, ask the agent to run humanify (or mention it by name) and pass the same flags.

```
/humanify                 # check and rewrite the target text
/humanify --dry-run       # report findings only, no edits
/humanify --voice <file>  # calibrate house-style checks against a writing sample
/humanify --version       # prints the installed version and checks for updates
/humanify --help          # reads and displays help.md verbatim, takes no other action
```

`help.md` is the authoritative argument reference; `SKILL.md` is the source of truth for behavior - the full list of banned phrases, AI vocabulary tiers, and structural checks. Report templates live in `references/report-format.md` and the second-tier vocabulary list in `references/tells.md`.

## Compatibility

| Feature | Claude Code | Codex | Antigravity | Gemini CLI |
|---------|:-----------:|:-----:|:-----------:|:----------:|
| Core skill (checks, edits, reports) | ✅ | ✅ | ✅ | ✅ |
| `--dry-run`, `--voice`, `--version`, `--help` | ✅ | ✅ | ✅ | ✅ |
| `references/` sub-documents | ✅ | ✅ | ✅ | ✅ |
| Marketplace install | ✅ | ✅ | ❌ | ❌ |
| Install from GitHub URL | ❌ | ❌ | ❌ | ✅ |

Legend: ✅ Supported · ❌ Not supported

humanify uses no platform-specific frontmatter, hooks, or subagents, so the same skill content runs unchanged on all four agents. The Antigravity copy is the root `SKILL.md`.

## References

- **Claude Code Skills:** https://code.claude.com/docs/en/skills
- **Codex Plugins:** https://developers.openai.com/codex/plugins/build
- **Antigravity Skills:** https://antigravity.google/docs/skills
- **Gemini CLI Extensions:** https://github.com/google-gemini/gemini-cli/blob/main/docs/extension.md
- **Agent Skills open standard:** https://agentskills.io/home

## About

```
Version 1.1.0 · MIT license
Source:  https://github.com/keithmackay/humanify
Author:  Keith MacKay <technofacto@gmail.com>
Update:  git -C ~/.claude/skills/humanify pull   (or: /plugin marketplace update mackayi; gemini extensions update humanify)
Issues:  https://github.com/keithmackay/humanify/issues
```

## Contributing

Pull requests are welcome - fork the repo, make your change, and open a PR describing what it does and why.

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for release history.

## License

[MIT](LICENSE)
