# Governance Council

**An AI skill that puts your biggest decisions through a real deliberation before it gives you an answer.**

Most AI assistants, when you ask them "should I do this?", give you a tidy, agreeable, single-voice answer in about ten seconds. It sounds confident. It's often wrong in ways you can't see, because nothing in it ever got *challenged*. There was no one in the room whose job was to attack the plan, no one protecting your actual goal, no one on record saying "I disagree, and here's exactly when I'd be proven right."

Governance Council fixes that. It's a portable skill you drop into an AI coding assistant (or run by hand in any chatbot) that turns one hard question into a **structured, adversarial deliberation** — a whole council of specialists and decision-makers who argue, refute each other, and are finally forced into a single binding ruling. Nothing reaches that ruling without having survived an attack first. And every position that *lost* the argument is written down and kept, so you can see what the decision cost and when it might come back to bite you.

Think of it as convening a board of directors, a red team, and a devil's advocate — on demand, in a few minutes — for any decision where being wrong is expensive.

---

## Table of contents

- [Who this is for](#who-this-is-for)
- [What problem it actually solves](#what-problem-it-actually-solves)
- [How it works — the five-stage pipeline](#how-it-works--the-five-stage-pipeline)
- [What you get back: the ruling](#what-you-get-back-the-ruling)
- [The design principles that make it trustworthy](#the-design-principles-that-make-it-trustworthy)
- [Cost, intensity, and how to control it](#cost-intensity-and-how-to-control-it)
- [Three ways to run it (works on any AI)](#three-ways-to-run-it-works-on-any-ai)
- [Installation](#installation)
- [How to use it](#how-to-use-it)
- [A worked example](#a-worked-example)
- [What's in this repository](#whats-in-this-repository)
- [Frequently asked questions](#frequently-asked-questions)
- [Honest limitations](#honest-limitations)
- [License](#license)

---

## Who this is for

Anyone facing a decision that is **consequential, contested, or hard to reverse**, who has access to an AI assistant and wants more than a confident-sounding opinion. For example:

- **Founders and operators** — "Should we launch this product? Pivot the business model? Take this acquisition offer? Raise now or bootstrap?"
- **Engineers and architects** — "Should we split the monolith? Migrate to this database? Adopt this framework? Rewrite or refactor?"
- **Investors and buyers** — "Is this the right price for this asset? Should I make this big purchase?"
- **Individuals at a crossroads** — "Should I leave my job to do this full-time? Take this offer? Move?"
- **Anyone with a strategy document, market study, or proposal** that they want genuinely stress-tested and critiqued — not politely summarized.

If you can phrase your situation as *"should I / should we do X?"* and the wrong answer would cost you real money, time, or trust, this is built for you.

It is **deliberately overkill for small stuff.** If the decision is cheap to reverse or low-stakes, just ask your AI normally. This skill earns its cost only when deliberating is much cheaper than being wrong.

---

## What problem it actually solves

When you ask a single AI (or a single person) for a verdict on something important, five predictable things go wrong. Governance Council is engineered specifically to defeat each one:

| The failure | What it looks like | The fix built into this skill |
|---|---|---|
| **Rubber-stamping** | Everything politely agrees with your plan. You feel validated and learn nothing. | A **Skeptic role is mandatory** at every stage. Specialists are *required* to refute, not just agree. |
| **Averaging** | "On balance, proceed cautiously." A mushy non-answer that splits every difference. | Every synthesis and the final chair are instructed to **decide, never average.** Conflicts are resolved by name — who was overruled, and why. |
| **Evidence laundering** | A guess gets repeated three times until it sounds like a proven fact. | Every number is **tagged** (verified / triangulated / inferred / refuted) and the tag travels with the claim through every stage. Invented statistics are banned. |
| **Buried dissent** | The one person who disagreed gets softened into a footnote until they vanish. | A dedicated **dissents section** preserves every objection, whether it was accepted, overruled, or is waiting to be proven right. |
| **Goal drift** | The council optimizes for generic prudence — money, safety, elegance — while *your actual goal* quietly leaves the room. | A **Guardian seat** holds your stated mission with formal veto power. It exists to catch exactly this. |

The result isn't just a smarter answer. It's an *auditable* one. Six months later you can read the ruling and see precisely why the decision was made, what it traded away, and what would have to change to reconsider it.

---

## How it works — the five-stage pipeline

You give it a decision and your goal. It runs a debate in five stages, each feeding the next. Here's the whole flow in plain language:

```
Stage 0 · CHARTER      Frame the fight. Name the decision, your mission, the sharpest
                       tension, and split the problem into specialist areas and council seats.

Stage 1 · AREA LEADS   One specialist per area studies the material and takes a real position:
                       what's RIGHT (agree), what's WRONG (refute), what's MISSING (expand).

Stage 2 · DEBATE       Each specialist gets attacked by a Skeptic (find the weak points) and
                       strengthened by a Builder (add what they missed).

Stage 3 · SYNTHESIS    Each area is reconciled into one firm position — conceding real hits,
                       absorbing real improvements, defending what held. It DECIDES; it never averages.

Stage 4 · COUNCIL      3–7 role-based "seats" (a CFO, a Risk officer, a Guardian of your
                       mission, etc.) each judge ALL the areas from their own mandate. They
                       vote blind — they can't see each other's verdicts, so they can't collude.

Stage 5 · RULING       A Chair reads everything and issues ONE binding ruling: the verdict,
                       the decisions per area, the kill-criteria, the budget, a roadmap you can
                       start this week, and every dissent preserved for the record.
```

A few of these ideas deserve a proper explanation, because they're what separate this from "ask the AI twice":

**Areas** are the decision cut into specialist domains — not by org chart, but by *what could make the ruling wrong*. For a business venture that might be: who the buyer is, the pricing/unit-economics, how you acquire customers, the product itself, the legal structure, and the mission/ethics. The test for a good split: "if the final ruling turns out wrong, which area failed?" If no area would own the failure, there's a hole in the plan.

**Seats** are the council members, defined as *mandates, not job titles*. Each seat protects one thing and is allergic to one kind of failure. A CFO seat kills anything that spends money before the decisive number is proven. A Risk seat watches for regulatory and reputational cliffs. Two seats are always present: a **Chair** (owns coherence, sequencing, and the final call) and a **Guardian** (holds your mission and can formally dissent).

**The Guardian** is the quiet hero of the whole design. Left alone, any council — human or AI — drifts toward optimizing whatever is easiest to measure: cost, speed, safety. Your actual reason for doing the thing gets traded away one reasonable-sounding compromise at a time. The Guardian's entire job is to stand on your stated mission and challenge every drift away from it, *even when the drift is economically rational.* Its dissent power is real: the Chair must either satisfy it or preserve its objection prominently in the ruling.

**The Skeptic and the Builder** are the adversarial engine. The Skeptic hunts for the weakest claims, hidden assumptions, and any place someone treated a guess as a fact — and every attack has to imply a fix. The Builder does the opposite: adds overlooked options and sharpens vague recommendations into something you could actually execute. Both are bound by the same evidence rules.

---

## What you get back: the ruling

The output is a single governance document. It always contains, in this order:

1. **The verdict, up front, in the first sentence** — one of: `Proceed` / `Proceed with conditions` / `Re-angle` / `Kill`. No burying the lede.
2. **How your source material was judged** — upheld, amended, or extended, and where.
3. **Binding decisions per area** — concrete enough to act on *this week*, not vague principles.
4. **Kill criteria** — the pre-committed gates that would stop the project, each with a trigger metric, a date, and a "no re-litigation" clause. These are written *before* results come in, because a gate you negotiate *after* the bad number arrives always loses.
5. **Budget / resources** — hard caps, and a list of the spends that were explicitly rejected.
6. **A roadmap** — phased, gated, and starting immediately.
7. **Preserved dissents** — every formal objection, whether it was triggered, incorporated, or overruled, and what would give it standing again. This is the ruling's audit trail.
8. **Limits** — what the deliberation honestly *couldn't* settle: unverified facts, small sample sizes, open questions. Stated plainly so nobody over-reads the ruling later.

The template for this document lives in [`governance-council/assets/ruling-template.md`](governance-council/assets/ruling-template.md).

---

## The design principles that make it trustworthy

These are the non-negotiables. Remove any one of them and you get an expensive-looking rubber stamp instead of a real deliberation:

- **Nothing survives unchallenged.** Every position is attacked before it can reach the ruling, and every adopted position names what it overruled.
- **Evidence tiers travel with every claim.** A load-bearing number is tagged `VERIFIED` (primary source, checked), `TRIANGULATED` (directional but from a commissioned/advocacy/dated source), `INFERRED` (reasoned, not measured), or `REFUTED` (failed checking — and once refuted, *never cited again*). This single mechanic is what stops a guess from hardening into a "fact" through repetition.
- **No invented precision.** A confident fabricated statistic — a made-up "$350k migration cost" or "60% success rate" — is treated as the canonical failure, worse than admitting you don't know. Numbers either trace to a real source or are openly marked as inference with their reasoning shown.
- **No borrowed authority.** The skill will never describe its process as something it wasn't. If it ran in a lightweight single-context mode, it says so — it won't claim the seats "reviewed independently" when they didn't. It is honest about its own fidelity, because a ruling that lies about how it was made has already failed the one fact it fully controls.
- **Decide, don't average.** Splitting the difference is prohibited. Genuine unresolved disagreements are passed up *labeled as unresolved* — but the mushy "balanced approach" that quietly ignores the real tension is banned.

---

## Cost, intensity, and how to control it

This is a real deliberation, so it does real work — which means real compute (tokens). The skill is built to let you dial the intensity to match the stakes, and it will always tell you the expected cost and ask before spending big.

There are two knobs: **scale** (how many specialists and seats) and **mode** (whether they run in parallel or one after another).

### Scale — how thorough

| Scale | Areas | Seats | Agents | When to use it |
|---|---|---|---|---|
| **Quick** | 3 | 3 | ~8–10 | Personal decisions, a fast first pass, time-boxed reviews |
| **Standard** | 4–5 | 4–5 | ~20 | Most real business or technical decisions |
| **Full** | 6–7 | 5–7 | ~30+ | Ventures, migrations — anything with a budget and a blast radius |

### Mode — how it runs (this is the *bigger* cost lever)

- **Sequential (cheapest):** one AI plays every role in turn. Roughly **8× cheaper** than parallel. Works on *any* AI system, including a plain chatbot.
- **Parallel (fastest, most rigorous):** every specialist and seat is a genuinely independent agent running at once. Buys true independence and wall-clock speed, at ~8× the cost. Requires an AI that can spawn sub-agents (e.g. Claude Code).

**The honest default the skill recommends:** for the vast majority of "should I do X?" questions, run **Quick scale, sequential mode** first. It costs about 100,000 tokens (a few minutes and a few cents-to-dollars depending on your model) and still produces the *complete* ruling document. Only escalate to a bigger or parallel run if that first pass surfaces a genuinely close call worth the extra spend.

The skill also includes finer levers for cost-conscious runs: dropping the Builder role, running the mechanical stages on a cheaper model while reserving the strong model for the Chair's final ruling, and a budget-aware mode that scales itself to a token ceiling you set and stops early rather than blowing past it.

**It always states the plan and the price before spending.** A council that bankrupts its own budget before it rules has failed the first lesson it teaches.

---

## Three ways to run it (works on any AI)

The skill is portable by design. It detects what your environment can do and picks the best available mode automatically:

- **Mode A — Claude Code with the Workflow tool** *(best)*: deterministic parallel fan-out with structured outputs. The most rigorous and repeatable way to run it. A ready-to-adapt script is included.
- **Mode B — parallel sub-agents**: same stages using an agent tool, without the full Workflow harness.
- **Mode C — sequential, single-context**: no sub-agents at all — one model runs every role in disciplined turns, or across several chat sessions using handoff files. **This is what makes the skill work in ChatGPT, Gemini, claude.ai, or a local model.**

If you're not on Claude Code, Mode C is your path, and it's a first-class citizen — the skill includes specific discipline (hard role switches, attack quotas, fresh-session handoffs) to protect adversarial quality even when everything runs in one context.

---

## Installation

This skill follows the [Agent Skills](https://code.claude.com/docs/en/skills) format. The actual skill lives in the [`governance-council/`](governance-council/) folder of this repo — that's the folder you install.

### For Claude Code (recommended)

Copy the `governance-council/` folder into your skills directory:

**Personal skill (available in every project):**
```bash
git clone https://github.com/makesupply/governance-council.git
cp -r governance-council/governance-council ~/.claude/skills/
```

**Project skill (shared with a repo/team), from your project root:**
```bash
git clone https://github.com/makesupply/governance-council.git
cp -r governance-council/governance-council .claude/skills/
```

On Windows (PowerShell), the destination is `C:\Users\<you>\.claude\skills\` for a personal skill. Copy the inner `governance-council` folder there.

Restart Claude Code (or start a new session) and it will discover the skill. Confirm with `/help` or just invoke it (below).

### For other AI assistants (ChatGPT, Gemini, claude.ai, local models)

There's nothing to install — the skill is just structured instructions. Open [`governance-council/SKILL.md`](governance-council/SKILL.md) and its `references/` files, paste the relevant parts into your assistant, and it will run in **Mode C** (sequential). The [`references/sequential-mode.md`](governance-council/references/sequential-mode.md) file is written specifically for this path.

---

## How to use it

Once installed in Claude Code, you don't need to memorize anything. Just describe your decision naturally. The skill triggers on requests like:

- *"Should we migrate our backend to microservices? Stress-test this for me."*
- *"Here's my market study and a critique of it — reconcile them into one verdict."* (attach or paste the document)
- *"I'm thinking of leaving my job to sell on Etsy full-time. Red-team this decision."*
- *"Convene a council on whether to accept this acquisition offer."*

You can also invoke it explicitly by name (e.g. `/governance-council` in Claude Code) if your assistant supports slash commands.

**The one thing it will always ask you for** is your **mission** — what the decision is actually *for*. This is the single most important input. Without it, the council optimizes for generic caution instead of your real objective. A good mission names a beneficiary and a priority ("enrich beginner cooks with pro-grade guidance — value that's obvious to experts but transformative for them," or "correctness over speed, speed over cost"). If you don't give one, it'll help you write it, then confirm before proceeding.

For anything beyond a quick sequential run, it will show you the scale, the mode, and the estimated cost, and wait for your go-ahead.

---

## A worked example

Here's the shape of a real full-scale run (anonymized), so you can see what "good" looks like:

> **Subject:** "Adjudicate this adversarial review of our market study — what's the best way to start, with a full business structure?"
> **Mission:** Enrich an underserved population with a product whose value is trivial to experts but transformative to them.
> **Central tension:** The economically rational buyer is *not* the mission's beneficiary.
> **Areas (6):** buyer & segmentation · offer & pricing · acquisition & customer cost · product & delivery · legal/financial structure · mission & trust ethics.
> **Seats (5):** Chair, CFO, CMO, Risk & Compliance, Mission Guardian.

**What the ruling did:** It found the source review "substantially upheld, materially amended, significantly extended." It ratified a pivot toward the profitable buyer — but *only as a sales channel*, while protecting the mission's real beneficiary with a written constitution, a calendar-triggered backstop that forces a decision if the mission gets deferred too long, and pre-committed kill criteria. Two council seats formally dissented; both dissents were incorporated as *binding conditions* and preserved in the record.

Notice what that ruling is not: it's not "on balance, proceed carefully." It's a specific, funded, gated plan that names its own failure conditions and keeps its dissenters on the record. That's the quality bar.

---

## What's in this repository

```
governance-council/            ← the installable skill (copy this folder)
├── SKILL.md                   The main skill: pipeline, cost model, all mechanics
├── assets/
│   └── ruling-template.md     The skeleton of the final ruling document
├── references/
│   ├── charter-design.md      How to design areas, seats, and constitutions (+ domain presets)
│   ├── prompts.md             Every role's prompt template (lead, skeptic, builder, seat, chair)
│   ├── claude-code-workflow.md  Ready-to-run parallel Workflow script (Mode A)
│   └── sequential-mode.md     How to run it on any AI, single-context (Mode C)
└── evals/
    └── evals.json             Evaluation scenarios used to test the skill's quality

README.md                      ← you are here
LICENSE
```

Each `references/` file is loaded by the AI only when it's needed for the stage it's running, which keeps the skill lightweight until it's actually deliberating.

---

## Frequently asked questions

**Is this just "ask the AI to argue with itself"?**
No. The discipline is the whole point. Role switches are hard, the skeptic has attack quotas, seats vote blind so they can't collude, evidence tags travel with every claim, and dissents are preserved by force. Those mechanics are what turn "argue with yourself" into something you can actually trust. Skip them and you get theatre.

**Does it need the internet or any special tools?**
No. It runs entirely on the AI's own reasoning over the material you give it. If your AI *does* have research tools, specialists may run a few quick fact-checks — but the skill prioritizes rigorous analysis over web-surfing, and everything it can't verify is openly marked as inference.

**Will it make the decision *for* me?**
It issues a clear verdict and a plan — that's the point of a *binding* ruling. But it also hands you the full reasoning, the dissents, and the limits, so you can override it with your eyes open. It's a decision *instrument*, not a decision *replacement*. It explicitly does not give personalized legal, tax, medical, or investment advice, and says so where relevant.

**How much does a run cost?**
A quick sequential run is ~90,000–115,000 tokens. A full parallel run can reach 2–2.6 million. The skill always tells you before it spends. See [Cost and intensity](#cost-intensity-and-how-to-control-it).

**Can I use it on ChatGPT or Gemini?**
Yes — that's Mode C. There's nothing to install; you paste the instructions in. It loses *structural* independence (everything runs in one context) but keeps the discipline, and it's honest about the difference in the ruling.

---

## Honest limitations

In the spirit of the skill itself, here's what it can't do:

- **It can't verify facts it has no access to.** If a load-bearing number (a salary, a profit figure, a market size) can't be checked, the ruling lists it as a *verification blocker*, not a settled truth. Garbage in, garbage out still applies — but at least the garbage is labeled.
- **Sequential mode is procedurally, not structurally, independent.** One model reviewing its own earlier reasoning is weaker than genuinely separate agents. The skill discloses which mode it ran in and never claims independence it didn't have.
- **It is not a substitute for a licensed professional** on legal, tax, medical, or regulated financial matters. It will flag where you need one.
- **It is expensive on purpose.** For low-stakes or easily-reversible choices, it's the wrong tool — just ask your AI normally.

---

## License

Released under the [MIT License](LICENSE). You're free to use, modify, and share it. If it helps you make a better call on something that mattered, that's the whole point.

---

*Governance Council is an Agent Skill — a self-contained package of instructions and templates that extends an AI assistant with a specific, repeatable capability. This one gives your AI the ability to hold a real board meeting about your hardest decisions, and to be honest with you about what it found.*
