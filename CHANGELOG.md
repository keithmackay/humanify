# Changelog

All notable changes to this project are documented here. The format follows [Keep a Changelog](https://keepachangelog.com/), and this project uses [Semantic Versioning](https://semver.org/).

## [Unreleased]

### Added

### Changed

### Deprecated

### Removed

### Fixed

### Security

## [1.1.0] - 2026-10-14

### Added

- Codex support: `.codex-plugin/plugin.json` and `skills/humanify/`.
- Gemini CLI support: `gemini-extension.json` and `GEMINI.md`.
- Antigravity support (the root `SKILL.md` follows the open Agent Skills format).
- Installation instructions and a compatibility matrix for all four platforms in the README.

### Changed

- The README and docs now describe humanify as a cross-agent skill, not only a Claude Code skill.

## [1.0.0] - 2026-10-14

### Added

- Banned-phrase, AI-vocabulary, and structural checks (copula substitution, participle endings, elegant variation, list-length uniformity, punctuation personality) with targeted, minimal edits.
- `--dry-run` mode that reports findings and a findings-per-1,000-words density figure without editing.
- `--voice <file>` calibration against a sample of the author's own writing.
- Protected content handling: quotes, citations, code, URLs, and proper nouns are flagged but never edited.
- `--help` and `--version` flags.
