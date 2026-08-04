# Charter design — areas, seats, constitutions, and domain presets

The charter is where the deliberation is won or lost. Thirty rigorous agents pointed at
the wrong decomposition produce a rigorous answer to the wrong question.

## Choosing areas (4–7)

Decompose by **decision surface**, not by org chart: each area is a cluster of questions
that one specialist could own end-to-end, and together the areas must cover everything
that could change the verdict. Test the set by asking: "if the ruling is wrong, which
area failed?" — if no area would own the failure, the decomposition has a hole.

Patterns that recur across domains:
- **The core dispute** always gets its own area (who is the buyer / which architecture /
  whether to invest at all). Don't smear the central question across areas.
- **Money mechanics** (pricing, unit economics, cost model) separate from **acquisition/
  adoption** (how it reaches people) — they fail differently.
- **The thing being built or bought** (product, system, asset) as its own area.
- **Legal/структural/operational reality** (entity, compliance, staffing, process).
- **Mission/values/ethics** as a real area, not a paragraph — if the constitution
  matters, it needs a specialist who operationalizes it into enforceable machinery.

Each area's focus paragraph should contain 3–6 concrete questions, name the specific
disputes it must adjudicate, and grant permission to consider options the source
material missed (this is where deliberations earn their cost).

## Choosing seats (3–7)

Seats are **mandates, not job titles** — each seat protects one thing and is allergic to
one class of failure. Always include:

- **Chair** — coherence and sequencing; allergic to doing three beachheads at once;
  owns the speed-vs-rigor trade-off and the final ruling.
- **Guardian** — holds the constitution with formal dissent power. The Guardian is what
  stops the council from optimizing itself away from the user's actual goal.

Add seats for whatever can kill the decision:

| Preset | Seats beyond Chair + Guardian |
|---|---|
| Business venture | CFO (unit economics, capital discipline, kill gates) · CMO/Growth (acquisition realism, channel-audience fit) · Risk & Compliance (regulatory exposure, platform dependency, reputational cliffs) |
| Technical architecture | Operability/SRE (runs it at 3am) · Security (attack surface, blast radius) · Delivery (migration cost, team capability, timeline realism) · Simplicity Guardian as the Guardian variant (allergic to speculative generality) |
| Investment / big purchase | Valuation (price vs value, base rates) · Risk (downside, correlation, exit) · Liquidity/Cashflow (can the decider absorb the worst case) |
| Strategy review | Economics · Execution (can THIS team do it) · Competition (how incumbents respond) |
| Personal decision | Finances (runway, worst case) · Life-fit (energy, family, reversibility) — Guardian holds the person's stated values |

Seat mandate template: *"You judge X. You are allergic to Y. You kill anything that Z.
You demand W."* Two to four sentences; specific enough that two seats could never write
each other's verdicts.

## Writing the constitution

One or two sentences, in the user's own words where possible, stating what the decision
is FOR. Good constitutions name a beneficiary and a value asymmetry ("enrich X's lives
with Y — value that is trivial for experts but transformative for them"), a priority
order ("correctness over speed, speed over cost"), or a hard boundary ("never at the
expense of Z"). A constitution that just says "be successful" gives the Guardian nothing
to enforce — push the user for the real one.

## Naming the central tension

Every consequential decision has one. Common shapes: economics vs mission · speed vs
safety · growth vs simplicity · best-for-now vs best-for-later · the segment with money
vs the segment with need. Name it in the charter and instruct agents to evaluate honest
reconciliations rather than rubber-stamping one pole. The chair's ruling must resolve it
with an enforcement mechanism (a trigger, a protocol, a funded backstop) — hopes and
"balanced approaches" are the failure mode.

## Evidence rules worth importing

- Pre-list any claims already known to be refuted; ban them by name.
- Require new evidence imported mid-deliberation to be tiered on entry.
- Useful generalized findings from real deliberations:
  - *Evidence about cost is not evidence about identity* — data showing a segment/path
    is expensive tells you sequencing, not who the decision serves.
  - *A benchmark competitor's own choices are market signal* — what the incumbent does
    with its own money is evidence, but of ITS strategy, not your mission.
  - *A gate calibrated on the wrong denominator produces false kills* — check that any
    pass/fail threshold is computed against what the tested stage can actually earn
    (e.g., a naked front-end offer can't be judged against full-funnel economics).

## Worked example (anonymized shape of a real full-scale run)

Subject: "Adjudicate this adversarial review of our market study; what is the best way
to start, with a full business structure?" · Constitution: enrich an underserved
population with a product whose value is trivial to experts but transformative to them ·
Central tension: the economically rational buyer is not the mission's beneficiary ·
Areas (6): buyer & segmentation · offer & pricing · acquisition & CAC · product &
delivery · legal/financial structure · mission & trust ethics · Seats (5): Chair, CFO,
CMO, Risk & Compliance, Mission Guardian.

Ruling shape that resulted: the source review was "substantially upheld, materially
amended, significantly extended"; the buyer pivot was ratified as a *channel* decision
while the mission beneficiary was protected by a signed constitution, a forced-decision
backstop with a calendar trigger, and pre-committed kill criteria — with two seat
dissents incorporated as binding conditions and preserved in the record. That is the
quality bar: every mechanic in SKILL.md exists because this run needed it.
