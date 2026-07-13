---
name: editorial-room
description: "Use when the user wants to stress-test, review, or rewrite a public post, tweet, thread, launch announcement, founder update, or product narrative through multiple critical audience perspectives. Run an independent multi-agent editorial room that separates reach, product clarity, credibility, and authorial intent; develops focused and provocative directions; blind-compares them; and returns publication-ready copy with likely reactions and tradeoffs. Do not use for a simple one-pass rewrite or private note."
---

# Editorial Room

Turn a draft or live thought into public writing without polishing away the
author's point of view. Use multiple agents to expose real tradeoffs, not to
manufacture consensus.

## Operating idea

A useful editorial room is not five agents agreeing that the safest draft is
best. Give reviewers incompatible goals, keep their first reads independent,
and let the primary agent make the editorial decision.

The default room uses five subagent turns:

1. Four blind, conflicting reviews of the source material
2. Two candidate directions written by the primary agent
3. One blind comparison by a fifth subagent
4. A final decision by the primary agent

If the host has fewer concurrency slots, run the four reviews in waves. Do not
reduce the number of perspectives merely because they cannot run at once.

## When to use the room

Use the full room when the user asks for multiple perspectives, a critical
review, publication-effect simulation, adversarial editing, or several rounds
of iteration on public-facing writing.

Do not start the full room for a simple translation, grammar fix, private note,
or one-pass rewrite unless the user explicitly asks for multi-agent review.
The room adds real latency, so use it when disagreement is part of the value.

## Step 1: recover the author's actual intent

Read the whole conversation before touching the draft. Extract:

- the platform and format
- the intended audience
- the primary outcome: reach, product understanding, credibility, conversion,
  conversation, or a personal point of view
- the live thought the author does not want edited away
- any claims that need evidence or careful attribution
- available artifacts such as images, video, links, or prior posts

Infer these from context when the answer is already present. Ask a question only
when a missing choice would materially change the result.

State the working editorial objective in one sentence before spawning reviews.
This prevents reviewers from optimizing five different posts by accident.

## Step 2: inspect the evidence

When the post refers to an attached artifact, inspect it before reviewing the
copy. Match claims to what the audience can actually see.

For factual claims that are central and plausibly disputed, verify them with an
appropriate source when tools are available. When verification is unavailable,
do not turn a theory into a fact. Preserve the idea as a clearly marked
interpretation, prediction, or question.

Never invent engagement numbers. Publication-effect simulation is qualitative
unless the user provides account analytics or comparable historical posts.

## Step 3: commission four independent reviews

Spawn four reviewers without showing them one another's output. Adapt the role
details to the post, but preserve the conflict between their objectives.

### Reviewer A: attention and distribution

Optimize for feed stopping power, immediate comprehension, pacing, and useful
conversation. Predict what gets ignored, quoted, or replied to. This reviewer
may accept more controversy than the others.

### Reviewer B: product and audience understanding

Optimize for what a new reader will remember about the product, idea, or author.
Check whether the artifact demonstrates the claim and whether another brand or
side topic steals the ending.

### Reviewer C: skeptical reader and evidence

Attack unsupported causality, ambiguity, overclaiming, terminology, and likely
derailing replies. Distinguish what is visible from what is merely implied.
This reviewer should make sharp claims safer without automatically deleting
them.

### Reviewer D: authorial-intent advocate

Defend the author's most distinctive or controversial thought. Assume that
generic safety edits can destroy the reason to publish. Find the strongest
defensible version of the original idea, even when other reviewers want it
removed.

Ask each reviewer for:

1. The strongest part
2. The biggest failure
3. Likely audience reactions, including skeptical replies
4. The editorial move they recommend
5. A concise rewrite or structural proposal

Do not ask reviewers to vote on a shared draft. Their value comes from
independent disagreement.

## Step 4: synthesize the conflict

Read all four reviews and separate convergence from goal conflict.

Do not treat majority opinion as the answer. Four cautious agents can still be
wrong for a founder who deliberately wants a provocative post. Explain what
each proposed edit optimizes and what it sacrifices.

Create two publication-ready candidates:

- **Focused direction**: maximizes clarity, credibility, and memory of the main
  subject.
- **Provocative direction**: preserves the author's sharpest defensible thesis
  and accepts a deliberate level of controversy.

The candidates should represent genuinely different editorial choices, not
minor wording variations. When a secondary thesis is valuable but distracting,
test a main-post plus first-reply structure rather than deleting it.

If bilingual output is requested, write each language for its native reading
rhythm. Do not translate line by line. Preserve the same intent and factual
strength.

Respect platform limits. Count characters for constrained formats such as a
standard X post and leave a small buffer when possible.

Keep internal editorial vocabulary out of the candidate unless the author's
audience naturally uses it. Terms such as "top of funnel", "derailment risk",
or "positioning" may help reviewers think, but they often weaken the published
voice.

## Step 5: run a blind comparison

Spawn a fifth reviewer after the primary agent has created both candidates.
Label them only Candidate A and Candidate B. Do not reveal which one is focused
or provocative, and do not provide the earlier reviews.

Ask the fifth reviewer to judge:

- fit with the stated editorial objective
- clarity to someone who has not seen the drafting conversation
- strength and distinctiveness of the author's point of view
- credibility and derailment risk
- alignment between the copy and any attached artifact

Require a winner, the reason it won, and the strongest element worth borrowing
from the other candidate. A tie is allowed only when the two choices serve
materially different user goals that remain unresolved.

## Step 6: make the editorial decision

The primary agent owns the final call. Use the blind result as evidence, not as
an instruction to outsource judgment.

Do not prefer a candidate merely because it came from the more elaborate
process. If a simpler draft or even the source text is stronger against the
stated objective, choose it and explain the minimal edit.

Before returning the copy, check:

- Did the opening lead with a reader-relevant observation rather than generic
  enthusiasm?
- Can a reader understand the point without private context?
- Does the copy say more than the artifact proves?
- Did caution erase the author's original thought?
- Is speculation labeled as speculation?
- Does another company or controversy steal attention from the intended subject?
- Does the writing sound like a person with a view, rather than a product page?
- Is the platform limit satisfied?

Read the chosen copy aloud once. Cut process language, symmetrical slogans,
generic marketing claims, and transitions that sound written by a committee.
Keep specific details, mixed feelings, and defensible rough edges that carry the
author's voice.

## Response shape

Lead with the editorial decision, then provide the ready-to-publish copy.
Summarize reasoning only to the extent that it helps the user judge the tradeoff.

Use this compact structure by default:

```markdown
Decision: [chosen direction and why]

[Final main post]

[Optional first reply or alternate language]

Effect simulation:
- Likely resonance: ...
- Likely objection: ...
- Main risk: ...

What the room disagreed about: [one concise paragraph]
```

Do not dump five raw reviews unless the user asks. The user wants a better
editorial decision, not meeting minutes.

## Failure modes

Avoid these patterns:

- Five cosmetic personas that optimize the same safe outcome
- Voting by majority instead of resolving objective conflicts
- Removing a controversial idea merely because it could attract disagreement
- Preserving a controversial idea as an unsupported factual claim
- Claiming to predict reach, likes, or conversion without data
- Returning many drafts without choosing one
- Making bilingual versions literal translations
- Letting the review process become longer than the writing deserves
- Treating more agents or more process as proof that an output is better

The room succeeds when the final post is clearer and more credible while still
containing the thought that made the author want to publish it.
