# Tiny 2D-code Printing App

Portable Electron app that generates GS1 DataMatrix codes (AIs 01 / 17 / 10) for the BCS replenishment project.

## Agent skills

### Issue tracker

Issues are tracked in GitHub Issues (neirud926-prog/Tiny-2Dcode-Printing-App-Gemini) via the `gh` CLI. See `docs/agents/issue-tracker.md`.

### Triage labels

Default five-role vocabulary: needs-triage, needs-info, ready-for-agent, ready-for-human, wontfix. See `docs/agents/triage-labels.md`.

### Domain docs

Single-context: root `GLOSSARY.md` + `docs/adr/`, both created when first needed. See `docs/agents/domain.md`.

### Skills

Project skills are in `.agents/skills/` (Matt Pocock's skills v1.3.1 and ponytail). Some say "call the Skill tool" or "dispatch a sub-agent": load that skill, or use `invoke_subagent`.

## Rules

- Never open, clone, search or fetch anything from `neirud926-prog/Tiny-2Dcode-Printing-App`. This repo is built from its spec alone.
