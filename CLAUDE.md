# Comparison for Gemini vs Claude

Compares Claude and Gemini coding tools on the same project: the Tiny 2D-code Printing App (Claude Code + Opus, done in `neirud926-prog/Tiny-2Dcode-Printing-App`) rebuilt with Google's Gemini tools + Gemini 4 Argon.

## Agent skills

Same skill set as the Tiny 2D-code repo, in `.claude/skills/` (Matt Pocock's skills v1.3.1 and ponytail).

### Issue tracker

Issues are tracked in GitHub Issues (neirud926-prog/Comparison-for-Gemini-vs-claude) via the `gh` CLI. See `docs/agents/issue-tracker.md`.

### Triage labels

Default five-role vocabulary: needs-triage, needs-info, ready-for-agent, ready-for-human, wontfix. See `docs/agents/triage-labels.md`.

### Domain docs

Single-context: root `GLOSSARY.md` + `docs/adr/`, both created when first needed. See `docs/agents/domain.md`.

## Layout

- `challenger-kit/`: the Challenger repo's starting files (Reference Build at `bb238d9`, `AGENTS.md`, Spec #1 with links moved to the Challenger repo)
- `docs/challenger-setup.md`: how to run the Challenger Build in Antigravity CLI
- `runs/`: one run log per build
