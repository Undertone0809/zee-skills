---
name: cold-email-outreach
description: "Use when the user wants to set up or run a B2B cold email outreach workflow: onboard a new campaign, explain the workflow to a first-time user, decide where campaign state should live, research target companies and decision makers, draft personalized first-touch emails, write follow-ups, review send readiness, or manage target status through a human-approved outbound funnel."
---

# Cold Email Outreach

<bootstrap_instruction>
First-use onboarding: read `references/bootstrap/bootstrap.md` before normal outreach work. Complete the agent-facing and user-facing onboarding, establish the state placement contract, create or update the state manifest, and help the user reach a clear first next action. After onboarding is complete and the state manifest exists, remove this entire `<bootstrap_instruction>...</bootstrap_instruction>` block from `SKILL.md` when file editing is available.
</bootstrap_instruction>

Run a human-approved B2B cold email outreach workflow. The skill helps a sales, growth, SDR, or founder-led sales operator move each target through this funnel:

```text
campaign setup
-> target added
-> target research and fit score
-> first email draft
-> human review
-> user sends or rejects
-> follow-up draft if no response
-> replied, disqualified, closed out, or handed off
```

The agent owns research, synthesis, draft quality, status recommendations, and follow-up timing. The user owns positioning, final factual approval, send decisions, and relationship judgment.

## Before Work Starts

If the user is new, confused, or has not completed onboarding, run `references/bootstrap/bootstrap.md`.

Use the state manifest as the source of truth for onboarding completion. The bootstrap tag is only a first-run UX hook; it is not the durable completion signal.

Look for a manifest in this order:

1. Project-owned state path from the user's current workspace, commonly `.cold-email-outreach/state-manifest.json`.
2. Agent-owned memory or workspace if the host exposes one.
3. Skill-local state only for personal local installs where the skill directory is writable.

If no manifest exists, explain the workflow to the user and negotiate state placement before creating files.

## State Ownership

Keep these ownership boundaries clear:

- **Skill-owned assets:** `SKILL.md`, bootstrap instructions, templates, examples, and reference docs bundled with this skill.
- **Project-owned assets:** product positioning, ICP, case studies, approved snippets, campaign rules, target lists, and reusable team-facing files.
- **Agent-owned state:** target status ledger, follow-up queue, prior draft feedback, user preferences, and run notes when the agent has a memory or workspace.
- **Ephemeral runtime state:** current task context, temporary research notes, and drafts from the current session.

When in doubt, ask whether the user wants campaign assets to be visible in the current project for review/version control or kept in the agent's private workspace.

## Operating Principles

- Do not send email or claim something was sent unless the user explicitly confirms it.
- Do not invent facts, customer proof, metrics, relationships, public signals, or compliance claims.
- Every email needs one concrete personalization signal from research or user-provided context.
- Prefer precise, low-friction asks over broad marketing language.
- Keep missing evidence visible; mark assumptions instead of hiding them.
- If research quality is weak, say so and draft a safer lower-confidence angle.
- Every initial email and follow-up goes through human review before send.

## Campaign Fields

Collect these fields during onboarding or when creating a new campaign. Ask in small batches, starting with the combined first question.

First question:

```text
What is your product called? How would you describe it in one sentence? What type of companies and roles do you want to reach?
```

Fields:

| Field | Purpose |
| --- | --- |
| Product | Product name and one-sentence description. |
| ICP | Industry, size, stage, geography, maturity, buying triggers. |
| Decision maker | Roles and seniority to contact. |
| Value proposition | Core pain and why the product matters now. |
| Social proof | Quantified case studies, logos, credibility signals, or "none yet". |
| Sender | Name, role, company, signature, and channel identity. |
| CTA | Reply, 15-minute call, demo, resource, referral, or other action. |
| Follow-up policy | Max follow-ups and interval in business days. |
| Tone | Formal, peer-to-peer, concise, technical, founder-led, data-driven, etc. |
| Forbidden content | Competitors, unreleased features, compliance claims, discounts, or other red lines. |

## Status Workflows

### Pending Research

Act as the target researcher. Read campaign state, target input, and any provided URLs or files. Research only with available sources or user-provided material. Produce a structured report using `references/templates/research-report.md`.

Completion criteria:

- Company overview is filled in.
- Recent signals include source context or are marked unknown.
- Decision maker profile is filled in or gaps are explicit.
- Pain hypotheses connect to the user's value proposition.
- Fit score has a clear proceed, research more, or disqualify recommendation.

Routing:

- Fit score `>= 4/10`: recommend moving to first email drafting.
- Fit score `< 4/10`: recommend disqualification and record the reason.

### Draft First Email

Act as the cold email copywriter. Use the research report and campaign rules to draft one personalized first-touch email using `references/templates/first-email.md`.

Completion criteria:

- Subject line is specific and not clickbait.
- Body is 80-150 Chinese characters or 80-150 English words unless the user asks otherwise.
- Opening references a concrete signal.
- Value proposition ties to the target's likely pain.
- CTA matches the user's preferred action.
- Angle notes, channel, word count, send timing, and review risks are included.

Routing:

- Move to human review.
- If personalization is weak, say what research is missing before drafting.

### Draft Follow-Up

Act as the follow-up copywriter. Use prior email history, elapsed time, target status, and campaign rules to draft the next follow-up using `references/templates/follow-up.md`.

Completion criteria:

- Follow-up angle is meaningfully different from prior emails.
- It references a new signal, useful resource, social proof, mutual context, technical observation, or respectful close-out.
- It includes a send timing recommendation.
- It respects the maximum follow-up count.

Routing:

- Move to human review.
- If the target has reached the follow-up limit with no response, recommend closed-out or disqualified status.

## Quality Bar

A good cold email feels researched, specific, and easy to answer.

Review every draft with this checklist:

- Is the personalization real and specific?
- Does the target understand why they were contacted?
- Is the pain connected to the user's value proposition?
- Are all numbers and proof grounded in user-provided or cited evidence?
- Is the CTA low-friction?
- Is the wording free of generic hype like "revolutionary", "game-changing", or "empower your business"?
- Are risks and missing facts visible to the user before approval?

## Reference Files

- First-use onboarding: `references/bootstrap/bootstrap.md`
- User welcome copy: `references/bootstrap/user-welcome.md`
- State placement guide: `references/bootstrap/state-placement.md`
- State manifest template: `references/bootstrap/state-manifest.template.json`
- Templates: `references/templates/`
- Examples: `references/examples/`

## Deliverable Format

Default to Markdown. Put the main artifact first, then concise operational notes:

1. Research report, first email draft, follow-up draft, or onboarding proposal.
2. Recommendation: proceed, revise, research more, send-ready, disqualify, or closed-out.
3. Missing facts or approval risks.
4. Suggested next status.
