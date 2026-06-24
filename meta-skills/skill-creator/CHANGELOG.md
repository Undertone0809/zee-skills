# Changelog

Notable changes to the `skill-creator` skill.

## 2026-06-25

- Added this changelog so future skill-maintenance work has a durable change history.

## 2026-06-24

- Split host compatibility guidance from a single `references/compatibility.md` file into host-specific guides under `references/compatibility/`.
- Added `references/compatibility/claudecode.md` for Claude Code behavior, real trigger measurement, and baseline mechanics.
- Added `references/compatibility/codex.md` for Codex/OpenAI behavior, judged trigger proxy expectations, and `agents/openai.yaml` metadata guidance.
- Added `references/compatibility/other.md` for generic shell agents, chat-only hosts, and headless worker adaptations.
- Updated `SKILL.md` to direct agents to read exactly one host-specific compatibility guide unless evaluating or packaging across multiple hosts.
