# Feature map: Claude Code vs Google Antigravity (checked 2026-10-19)

Antigravity = the Antigravity CLI (`agy`) and the Antigravity App 2.0 (desktop). Both run Gemini 4 Argon; the Antigravity IDE does not.

| Claude Code | Antigravity | Match |
|---|---|---|
| Terminal agent (`claude`) | `agy` | ✅ |
| Desktop app | Antigravity App 2.0 | ✅ |
| IDE extensions | VS Code, JetBrains, Zed, Xcode, Visual Studio extensions; Antigravity IDE has no Argon | ⚠️ |
| `CLAUDE.md` | `AGENTS.md` / `GEMINI.md`, `.agents/rules/*.md` | ✅ |
| Skills (`SKILL.md`, `/name`) | Same format in `.agents/skills/`, `/name` in the CLI. Only `name` and `description` frontmatter documented | ✅ |
| Subagents | `invoke_subagent`, custom agents in `.agents/agents/`, run in the background | ✅ |
| Hooks | `.agents/hooks.json`: SessionStart, PreToolUse, PostToolUse, PreInvocation, PostInvocation, Stop. PreToolUse can deny | ✅ (fewer events) |
| MCP | MCP | ✅ |
| Plugins, marketplace | Plugins, marketplace | ✅ |
| Plan mode | `/plan` | ✅ |
| Permission modes | `/permissions`, `toolPermission` setting, terminal sandbox (macOS, Linux) | ✅ |
| Resume, rewind | `/resume`, `/rewind`, `/fork` | ✅ |
| Headless `-p`, JSON output | `-p`, `--output-format json / stream-json` | ✅ |
| Agent SDK | Antigravity SDK | ✅ |
| Status line | `/statusline` | ✅ |
| Worktrees | App projects use Git worktrees | ✅ |
| Browser use | `/browser` subagent | ✅ |
| Scheduled tasks that run in the cloud with the laptop off | `/schedule`: cron or timer, but scoped to the conversation that made it, on your machine. Sidecars: local background jobs | ⚠️ |
| Cloud sessions (run with the computer off) | None with Argon. Jules runs in the cloud on Gemini 3.1 Pro; Gemini API Managed Agents run in the cloud on Flash models only | ❌ |
| Phone app driving cloud sessions | Remote Control: a web dashboard (installable on the phone, with push notifications) driving the App or `agy` on your PC. The PC must stay on | ⚠️ |
| GitHub PR watching, CI autofix, review replies, `@claude` GitHub Action | No first-party equivalent. Local `gh` and the App's Git panel; headless `agy` inside your own Actions workflow; community actions. Jules has a CI fixer, without Argon | ❌ |

## Where Gemini 4 Argon runs

- ✅ Antigravity CLI and Antigravity App: Ultra, and paid (not trial) Pro per the models page
- ✅ Gemini app and AI Mode in Search: Pro and Ultra (chat only, no repo)
- ✅ Gemini API in Google AI Studio (`gemini-4-argon`), and the Gemini Enterprise Agent Platform on Google Cloud
- ❌ Antigravity IDE, Jules, Gemini API Managed Agents

Sources: [hooks and docs index](https://antigravity.google/docs/hooks), [remote control](https://antigravity.google/docs/remote-control/), [/schedule](https://antigravity.google/docs/commands/schedule/), [App features](https://antigravity.google/docs/features/), [sidecars](https://antigravity.google/docs/sidecars/), [headless](https://antigravity.google/docs/cli/headless/), [models](https://antigravity.google/docs/models), [skills](https://antigravity.google/docs/skills), [subagents](https://antigravity.google/docs/subagents), [Managed Agents](https://ai.google.dev/gemini-api/docs/custom-agents), [Jules changelog](https://jules.google/docs/changelog/), [Argon availability](https://blog.google/innovation-and-ai/models-and-research/gemini-models/introducing-gemini-4-argon/).
