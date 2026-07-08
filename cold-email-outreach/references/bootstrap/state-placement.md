# State Placement Guide

State placement decides where personalization, campaign assets, and runtime state live.

The distributed skill should stay mostly stateless. The first run creates a local state contract that fits the user's project and agent runtime.

## State Classes

| Class | Owner | Examples | Recommended Location |
| --- | --- | --- | --- |
| Skill-owned assets | Skill package | Templates, examples, bootstrap docs, scripts | Skill folder, read-only after install |
| Project-owned assets | User/project | Product positioning, ICP, case studies, approved snippets, target list, reusable campaign rules | `.cold-email-outreach/` or a user-chosen project folder |
| Agent-owned state | Agent runtime | Target status ledger, follow-up queue, previous draft feedback, user preference memory | Agent memory/workspace/database when available |
| Ephemeral runtime state | Current session | Temporary research notes, current draft, one-off assumptions | Current conversation/session |

## Detection

Check:

- Whether the agent exposes memory, workspace, database, or durable state tools.
- Whether the current directory is a project or repo.
- Whether the user is working in a specific product folder.
- Whether the skill folder is writable.
- Whether campaign files already exist.

## Recommendation Rules

- If the campaign is tied to a product repo or team workflow, recommend project-owned assets.
- If the state is mostly scheduling, queues, preferences, or prior corrections, recommend agent-owned state.
- If the user wants portability and version control, recommend project-owned files.
- If the user wants a personal private workflow and the skill folder is writable, skill-local state is acceptable but less distributable.
- If the host has no visible agent state store, do not pretend it does. Use project-owned files unless the user says otherwise.

## User Choice

Offer three choices in plain language:

```text
Default: put project assets in the current project and let the agent maintain runtime state.
All-project: put all campaign files, target status, and follow-up queues in the current project.
All-agent: keep as much as possible in the agent workspace or memory, with only required templates in the skill.
```

Explain the tradeoff:

- Project-owned is easier to review, version, migrate, and share.
- Agent-owned is lower-friction and more personal, but less visible.
- Skill-local is simple for one machine, but weaker for distribution and upgrades.
