---
name: marketing-council
description: "Evaluate a marketing plan, campaign brief, GTM strategy, launch plan, positioning doc, or funnel through a council of 6 specialist advisors arranged in three opposing pairs (Brand Strategist vs Performance Marketer, Customer vs Rival, Creative Director vs Operator). Each scores the plan independently, then they peer-review each other anonymously, then a CMO chairman issues a Go / Go-with-changes / Rework / Kill verdict with a health scorecard. MANDATORY TRIGGERS: 'marketing council', 'run the marketing council', 'CMO review', 'review my marketing plan', 'evaluate this campaign', 'pressure-test this campaign', 'red team this launch', 'score this plan'. STRONG TRIGGERS (use when the user supplies an actual plan, brief, or strategy document): 'what do you think of this marketing plan', 'is this campaign any good', 'will this launch work', 'critique my GTM', 'evaluate my positioning', 'should we run this campaign', 'poke holes in this'. Do NOT trigger for writing marketing copy, drafting a single ad, answering generic marketing questions, or channel how-tos. This skill EVALUATES an existing plan; it does not write one."
---

# Marketing Council

A marketing plan reviewed by one AI gets one opinion, and that opinion bends to how the plan was framed. Present a campaign enthusiastically and you get validation. Present the same campaign anxiously and you get concerns. Same plan, opposite readings.

The Marketing Council fixes this by running the plan past six advisors who are structurally incapable of agreeing with each other, making them review each other's work blind, and then having a chairman force the disagreement into a single verdict.

This is the LLM Council methodology (Karpathy) retargeted for marketing evaluation. The generic council uses five thinking styles for open decisions. This one uses six marketing lenses, adds a structured intake gate, forces numeric scoring, and ends in a four-way verdict instead of a recommendation.

---

## The design principle

The roster is built on **tensions, not job titles.**

The failure mode when building a marketing council is staffing it by channel — a Social person, an SEO person, an Email person, a Content person. That produces six advisors who each say "yes, and here's how my channel contributes." No tension, no signal.

These six are arranged in three opposing pairs. Each pair argues about something marketers genuinely and legitimately disagree about. If two advisors start producing similar output, that is a roster failure to correct, not a consensus to celebrate.

---

## The roster

### Pair 1 — Time horizon: equity vs. revenue

**1. The Brand Strategist** — owns long-term asset building. Judges whether the plan makes the brand more mentally available to future buyers or merely rents attention this quarter. Hunts for category-generic positioning ("trusted," "expert," "your partner in X").

**2. The Performance Marketer** — owns the arithmetic. CAC, payback period, channel saturation, funnel conversion math. Kills beautiful ideas that do not pencil out.

*The tension:* brand building vs. activation. The most important argument in marketing, and most plans resolve it by accident rather than on purpose.

### Pair 2 — Whose judgment counts: the buyer vs. the rival

**3. The Customer** — zero marketing literacy. Reads the plan as the actual target person would encounter the actual output. Does not know the category jargon, was not thinking about the brand at all. Catches the curse of knowledge.

**4. The Rival** — plays the strongest competitor reading this plan the morning it launches, looking to neutralize it cheaply. Tests defensibility and response-fragility.

*The tension:* the Customer wants resonance and clarity; the Rival wants defensibility. The most resonant message is often the most copyable one.

### Pair 3 — Ambition vs. reality

**5. The Creative Director** — owns interestingness. Treats "boring" as the primary risk, because being ignored is the default outcome in marketing.

**6. The Operator** — owns feasibility. Team capacity, budget reality, timeline, dependencies, who actually does the work on Monday.

*The tension:* the ambitious swing vs. what actually ships.

Full prompt templates for all six: `references/advisors.md`

---

## When to run the council

**Good council inputs** — an actual artifact with stakes:
- A campaign brief or launch plan
- A GTM strategy for a new product or market
- A positioning or messaging document
- A funnel or landing page strategy
- A quarterly or annual marketing plan
- A budget allocation across channels

**Skip the council for:**
- Writing copy, ads, posts, or emails (creation, not evaluation)
- Generic marketing questions with known answers ("what is a good email open rate")
- Channel how-tos ("how do I set up a Meta campaign")
- A plan so thin there is nothing to evaluate — run the intake gate and return the gaps instead

---

## How a council session works

### Step 1: intake gate

**Do this before anything else. It is the highest-value step in the skill.**

Unlike an open question, a marketing plan has known required components. Extract them from what the user supplied, and record which are missing. Missing components are findings in themselves — the two most commonly absent are a real success metric and a genuinely specific audience, and a plan missing either is unevaluable rather than merely weak.

Work through `references/intake-checklist.md`. Produce an intake table listing each component as PRESENT, VAGUE, or MISSING.

Then branch:

- **4 or more components MISSING** — stop. Do not convene. Return the gap report and ask the user to fill the gaps first. Convening six advisors on a plan this thin produces six advisors inventing the same missing information differently.
- **1 to 3 MISSING or VAGUE** — proceed, but pass the gaps to every advisor as explicit unknowns. Instruct advisors to treat them as risks, not to invent assumptions and quietly evaluate their own invention.
- **All PRESENT** — proceed normally.

### Step 2: context scan and framing

Scan the workspace for material that makes advice specific rather than generic. Spend no more than 30 seconds; you are looking for the two or three files that matter:

- `CLAUDE.md` or `claude.md` in the project root or workspace
- Any `memory/` folder (audience profiles, voice docs, past decisions)
- `brand/`, brand guidelines, tone-of-voice docs
- Past campaign results, revenue data, analytics exports
- Prior council transcripts in this folder, to avoid re-counciling settled ground

Use `Glob` and quick `Read` calls.

Then assemble the **council brief** that all six advisors receive identically:

1. The plan itself, reproduced in full
2. The intake table, including the gaps
3. Business context from workspace files (stage, audience, constraints, past results, relevant numbers)
4. What is at stake — budget committed, timeline, what happens if this fails

Do not editorialize. Do not signal your own read of the plan. The brief is neutral input.

### Step 3: convene the six advisors in parallel

Spawn all six sub-agents simultaneously. Sequential spawning wastes time and risks contamination between responses.

Each advisor receives their identity from `references/advisors.md`, the council brief, and the standing instruction to lean fully into their lens without hedging or attempting balance.

**This instruction matters more here than in the generic council.** Marketing roles share vocabulary and will drift toward polite consensus if permitted. Each advisor must be told explicitly that the other five cover the angles they are not covering, so partial views are correct behavior, not a flaw.

Every advisor returns the same standardized block:

```
SCORE (1-10) on my dimension, with one sentence of justification
STRONGEST ELEMENT of the plan, from my angle
FATAL FLAW (or "none found" — but look hard before concluding that)
KILL CRITERIA: the one thing that, if true, means do not run this
THE FIX: one specific change that most improves the plan on my dimension
```

Followed by 150-250 words of reasoning. No preamble.

### Step 4: anonymized peer review

Collect all six responses. Relabel them **Response A through F, randomizing the mapping** so there is no positional or identity bias. Anonymity is load-bearing: with names attached, reviewers defer to whichever advisor sounds most senior rather than evaluating the argument.

Spawn six fresh sub-agents. Each sees all six anonymized responses and answers:

1. Which response is strongest, and why?
2. Which has the biggest blind spot, and what is it missing?
3. What did ALL six miss that the council should consider?

Prompt template in `references/peer-review.md`.

### Step 5: the budget reallocation exercise

Before synthesis, force one concrete tradeoff. Ask all six advisors the same single question:

> If you had to move 20% of this budget, where would it come from and where would it go?

Nothing exposes a plan's real weak point faster than making someone defund part of it. Abstract critique becomes a decision. Collect all six answers; convergence here is a very strong signal.

Bundle this into the peer review spawn to save a round trip — the reviewer prompt in `references/peer-review.md` already includes it as question 4.

### Step 6: chairman synthesis — the CMO seat

One final agent receives everything: the plan, the intake table, all six advisor responses now de-anonymized, all six peer reviews, and all six reallocation answers.

It produces the verdict in the fixed structure below. Chairman prompt in `references/peer-review.md`.

### Step 7: report and transcript

Generate both output files. Template and instructions in `assets/report-template.html`.

---

## The verdict format

The chairman output follows this structure exactly:

```
0. INTAKE GAPS          What the plan does not specify. Stated before anything else.

1. PLAN HEALTH SCORECARD Six scores, composite, and the spread between highest and lowest.

2. WHERE THE COUNCIL AGREES
                        Points multiple advisors reached independently.
                        Independent convergence is the highest-confidence signal available.

3. WHERE THE COUNCIL CLASHES
                        Genuine disagreements, presented unsmoothed, with why each
                        side is reasonable.

4. BLIND SPOTS PEER REVIEW CAUGHT
                        What surfaced only in round two.

5. THE BRAND / PERFORMANCE SPLIT
                        Where this plan actually sits on the equity-vs-activation
                        axis, and whether that placement is deliberate or accidental.

6. THE VERDICT          Exactly one of: GO / GO WITH CHANGES / REWORK / KILL.
                        No hedging, no "it depends."

7. THE THREE CHANGES THAT MATTER MOST
                        Ranked, specific, each tied to which advisor raised it.

8. THE ONE THING TO DO MONDAY
                        A single concrete action. Not a list.
```

---

## Three rules that keep the council coherent

**1. The measurement gate.** A plan with no way to know whether it worked is capped at REWORK, regardless of how strategically elegant it is. This is a hard rule for the chairman, not a soft preference. It is the discipline most marketing plans lack, and the council exists partly to enforce it.

**2. The chairman can override the majority.** If five advisors approve and the Rival's single objection is the strongest argument in the room, the chairman sides with the Rival and explains why. The chairman judges argument quality, not vote count.

**3. Distinctiveness beats agreement.** If two advisors produce substantially similar output, note it as a roster problem in the transcript. The Brand Strategist and the Creative Director are the pair most at risk of collapsing into each other — hold the Strategist to *positioning* and the Creative Director to *attention*, and they stay separate.

---

## Output

Every session produces two files in the working directory:

```
marketing-council-report-[YYYY-MM-DD].html     visual scorecard report
marketing-council-transcript-[YYYY-MM-DD].md   full transcript
```

The transcript includes the plan as submitted, the intake table, the council brief, all six advisor responses, all six peer reviews with the anonymization mapping revealed, the reallocation answers, and the chairman's full synthesis. It is the artifact — if the user revises the plan and re-councils it, the previous transcript shows how the thinking moved.

Open the HTML report after generating it.

---

## Important notes

- **Always run the intake gate first.** Skipping it is the single most common way this skill produces useless output.
- **Always spawn all six advisors in parallel.**
- **Always anonymize before peer review.**
- **Never let the council write the plan.** This skill evaluates. If the user wants a plan written, that is a different task.
- **Six is the ceiling, not a floor.** Every added seat costs signal-to-noise in peer review. Do not add a seventh advisor for a new channel or specialty.

---

Methodology adapted from [Andrej Karpathy's LLM Council](https://x.com/karpathy/status/1962263486196867115), by way of the Claude Code [llm-council skill](https://github.com/tenfoldmarc/llm-council-skill). Marketing roster, intake gate, scoring rubric, reallocation exercise, and four-way verdict are additions specific to plan evaluation.
