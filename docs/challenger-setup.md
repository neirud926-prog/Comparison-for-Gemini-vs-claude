# Challenger Build setup: Gemini 4 Argon with the same skills and workflow

How to run the Challenger Build in Google's tools the same way the Reference Build ran in Claude Code. Decisions behind it: `docs/adr/`. Words: `GLOSSARY.md`.

## What maps to what

| Claude Code (Reference Build) | Antigravity CLI (Challenger Build) |
|---|---|
| `claude` in a cloud session | `agy` on your PC |
| `CLAUDE.md` | `AGENTS.md` |
| `.claude/skills/<name>/SKILL.md` | `.agents/skills/<name>/SKILL.md` (same file format) |
| `/skill-name` | `/skill-name` (the CLI turns each skill into a slash command) |
| Sub-agents (Agent tool) | `invoke_subagent` (runs in the background) |
| Model picker | `/model` |
| Usage | `/usage` (alias `/quota`) |
| `gh` CLI | `gh` CLI (same) |

## 1. Plan

Google AI Ultra, US$100/month, bought on an account whose country is the US. Antigravity's models page also lists Gemini 4 Argon for paid (not trial) AI Pro, at lower limits. Gemini 4 Argon is **not** in the Antigravity IDE, only in the CLI and the desktop app.

## 2. Tools on the PC

- git, Node.js 22 (the spec's toolchain), and the GitHub CLI. Then sign in with `gh auth login`.
- Antigravity CLI:
  - Windows PowerShell: `irm https://antigravity.google/cli/install.ps1 | iex`
  - macOS or Linux: `curl -fsSL https://antigravity.google/cli/install.sh | bash`
  - Check: `agy --version`

## 3. The Challenger repo

1. On GitHub, create an empty public repo `neirud926-prog/Tiny-2Dcode-Printing-App-Gemini` (no README, no licence).
2. Fill it from this repo's kit and skills (on Windows, run these in Git Bash):

```sh
git clone -b claude/gemini-claude-comparison-da7vcb https://github.com/neirud926-prog/Comparison-for-Gemini-vs-claude
git clone https://github.com/neirud926-prog/Tiny-2Dcode-Printing-App-Gemini
cd Tiny-2Dcode-Printing-App-Gemini
cp -r ../Comparison-for-Gemini-vs-claude/challenger-kit/. .
mkdir .agents && cp -r ../Comparison-for-Gemini-vs-claude/.claude/skills .agents/skills
mv spec-1.md ../spec-1.md
git add -A && git commit -m "docs: agent skills setup, glossary and Internal GTIN ADRs" && git push
```

3. Create the five triage labels, then post the Starting Spec as issue #1:

```sh
for l in needs-triage needs-info ready-for-agent ready-for-human wontfix; do gh label create "$l" --force; done
gh issue create --title "Spec: Tiny 2D-code Printing App v1 (Internal GTIN GS1 DataMatrix labels)" --body-file ../spec-1.md --label ready-for-agent
```

The repo now matches the Reference Build's state just before any code: commit `bb238d9` plus Spec #1. The two differences are `AGENTS.md` in place of `CLAUDE.md` and the skills living in the repo.

## 4. First launch

1. In the repo folder run `agy`. Trust the workspace when asked, and sign in with the Ultra Google account.
2. `/model` → **Gemini 4 Argon**, thinking level **High**. The choice is kept across sessions, but check it at the start of every session: Antigravity also offers Claude Opus 5.5 and Sonnet 5.5, and using them would void the Run.
3. `/skills`: expect 33 project skills. If some are missing, note the names as Skill Misfires.
4. `/usage`: write the starting quota into `runs/challenger.md`.
5. Phone: in a second terminal on the PC run `agy remote-control start` (a background service; `agy remote-control status` shows the machine's name, `agy remote-control stop` ends it). On the phone, open https://antigravity.google.com, sign in with the same Google account, pick the machine, and add the page to the home screen for push notifications. Keep the PC awake for the whole Run.

## 5. The Run (same Workflow as the Reference Build)

The agent drives the Workflow itself from the Run Guide, `docs/agents/run.md` in the Challenger repo: `/to-tickets` on issue #1, `/implement` each ticket (`tdd`, then `code-review`), push, then one `/ponytail-review` round and its fixes. Start it with one prompt, from the PC or the phone:

```
Build this app: follow docs/agents/run.md, starting at step 1.
```

Answer the agent's questions normally; anything beyond that is an Intervention, so log it. If a session ends mid-Run, start the next one with `Continue the Run in docs/agents/run.md from where the issues show it stopped.`

After each ticket:

- Fill a row in `runs/challenger.md`: times, Interventions (word for word), Skill Misfires, `/usage`, any top-up.
- Scan the ticket's conversation for `Tiny-2Dcode-Printing-App` without `-Gemini`. Any hit is a breach: log it and redo the ticket.
- If a background subagent ran on a model other than Gemini 4 Argon (check `/agents`), log it under Model check.

## 6. The end

Tell Claude in the comparison repo that the Run is done. It runs the Acceptance Check (the Challenger encoder against `test-vectors/internal-gtin-v1.csv` from the Reference Build) and a ponytail-review on both builds, and writes the results.

## Known gaps

- Antigravity documents only `name` and `description` in skill frontmatter. `disable-model-invocation` is probably ignored, so skills like `implement` or `grill-with-docs` may start on their own. Log it as a Skill Misfire if it happens.
- Skills that say "call the Skill tool" are Claude wording. `AGENTS.md` tells the agent how to read them in Antigravity.

Sources: [Antigravity skills](https://antigravity.google/docs/skills), [rules](https://antigravity.google/docs/rules), [models](https://antigravity.google/docs/models), [subagents](https://antigravity.google/docs/subagents), [getting started](https://antigravity.google/docs/getting-started/), [CLI reference](https://antigravity.google/docs/cli/reference/).
