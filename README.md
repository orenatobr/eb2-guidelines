# eb2-guidelines

Reads every file of an EB-2 NIW (I-140 National Interest Waiver) dossier the way the adjudicating officer would, and keeps an RFE risk report up to date. It applies a checklist (Dhanasar prongs, credibility, recommendation letters, credentials, form, sworn forms, weak evidence, current policy), verifies critical findings at the source and records each finding with a stable ID and a severity. The output is a technical review and does not replace legal advice.

## Install

For Claude Code, copy the skill folders into your personal skills directory:

```bash
git clone https://github.com/orenatobr/eb2-guidelines.git
mkdir -p ~/.claude/skills
cp -R eb2-guidelines/skills/* ~/.claude/skills/
```

Start a new Claude Code session. Skills load from the `description` in each `SKILL.md`, so they trigger on their own and can also be called by name.

Other agents: each skill is a folder with a `SKILL.md` (YAML frontmatter plus Markdown). Tools that read the Agent Skills format can load `skills/` as it is. For the rest, paste the body of the `SKILL.md` into the agent's instruction file.

## Usage

Run `/eb2-guidelines` from the folder that holds `eb2-niw/`. It also triggers on "RFE", "EB2" and "EB-2 NIW".

## Requirements

A local dossier folder named `eb2-niw/`. The last step edits an HTML report source and runs a build script, both of which you supply. Optional: markitdown, tesseract and Google Scholar MCP servers.

## What is in this repo

| Path | Purpose |
|------|---------|
| `skills/eb2-guidelines/` | The `eb2-guidelines` skill. |
| `CHANGELOG.md` | Notable changes, in Keep a Changelog format. |
| `AGENTS.md` | Rules for AI agents that edit this repo. |

## Contributing

`main` is protected: changes land through a pull request with one approval. See [AGENTS.md](AGENTS.md) for the conventions.

## License

MIT, see [LICENSE](LICENSE).
