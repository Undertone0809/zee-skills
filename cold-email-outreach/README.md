# Cold Email Outreach

`cold-email-outreach` runs a human-approved B2B cold email workflow: first-time onboarding, campaign state placement, target research, personalized first-touch drafts, follow-ups, and status recommendations.

The skill is intentionally distributed as a mostly stateless package. On first use, it helps the user decide where campaign state should live: in the current project, in the agent's own workspace or memory, or in a skill-local personal folder. It then creates a state manifest so future runs can continue without repeating onboarding.

## Install

```bash
npx skills add Undertone0809/zee-agent-skills/cold-email-outreach
```

## What It Does

- Explains the workflow to a completely new user.
- Negotiates state ownership for project assets, agent state, skill assets, and temporary runtime state.
- Collects campaign positioning: product, ICP, decision maker, value proposition, proof, sender, CTA, tone, follow-up policy, and forbidden content.
- Researches target companies and decision makers.
- Scores fit and recommends whether to proceed.
- Drafts personalized first-touch emails and follow-ups.
- Keeps the user in the approval loop before every send.

## First Use

First use follows four steps:

```text
Explain
-> negotiate state placement
-> initialize manifest and campaign files
-> guide the user to a first useful action
```

The bootstrap workflow lives in `references/bootstrap/bootstrap.md`. User-facing onboarding copy lives in `references/bootstrap/user-welcome.md`.

## Typical Prompts

```text
Use cold-email-outreach to set up a campaign for our data infrastructure product.
```

```text
Research this target and decide if we should email them: DataStream, VP Engineering Alex Chen.
```

```text
Draft the first email for this research report. Keep it peer-to-peer and data-driven.
```

```text
No reply after 4 business days. Draft follow-up #1 with a new value angle.
```

## Included Assets

- `references/bootstrap/bootstrap.md`: agent-facing first-use procedure.
- `references/bootstrap/user-welcome.md`: new-user introduction copy.
- `references/bootstrap/state-placement.md`: state ownership decision guide.
- `references/bootstrap/state-manifest.template.json`: manifest template.
- `references/templates/`: research, first email, follow-up, and campaign asset templates.
- `references/examples/`: quality calibration examples.
