---
name: governance-council
description: Convene a multi-stage adversarial deliberation ("governance council") that adjudicates a high-stakes decision, plan, proposal, or review document and returns a binding ruling with kill criteria, a roadmap, and preserved dissents. Pipeline — area leads (agree/refute/expand) → skeptic + builder debate → decide-not-average synthesis → role-based seats incl. a Guardian with formal dissent power → chair's final ruling. Use whenever the user wants a decision stress-tested, red-teamed, reviewed by a board/panel/council, or asks "should we / should I do X" about anything consequential (launch, migration, investment, pivot, acquisition, business model, architecture), wants competing analyses reconciled into one verdict, or wants a strategy/market/architecture document critiqued and ruled on — even if they never say "council". Scales quick/standard/full; degrades from parallel subagents (Claude Code) to sequential single-context mode on any AI system.
---

# Governance Council

A structured adversarial deliberation that turns "here's a big decision" into a binding,
dissent-preserving ruling. Nothing survives into the final ruling without having been
attacked first, and every adopted position names what it overruled.

Use it when the decision is **consequential, contested, or hard to reverse**. It is
deliberately expensive — a full run spawns 25–35 agents. For a quick opinion or a
low-stakes choice, answer directly instead; this skill's cost is only justified when
being wrong is much more expensive than deliberating.

## The pipeline

```
Stage 0  CHARTER      Frame the deliberation (subject, constitution, areas, seats, scale)
Stage 1  AREA LEADS   One specialist per area reads the source material and takes a position:
                      AGREE (with reasons) / REFUTE (with reasons) / EXPAND (what's missing)
Stage 2  DEBATE       Each lead is attacked by a SKEPTIC and strengthened by a BUILDER
Stage 3  SYNTHESIS    One synthesizer per area reconciles lead + debate — DECIDES, never averages
Stage 4  COUNCIL      3–7 role-based seats each judge ALL areas from their seat's mandate;
                      a Guardian seat holds the user's stated mission with formal dissent power
Stage 5  RULING       The chair issues one binding ruling: decisions per area, kill criteria,
                      budget/resources, roadmap, and every dissent preserved for the record
```

## Step 1 — Draft the charter

Before spawning anything, write the charter. It is the deliberation's constitution and
every agent prompt embeds it. Read `references/charter-design.md` for area/seat design
guidance and domain presets (business venture, technical architecture, investment,
strategy review, personal decision). A charter contains:

1. **Subject** — the decision or document under review, stated as a question the ruling
   must answer (e.g., "what is the best way to start, with a full business structure?").
   When there is no document — the subject is a decision described in the user's message —
   the stated facts ARE the source material: restate them in the charter, and any
   unverified load-bearing fact (a salary, a profit figure, an insurance arrangement)
   is listed in the ruling's Limits as a verification blocker, not silently assumed true.
2. **Constitution** — the user's mission, goal, or values in one or two sentences. Every
   agent must answer to it, and the Guardian seat enforces it. If the user hasn't stated
   one, ask — or derive it from context and confirm. This is the single most important
   input: without it, the council optimizes for generic prudence instead of the user's
   actual objective.
3. **Central tension** — name the sharpest conflict up front (economics vs mission,
   speed vs safety, growth vs simplicity). Deliberations that don't name their tension
   produce rulings that quietly ignore it.
4. **Areas** (4–7) — the decision decomposed into specialist domains, each with a focus
   paragraph containing the specific questions that area must answer.
5. **Seats** (3–7) — role-based council perspectives. Always include a chair and a
   Guardian; add seats for whatever can kill the decision (money, growth, risk, legal…).
6. **Scale** — quick / standard / full (see table below).
7. **Evidence rules** — the tier protocol (below) plus any claims already known to be
   refuted, which are banned from citation.

**Confirm the charter AND the cost with the user before launching** any run that spawns
subagents (Mode A/B) or a standard/full run in any mode. This skill is expensive by
design; the user must consent to the spend, not just the analysis. State the scale, the
mode, and the rough token cost from the table below, and get a yes. A quick run in Mode C
(single context, no subagents) on a clearly-scoped request may proceed directly — it is
the cheap default.

### Scale and cost

Token figures are measured from real runs (fable-class model), not guesses. They move the
decision more than any other knob, so lead with them.

| Scale | Areas | Debate | Seats | Agents | Mode C tokens (seq.) | Mode A/B tokens (parallel) | When |
|---|---|---|---|---|---|---|---|
| quick | 3 | skeptic only | 3 | ~8–10 | **~90–115K** | ~700–850K | Personal decisions, time-boxed reviews, first pass |
| standard | 4–5 | skeptic + builder | 4–5 | ~20 | ~250–350K | ~1.3–1.8M | Most business/technical decisions |
| full | 6–7 | skeptic + builder | 5–7 | ~30+ | ~600–800K (long transcript) | ~2.0–2.6M | Ventures, migrations, anything with a budget and a blast radius |

**The single biggest cost lever is mode, not scale.** Parallel modes (A/B) cost roughly
**7–8× the tokens** of sequential Mode C at the same scale, because every agent
independently re-reads the charter and source material. Parallel buys wall-clock speed and
genuine independence; sequential buys an ~8× discount. For most real decisions, run
**quick scale in Mode C first** (~100K tokens, a few minutes) — it is usually enough to
decide, and you can escalate to a parallel or full run only if the first pass surfaces a
close call worth the spend. Reserve full parallel runs for genuinely irreversible,
high-blast-radius decisions where being wrong costs far more than ~2M tokens.

### Controlling token usage (say the plan before you spend)

Before launching, state a one-line cost plan and pick levers to fit the user's budget:
1. **Mode** — Mode C over A/B is the ~8× lever. Default to C unless the user needs speed
   or true independence.
2. **Scale** — quick over standard over full; drop an area or a seat rather than adding.
3. **Skeptic-only** — omit the builder (quick scale already does). Saves one agent per area.
4. **Model tiering** (Mode A/B) — run the mechanical stages (leads, skeptics, builders,
   synthesizers) on a cheaper model and reserve the strong model for the two stages where
   judgment compounds: the council seats and the chair's ruling. In the Workflow script,
   pass `model: 'haiku'` or `model: 'sonnet'` on lead/debate/synth agents and leave the
   chair on the session model with `effort: 'high'`. This can halve a parallel run's cost
   with little quality loss, because the leads' job is coverage, not final judgment.
5. **Effort tiering** — reserve high reasoning effort for the chair (and optionally the
   Guardian seat); the mechanical stages run fine at default effort.
6. **Budget-aware fan-out** (Mode A) — the Workflow tool exposes a `budget` global; the
   script in `references/claude-code-workflow.md` shows how to scale area/seat count to a
   token target and stop early when the budget is spent. Use it when the user gives a "+Nk"
   ceiling.
7. **Pre-run estimate** — for any parallel or standard/full run, print the estimated token
   cost from the table and get explicit approval. A council that bankrupts its own budget
   before it rules has failed rule 1 of the thing it teaches.

Guidance: for the vast majority of "should I do X" questions, **quick + Mode C on a
mid-tier model is the right answer** — it is ~100K tokens and produces the full ruling
document. Escalate deliberately, not by default.

## Step 2 — Pick the execution mode

- **Mode A — Claude Code with the Workflow tool** (preferred): deterministic fan-out,
  structured outputs, parallel debate. Read `references/claude-code-workflow.md` and
  adapt the ready-made script. Only launch a Workflow when the user has opted into
  multi-agent orchestration; invoking this skill for a council run counts as that opt-in.
- **Mode B — parallel subagents without Workflow**: same stages using the Agent tool;
  batch independent agents (all leads at once, all seats at once) in single messages.
- **Mode C — sequential single-context (any AI system)**: no subagents at all. Run every
  role as a sequential pass with strict role-switching discipline, or across multiple
  chat sessions using handoff files. Read `references/sequential-mode.md`. This mode is
  what makes the skill portable to ChatGPT, Gemini, claude.ai, or a local model.

Detect the mode from what's available; degrade A → B → C without asking.

## Step 3 — Run the stages

All role prompt templates live in `references/prompts.md`. Fill their `{{PLACEHOLDERS}}`
from the charter. The mechanics below are what make the output trustworthy — they are
the skill; skipping any of them produces an expensive-looking rubber stamp.

**Evidence tiers.** Every load-bearing quantitative claim is tagged:
- `VERIFIED` — primary source, survived adversarial checking
- `TRIANGULATED` — directionally supported but commissioned/advocacy/dated
- `INFERRED` — reasoned, not measured; flagged as such
- `REFUTED` — failed verification; **never cited again** once tagged

Why: deliberations die from evidence laundering — an INFERRED claim gets repeated three
times and arrives at the council sounding VERIFIED. Tags travel with claims.

Two corollaries, both observed as real failures in untooled runs:
- **No invented precision.** A quantitative claim either traces to the source material or
  a checked source, or it carries INFERRED with its derivation shown. An invented
  "$350–450k migration cost" or "60–70% base rate" presented as a finding is the
  canonical failure — confident fabricated statistics are worse than none.
- **No borrowed authority.** Never describe the process as something it wasn't: a
  single-context run must not claim seats "reviewed independently." The fidelity note
  is part of the ruling's evidence, not decoration.

**Agree / refute / expand.** Leads must do all three. A lead who only agrees is a
rubber stamp; a lead who only refutes is a heckler; a lead who doesn't expand adds
nothing the source didn't already contain.

**The skeptic attacks, the builder strengthens.** The skeptic's job description:
find the weakest claims, hidden assumptions, and evidence-discipline violations —
every attack should imply a fix. The builder adds overlooked options and sharpens
recommendations. Same evidence rules bind both.

**Synthesis decides.** The synthesizer is explicitly instructed: where the skeptic
landed a real blow, concede and adjust; where the builder added value, absorb it;
where the lead was right, defend it. *Do not average.* Averaged syntheses produce
mush the council can't rule on. Unresolved disagreements are sent to the council
labeled as such — that is allowed; splitting the difference is not.

**Seats judge everything from one mandate.** Each seat reads all area briefs and
issues: a verdict, specific overrides demanded (with reasons), ordered priorities,
and a formal dissent or "none". The Guardian seat exists so the user's stated mission
cannot be quietly traded away for whatever the other seats optimize; its dissent
power is real — the chair must either satisfy it or preserve the dissent prominently.

**The chair resolves, never averages.** The ruling must explicitly adjudicate the
source material (upheld / amended / extended), resolve each inter-seat conflict by
name (who was overruled and why), and reconcile the central tension with an
enforcement mechanism — not a hope. A ruling that "balances all perspectives" has
failed; a ruling that says "the CFO's objection is incorporated as a funding
condition; their underlying position stands in the record" has succeeded.

## Step 4 — Produce the ruling document

Use `assets/ruling-template.md` as the skeleton. The non-negotiable sections:

- **Verdict up front** — proceed / proceed-with-conditions / re-angle / kill, in the
  first sentence, followed by how the source material was adjudicated.
- **Binding decisions per area** — concrete enough to execute this week.
- **Kill criteria** — pre-committed gates written BEFORE results arrive, each with its
  trigger metric and what firing it means. Distinguish automatic kills (a gate fires,
  no renegotiation) from judgment kills (a named decision-maker, in writing). Each gate
  carries a commitment device: a dated review or decision point and a no-relitigation
  clause. Why pre-committed: gates negotiated after the number arrives always lose,
  and gates without dates die by drift.
- **Resources/budget** — what the plan costs, hard-capped, with rejected spends listed.
- **Roadmap** — phased, gated, starting this week.
- **Dissents preserved** — every formal dissent, whether it was triggered, incorporated,
  or overruled, and what would give it standing again. Dissents are the ruling's audit
  trail; erasing them converts a deliberation back into an opinion.
- **Limits** — what this deliberation cannot settle (sample sizes, unverified facts,
  open questions), stated plainly so the ruling can't be over-read.

Deliver the ruling as a document (markdown; render to PDF only if asked). For standard
and full runs, also preserve the full record (area analyses, debates, seat verdicts)
as appendix chapters — the record is what makes the ruling auditable later, and in
Mode C it is also the proof that the passes actually ran.

## Failure modes to design against

- **Rubber-stamping** — every stage politely agreeing. Antidote: the skeptic role is
  mandatory, and leads must produce refutations, not just agreements.
- **Seat collusion** — seats converging because they share context. Antidote: seats
  receive area *briefs*, not each other's verdicts; only the chair sees all verdicts.
- **Averaging** — "on balance, proceed cautiously." Antidote: synthesis and chair
  prompts both carry the decide-don't-average instruction; conflicts are resolved by
  name.
- **Dissent burial** — dissents softened into the summary until they vanish.
  Antidote: a dedicated dissents section that states each one's disposition.
- **Evidence laundering** — inference hardening into fact through repetition.
  Antidote: tier tags travel with claims through every stage.
- **Goal drift** — the council optimizing economics/safety/elegance while the user's
  actual mission quietly exits. Antidote: the constitution in every prompt and a
  Guardian seat with teeth. A useful generalized test from a real deliberation:
  *evidence about cost is not evidence about identity* — data showing a path is
  expensive tells you how to sequence, not who the decision is for.

## Cost honesty

Tell the user what scale you're running and roughly what it implies (a full run is
25–35 agents and can take 15–25 minutes wall-clock in Mode A; Mode C at full scale
produces a very long transcript and is better run at quick/standard). If mid-run
agents fail, continue with what completed and disclose the gap in Limits.
