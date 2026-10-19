# Comparison for Gemini vs Claude

Two AI coding toolchains build the same app from the same spec, so their results can be compared side by side.

## Language

**Reference Build**:
The Tiny 2D-code Printing App as Claude Code built it (mostly Opus 5.5, some Sonnet 5.5), in `neirud926-prog/Tiny-2Dcode-Printing-App`.
_Avoid_: Claude version, Opus app, original

**Challenger Build**:
The same app built by Google's tools using Gemini 4 Argon only, in its own repo.
_Avoid_: Gemini version, rebuild, copy

**Starting Spec**:
The only project input the Challenger Build gets: the Reference Build's v1 spec (Spec #1), copied word for word.
_Avoid_: brief, prompt, requirements

**Run**:
One build from the Starting Spec to a reviewed app, following the Workflow.
_Avoid_: attempt, session

**Workflow**:
The fixed skill sequence a Run follows: to-tickets, then each ticket built test-first with tdd, then ponytail-review.
_Avoid_: process, pipeline

**Acceptance Check**:
The hard pass/fail test: the Challenger Build's Internal GTIN encoder must match every pair in the Reference Build's shared test-case file.
_Avoid_: smoke test, validation

**Skill Misfire**:
A place where a skill's Claude-specific wording confuses the Challenger Build's agent, logged as a finding instead of fixed.
_Avoid_: skill bug, incompatibility
