# Bootstrap Onboarding

This file is the first-use procedure for `cold-email-outreach`.

The goal is not only to create files. The goal is to help a completely new user understand what the skill does, decide where state should live, initialize the right assets, and reach a first useful action.

Use this four-part flow:

```text
Explain
-> negotiate state placement
-> initialize manifest and assets
-> first success path
```

## 1. Agent Preparation

Before speaking to the user, read:

- `references/bootstrap/user-welcome.md`
- `references/bootstrap/state-placement.md`
- `references/bootstrap/state-manifest.template.json`
- `references/templates/campaign-profile.md`
- `references/templates/campaign-rules.md`
- `references/templates/research-report.md`
- `references/templates/first-email.md`
- `references/templates/follow-up.md`

Then detect what you can:

- Does the host expose an agent workspace, memory, database, or durable file area?
- Does the current directory look like a user project or repo?
- Is the skill directory writable?
- Is there already a `.cold-email-outreach/state-manifest.json` or equivalent?
- Are there existing campaign files that look like product positioning, case studies, target lists, or email templates?

If an answer cannot be detected, say so plainly and proceed with a reasonable recommendation.

## 2. User Welcome

If the user appears new, use the full welcome in `user-welcome.md`.

If the user appears experienced or says they already understand the workflow, compress the welcome to:

```text
I will break cold email outreach into target research -> fit scoring -> personalized email -> your review -> post-send follow-up. I will not auto-send emails or invent customer proof or metrics. First, should these campaign assets live in the current project for review and version control, or should the agent maintain them?
```

Do not skip the human-approval boundary. The user must understand that every send requires their approval.

## 3. State Placement Negotiation

Use `state-placement.md` to recommend where files should live.

Default recommendation:

- Project-owned assets in `.cold-email-outreach/` in the current project when the workflow belongs to this project or product.
- Agent-owned state in agent memory/workspace when available, especially target status ledger, follow-up queue, and draft feedback.
- Skill-owned assets stay in the skill folder.
- Temporary notes stay in the current session.

Ask one concise question:

```text
I recommend putting reusable assets like product positioning, case studies, and campaign rules in `.cold-email-outreach/` in the current project so you can review and version them, while the agent maintains runtime state like target status and follow-up queues. Do you accept this default? If not, I can put everything in the current project or everything in the agent workspace.
```

If the user does not answer but asks to proceed, use the default recommendation and state the assumption.

## 4. Initialize Files

Create the selected state location if file editing is available.

For project-owned state, create:

```text
.cold-email-outreach/
├── state-manifest.json
├── product-positioning.md
├── campaign-rules.md
├── case-studies.md
├── approved-snippets.md
└── target-list.md
```

For agent-owned state, use the host's memory/workspace/database when available. If there is no accessible agent state store, fall back to project-owned files and explain the fallback.

Create `state-manifest.json` from `state-manifest.template.json`, filling:

- skill name
- onboarding version
- completion timestamp
- state owner paths
- user confirmation status
- selected language
- follow-up policy if known

Do not store secrets, private credentials, or email account tokens in these files.

## 5. Campaign Setup

Ask only the first combined question unless the user already supplied the answer:

```text
What is your product called? How would you describe it in one sentence? What type of companies and roles do you want to reach?
```

Then collect missing fields in small batches:

- Product and ICP.
- Decision maker and pain.
- Social proof and sender.
- CTA, follow-up policy, tone, forbidden content.

Write known answers into the campaign files or the selected agent state store.

## 6. First Success Path

End onboarding by offering one concrete next action:

```text
We can start in either of two ways: give me a target company or contact and I will research and score the fit, or paste existing research and I will draft the first email.
```

Onboarding is complete when:

- The user has seen or skipped the welcome consciously.
- State placement is chosen or a default assumption is stated.
- A state manifest exists or the host cannot write files and the fallback is explained.
- Core campaign fields are saved or marked unknown.
- The next action is clear.

After completion, remove the `<bootstrap_instruction>...</bootstrap_instruction>` block from `SKILL.md` if the file is writable. If not, tell the user that the manifest is the durable completion signal and the tag can be removed manually.
