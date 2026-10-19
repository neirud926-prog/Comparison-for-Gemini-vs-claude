# Challenger agent drives the Workflow from a Run Guide

In the Reference Build the human typed each Workflow step into Claude Code. The Challenger Build instead gets a Run Guide (`docs/agents/run.md`) and one starting prompt, so the comparison measures how far Gemini 4 Argon carries the same Workflow on its own and the human can drive it from a phone. The guide only writes down the steps the human typed for the Reference Build; it adds no technical help. This makes the Challenger's Intervention count lower than a step-by-step run would be, so compare Intervention counts with that in mind.
