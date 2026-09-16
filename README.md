# Marketing Council — A Claude Code Skill

Six advisors who cannot agree with each other tear apart your marketing plan, review each other blind, and hand you a one-word verdict.

In use by the CEO and marketing team at **CHS Education Limited**, who run real marketing plans
through it.

See a complete run in [`examples/`](examples/) — the plan that went in, the full council
transcript, and the scorecard that came out.

Adapted from [Andrej Karpathy's LLM Council](https://x.com/karpathy/status/1962263486196867115) methodology and the [llm-council skill](https://github.com/tenfoldmarc/llm-council-skill), retargeted from open decisions to marketing plan evaluation.

---

## The problem

Ask any AI to review your marketing plan and the answer bends to your framing. Present the campaign enthusiastically, get validation. Present the same campaign anxiously, get concerns. Same plan, opposite readings, no way to tell which one was real.

Worse: a single reviewer evaluates a plan from wherever it happens to be standing. It might catch the weak positioning and miss that the CAC math is impossible, or catch the math and miss that no actual human would understand the message.

## The roster

Six advisors in three opposing pairs. Built on tensions, not job titles — the common mistake is staffing a marketing council by channel (a Social person, an SEO person, an Email person), which produces six advisors who all say "yes, and here's how my channel helps."

| Pair | Advisor | Owns | Argues with |
|---|---|---|---|
| **Horizon** | The Brand Strategist | Long-term equity, ownable positioning | Performance Marketer |
| | The Performance Marketer | CAC, payback, funnel arithmetic | Brand Strategist |
| **Judgment** | The Customer | Zero marketing literacy, reacts as the real buyer | The Rival |
| | The Rival | Competitive defensibility, response-fragility | The Customer |
| **Ambition** | The Creative Director | Interestingness — boring is the biggest risk | The Operator |
| | The Operator | Capacity, timeline, what ships Monday | Creative Director |

## What it does

1. **Intake gate** — checks the plan for nine required components and reports what's missing. Four or more missing and it refuses to convene, because six advisors evaluating six different sets of invented assumptions is worse than no review.
2. **Context scan** — pulls `CLAUDE.md`, `memory/`, brand files, past campaign results.
3. **Six advisors in parallel** — each scores the plan 1–10 on their dimension and names a fatal flaw, a kill criterion, and one fix.
4. **Blind peer review** — responses anonymized A–F with randomized mapping, then each advisor reviews all six without knowing who wrote what.
5. **Budget reallocation** — every reviewer must move 20% of the budget. Nothing exposes a plan's weak point faster than making someone defund part of it.
6. **CMO chairman** — synthesizes into a scorecard, the agreements, the clashes, the blind spots, the brand/performance split, and a verdict.
7. **Report + transcript** — HTML scorecard and full markdown record.

## The verdict

Exactly one of four. No hedging, no fifth option.

- **GO** — run it as written. Rare.
- **GO WITH CHANGES** — run it, but the listed changes aren't optional.
- **REWORK** — don't run it yet.
- **KILL** — the premise is wrong.

## The measurement gate

A plan with no way to know whether it worked is capped at **REWORK**, regardless of how good everything else is. Hard rule, not a preference. It's the discipline most marketing plans lack.

---

## Install

Requires [Claude Code](https://claude.com/claude-code). No API keys, no dependencies, nothing to build — the skill is plain markdown that Claude Code reads.

Clone it into your personal skills directory:

```bash
git clone https://github.com/zihadizisan/marketing-council.git ~/.claude/skills/marketing-council
```

On Windows, the same path is `%USERPROFILE%\.claude\skills\marketing-council`.

To scope it to one project instead of your whole machine, clone into that project's `.claude/skills/` directory:

```bash
git clone https://github.com/zihadizisan/marketing-council.git .claude/skills/marketing-council
```

Start a new Claude Code session and the skill is picked up automatically. Confirm it loaded by typing `/marketing-council`.

## Use

Trigger phrases:

- `marketing council`
- `CMO review`
- `review my marketing plan`
- `evaluate this campaign`
- `red team this launch`
- `pressure-test this campaign`

Then paste or point at the plan. The richer the input, the sharper the output — but the intake gate will tell you if it's too thin to evaluate.

**Good inputs:** campaign briefs, launch plans, GTM strategies, positioning docs, funnel strategies, quarterly plans, channel budget allocations.

**Not for:** writing copy, drafting ads, generic marketing questions, channel how-tos. This skill evaluates a plan; it doesn't write one.

---

## Files

```
SKILL.md                          the pipeline
references/intake-checklist.md    the nine components and the convene/refuse branch
references/advisors.md            all six advisor prompts + anti-drift check
references/peer-review.md         reviewer prompt, chairman prompt, standing rules
assets/report-template.html       HTML scorecard template
LICENSE                           MIT
NOTICE                            what is derived, what is original
examples/                         a complete worked run, input to verdict
```

## Design notes

**Why six and not five.** The generic council uses five thinking styles. Marketing plans have more distinct failure surfaces — a plan can have great positioning and dead creative, or perfect creative and impossible economics. Six is the ceiling though, not a starting point: every added seat costs signal-to-noise in peer review. Don't add a seventh for a new channel.

**Why the NOT YOUR JOB lines matter.** Marketing roles share vocabulary and drift toward a general "here are my thoughts" register if allowed. Each advisor prompt explicitly names what belongs to someone else. The Brand Strategist and Creative Director are the pair most at risk of collapsing into each other — one is held to *positioning*, the other to *attention*.

**Why anonymity is load-bearing.** With names attached, reviewers defer to whoever sounds most senior — in marketing that's almost always the Brand Strategist, regardless of argument quality.

---

## Licence

MIT — see [LICENSE](LICENSE), with derivation details in [NOTICE](NOTICE). Adapted from the MIT-licensed
[llm-council skill](https://github.com/tenfoldmarc/llm-council-skill) and Andrej Karpathy's
[LLM Council](https://x.com/karpathy/status/1962263486196867115) methodology; the marketing roster,
intake gate, scoring, budget reallocation, measurement gate and verdict are additions here.
