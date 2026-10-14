# agent-review-skills

Two small skills for coding agents, written as plain `SKILL.md` files. They work in Codex CLI and Claude Code.

| Skill | What it does |
| --- | --- |
| `code-review` | Reviews your current diff (or a PR, branch or path) for real bugs. It traces each finding through the code, marks it CONFIRMED or PLAUSIBLE, and ranks by severity. Review only: it edits nothing unless you ask for a fix. |
| `simplify` | Cleans up the code you just changed: reuse, simplification, efficiency. It does not change behaviour and does not hunt for bugs. It runs your lint and tests afterwards. |

## Install

Codex CLI:

```bash
mkdir -p ~/.codex/skills
cp -r skills/code-review skills/simplify ~/.codex/skills/
```

Claude Code:

```bash
mkdir -p ~/.claude/skills
cp -r skills/code-review skills/simplify ~/.claude/skills/
```

Start a new session afterwards so the agent picks them up.

## Use

Ask in plain words, or name the skill:

- `Use the code-review skill at high effort on my uncommitted changes. Do not edit anything.`
- `Use the simplify skill on my uncommitted changes. Keep the behaviour the same.`

Tell the agent what is already settled (for example "do not re-report the focus fix") so it does not repeat it.

A good routine is review first, then fix what it found, then simplify, then run your tests.

## Licence

MIT, see `LICENSE`.
