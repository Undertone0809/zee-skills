# Meta Skills

[中文](README.zh-CN.md)

Meta skills help you create, evolve, evaluate, and package other Agent Skills.
This collection now exposes one maintained skill, `skill-creator`, for the full
skill engineering loop.

## Why Meta Skills

Agent Skills should not be static instructions that are written once and then
left alone. Useful skills reveal their real boundaries through practice: missed
triggers, unclear assumptions, brittle workflows, tool gaps, weak outputs, user
corrections, and repeated failure modes.

The core idea behind meta skills is simple: practice makes perfect. Real work
should produce evidence. Evidence should become new skills, failure classes,
workflow patches, trigger evals, validation cases, and reviewable changes. That
is how agent capability improves systematically over time.

Meta skills are not about writing longer prompts. They are a skill engineering
loop:

```text
real tasks or conversations
-> observe wins, corrections, and failure modes
-> decide whether to create a new skill or improve an existing one
-> produce a reviewable skill change
-> add evals, trigger cases, and validation cases
-> validate again in future practice
```

## When To Use Meta Skills

Use meta skills when an agent workflow should become reusable, more reliable,
easier to trigger, or easier to evaluate.

- Extract a new reusable workflow from a real conversation.
- Turn a repeated process into a discoverable Agent Skill.
- Improve an existing skill that misfires, under-triggers, over-triggers, or
  produces inconsistent outputs.
- Convert a failure, rework cycle, review blocker, or successful run into
  durable skill instructions.
- Add trigger evals, validation cases, benchmark plans, or packaging docs to a
  skill.
- Turn experience into an engineering asset that can be reviewed, tested, and
  improved.

## Common Scenarios

The most common workflow is maintaining Agent Skills from real practice:

```text
Look at the latest 30 Codex sessions. Considering the existing skills, are there
any new workflows that should become skills, or existing skills that need
optimization?
```

Use [`skill-creator`](skill-creator/SKILL.md) to separate new-skill candidates
from existing-skill patches, then produce the skill change, evals, validation
cases, benchmark plan, or package.

You can also use meta skills for narrower requests:

- "Turn this successful release workflow into a skill."
- "This skill often mis-triggers; improve the description and trigger evals."
- "Based on this failure, add validation cases to the existing skill."
- "Package this skill so it is installable and evaluable."

## Self-Iteration Best Practice

Meta skills are most valuable when they run as a recurring skill-maintenance
loop. You do not need heavy manual process. A small amount of human feedback,
captured from real sessions, is enough when it is reviewed systematically.

In Claude Code, Codex, OpenClaw, or any agent runtime that supports scheduled
work, create a daily automation task:

```text
Look at the latest 30 Codex sessions. Considering the existing skills, are there
any new workflows that should become skills, or existing skills that need
optimization with Skill Creator?
```

Use [`skill-creator`](skill-creator/SKILL.md) to turn repeated successful
workflows, corrected procedures, emerging SOPs, execution failures, missed
triggers, weak outputs, and validation gaps into new skill packages or small
reviewable patches.

Each report should include:

- the session evidence behind each recommendation
- whether the recommendation is a new-skill candidate or an existing-skill patch
- the failure class or repeated success pattern
- the suggested skill change
- the eval, trigger case, or validation case that would prove the change works

With that report, the human feedback loop stays small: review the
recommendations, approve the high-signal changes, and let the next day's
sessions test whether the skills improved.

## Skills

- [`skill-creator`](skill-creator/SKILL.md): create new skills from real
  practice, improve existing skills from evidence, run evaluation loops,
  optimize trigger descriptions, benchmark behavior, and package skill
  artifacts.

## Install

Install the meta-skills collection:

```bash
npx skills add Undertone0809/zee-agent-skills/meta-skills
```

The collection currently contains one maintained skill: `skill-creator`.

## How To Use

Start from one of two paths inside `skill-creator`:

1. Create a new skill: extract a reusable workflow from a conversation, SOP,
   team process, or successful run.
2. Improve an existing skill: diagnose, patch, validate, and benchmark an
   existing skill from real evidence.

If you are not sure whether to create or optimize, ask the agent to review
recent sessions against the existing skill set. It should separate new skill
candidates from improvements that belong in current skills.
