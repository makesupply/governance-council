# Mode A — Claude Code Workflow script

A ready-to-adapt script for the Workflow tool. Sections marked `// ADAPT` are filled
from the charter; everything else is the proven machinery. Structure: pipeline over
areas (lead → debate → synthesis, no barrier between areas), then a barrier before the
council (every seat needs ALL briefs), blind seats in parallel, then the chair.

Notes that matter:
- `pipeline()` lets area 1 debate while area 2's lead still writes — no wasted wall-clock.
- The barrier before Stage 4 is genuinely required (seats read all briefs) — this is the
  one place a barrier is correct.
- Seats receive **briefs only**, never each other's verdicts (collusion prevention);
  only the chair gets the full council output.
- Schemas force structured returns so the final document can be assembled mechanically.
- On agent failure, `.filter(Boolean)` keeps the run alive; disclose gaps in Limits.
- Scale knobs: quick = 3 areas / skeptic-only / 3 seats; standard = 4–5 / both / 4–5;
  full = 6–7 / both / 5–7. Chair gets `effort: 'high'`.

```javascript
export const meta = {
  name: 'governance-council',
  description: 'Adversarial multi-stage deliberation to a binding ruling',
  phases: [
    { title: 'Area Leads', detail: 'one specialist per area: agree/refute/expand' },
    { title: 'Debate', detail: 'skeptic + builder challenge each lead' },
    { title: 'Area Synthesis', detail: 'decide-not-average per area' },
    { title: 'Council', detail: 'blind role-based seats judge all areas' },
    { title: 'Final Ruling', detail: 'chair resolves conflicts, preserves dissents' },
  ],
}

// ADAPT ------------------------------------------------------------------
const SOURCE_PATH = 'C:/path/to/source-material.md'   // or inline the text in CTX
const CTX = [
  '=== DELIBERATION CONTEXT (shared) ===',
  'SUBJECT: ...the question the ruling must answer...',
  'SOURCE MATERIAL: read in full at: ' + SOURCE_PATH,
  'CONSTITUTION (the mission every agent answers to): "..."',
  'DECIDER PROFILE: ...',
  'CENTRAL TENSION: ...',
  'EVIDENCE RULES: Tag claims VERIFIED / TRIANGULATED / INFERRED / REFUTED. ' +
    'Banned (pre-refuted) claims: ...',
  'RULES: Complete sentences, plain language; evidence vs inference distinguished; ' +
    'agree, refute, or expand with reasons — no fence-sitting.',
].join('\n')

const AREAS = [ // 3 (quick) to 7 (full); focus = the questions the area must answer
  { key: 'core',    title: '...', focus: '...' },
  { key: 'money',   title: '...', focus: '...' },
  { key: 'mission', title: '...', focus: '...' },
]

const SEATS = [ // always Chair-equivalent handled separately; these are the voting seats
  { name: 'Chair / CEO — Strategy & Sequencing', brief: 'You judge coherence and sequencing. You are allergic to pursuing several beachheads at once. You own the speed-vs-rigor trade-off.' },
  { name: 'CFO — Unit Economics & Capital Discipline', brief: 'You judge the money: gates, cash, downside. You kill anything that spends before the decisive number is bought. You demand explicit budgets.' },
  { name: 'Guardian — the Constitution', brief: 'You hold the mission. You challenge every economically-motivated drift from the stated beneficiary. You decide whether reconciliations are genuine or fig leaves. You may formally dissent.' },
]
const DEBATE_ROLES = ['skeptic', 'builder']  // quick scale: ['skeptic']

// COST CONTROL — model & effort tiering. The mechanical stages (leads, debate,
// synthesis) do coverage work and run fine on a cheaper model; judgment compounds only
// at the council and chair, so spend the strong model there. Set MECH_MODEL to null to
// run everything on the session model. This can roughly halve a parallel run's cost.
const MECH_MODEL = 'sonnet'   // leads/debaters/synths; try 'haiku' for simple decisions, null = session model
const JUDGE_MODEL = null      // seats + chair; null = session model (usually the strong one)
const mech = MECH_MODEL ? { model: MECH_MODEL } : {}
const judge = JUDGE_MODEL ? { model: JUDGE_MODEL } : {}

// Optional hard token ceiling. If the user gave a "+Nk" budget, set BUDGET_STOP and the
// run trims later areas when the pool is nearly spent rather than blowing past the cap.
const BUDGET_STOP = budget.total ? budget.total : null   // null = no cap
// END ADAPT --------------------------------------------------------------

const LEAD_SCHEMA = { type: 'object',
  required: ['stance','analysis','agreements','refutations','expansions','recommendations','open_questions'],
  properties: {
    stance: { type: 'string' },
    analysis: { type: 'string', description: '500-800 words, complete sentences' },
    agreements: { type: 'array', items: { type: 'string' } },
    refutations: { type: 'array', items: { type: 'string' } },
    expansions: { type: 'array', items: { type: 'string' } },
    recommendations: { type: 'array', items: { type: 'string' } },
    open_questions: { type: 'array', items: { type: 'string' } } } }

const DEBATE_SCHEMA = { type: 'object', required: ['role','verdict','critique','additions'],
  properties: { role: { type: 'string' }, verdict: { type: 'string' },
    critique: { type: 'string', description: '250-400 words' },
    additions: { type: 'array', items: { type: 'string' } } } }

const SYNTH_SCHEMA = { type: 'object',
  required: ['consensus','disagreements','final_recommendations','council_brief'],
  properties: { consensus: { type: 'string', description: '400-600 words; decide, never average' },
    disagreements: { type: 'string' },
    final_recommendations: { type: 'array', items: { type: 'string' } },
    council_brief: { type: 'string', description: '150-220 words' } } }

const SEAT_SCHEMA = { type: 'object',
  required: ['member','verdict','key_positions','overrides','priorities','dissent'],
  properties: { member: { type: 'string' }, verdict: { type: 'string', description: '300-450 words' },
    key_positions: { type: 'array', items: { type: 'string' } },
    overrides: { type: 'array', items: { type: 'string' } },
    priorities: { type: 'array', items: { type: 'string' } },
    dissent: { type: 'string', description: 'formal dissent with trigger + satisfaction condition, or "none"' } } }

const RULING_SCHEMA = { type: 'object',
  required: ['ruling','area_decisions','kill_criteria','budget','roadmap','dissents_noted','limits'],
  properties: {
    ruling: { type: 'string', description: '800-1200 words: verdict, adjudication of source, reconciliation of the central tension with an enforcement mechanism' },
    area_decisions: { type: 'array', items: { type: 'object', required: ['area','decision'],
      properties: { area: { type: 'string' }, decision: { type: 'string' } } } },
    kill_criteria: { type: 'array', items: { type: 'string' } },
    budget: { type: 'string' }, roadmap: { type: 'string' },
    dissents_noted: { type: 'string' }, limits: { type: 'string' } } }

function leadPrompt(a) {
  return CTX + '\n\n=== YOUR ROLE ===\nYou are the LEAD SPECIALIST for "' + a.title +
  '".\nFIRST ACTION: read the source material in full.\nYOUR FOCUS: ' + a.focus +
  '\n\nTASK: (1) AGREE where the source is right and why; (2) REFUTE what is wrong or ' +
  'under-argued, with reasoning (up to 3 quick checks allowed for decisive facts); ' +
  '(3) EXPAND with options the source missed. Ground everything in the constitution. ' +
  'A skeptic will attack this and a council will judge it — make it rigorous.'
}
function debatePrompt(a, lead, role) {
  const d = role === 'skeptic'
    ? 'You are the SKEPTIC. Attack the lead analysis: weakest claims, hidden assumptions, evidence-discipline violations (refuted claims cited, inference stated as fact). Every attack should imply a fix.'
    : 'You are the BUILDER. Strengthen the lead analysis: overlooked options, sharper more executable recommendations, missed expansions. No invented facts.'
  return CTX + '\n\n=== YOUR ROLE ===\n' + d + '\n\nAREA: "' + a.title + '" — ' + a.focus +
    '\n\nTHE LEAD ANALYSIS:\n' + JSON.stringify(lead, null, 1)
}
function synthPrompt(a, bundle) {
  return CTX + '\n\n=== YOUR ROLE ===\nYou are the AREA SYNTHESIZER for "' + a.title +
  '". Reconcile lead + debate into the area position for the council. Concede real ' +
  'blows, absorb real value, defend what held. DO NOT AVERAGE — decide. Send genuine ' +
  'unresolved conflicts to the council labeled as such.\n\nLEAD:\n' +
  JSON.stringify(bundle.lead, null, 1) + '\n\nDEBATE:\n' + JSON.stringify(bundle.debate, null, 1)
}
function seatPrompt(seat, briefs) {
  return CTX + '\n\n=== YOUR ROLE ===\nYou are a GOVERNANCE COUNCIL member: ' + seat.name +
  '.\n' + seat.brief + '\n\nThe area briefs follow. Judge ALL areas from your seat. ' +
  'File a formal dissent only for decisions you cannot accept — state its trigger and ' +
  'what would satisfy you.\n\n=== AREA BRIEFS ===\n' + briefs
}
function chairPrompt(briefs, seats) {
  return CTX + '\n\n=== YOUR ROLE ===\nYou are the COUNCIL CHAIR issuing the FINAL ' +
  'RULING. Resolve conflicts explicitly — name who was overruled and why; never ' +
  'average. For each dissent: triggered, incorporated (how), or overruled (why) — ' +
  'preserved either way. Adjudicate the source material (upheld/amended/extended), ' +
  'reconcile the central tension with an ENFORCEMENT MECHANISM, and end with a funded, ' +
  'gated plan starting this week.\n\n=== AREA BRIEFS ===\n' + briefs +
  '\n\n=== COUNCIL VERDICTS ===\n' + JSON.stringify(seats, null, 1)
}

// Budget guard: skip an area's deliberation once the pool is nearly spent (keeps the run
// from overshooting a "+Nk" ceiling). Areas already done still produce a valid ruling.
const nearlyBroke = () => BUDGET_STOP && budget.remaining() < BUDGET_STOP * 0.2

log('Convening ' + AREAS.length + ' specialist areas...')
const areaResults = await pipeline(
  AREAS,
  a => nearlyBroke() ? null : agent(leadPrompt(a), { label: 'lead:' + a.key, phase: 'Area Leads', schema: LEAD_SCHEMA, ...mech }),
  async (lead, a) => {
    if (!lead) return null   // budget-skipped or failed; dropped from the ruling, disclosed in Limits
    const debate = await parallel(DEBATE_ROLES.map(r => () =>
      agent(debatePrompt(a, lead, r), { label: r + ':' + a.key, phase: 'Debate', schema: DEBATE_SCHEMA, ...mech })))
    return { lead, debate: debate.filter(Boolean) }
  },
  async (bundle, a) => {
    if (!bundle) return null
    const synth = await agent(synthPrompt(a, bundle), { label: 'synth:' + a.key, phase: 'Area Synthesis', schema: SYNTH_SCHEMA, ...mech })
    return { area: a, ...bundle, synth }
  }
)
const done = areaResults.filter(Boolean).filter(r => r.synth)
log(done.length + '/' + AREAS.length + ' areas completed' + (done.length < AREAS.length ? ' (rest skipped — note in Limits)' : ''))

const briefs = done.map(r => '--- AREA: ' + r.area.title + ' ---\nBRIEF: ' + r.synth.council_brief +
  '\nRECOMMENDATIONS: ' + (r.synth.final_recommendations || []).join(' | ') +
  '\nUNRESOLVED: ' + (r.synth.disagreements || 'none')).join('\n\n')

log('Council convening...')
const seats = (await parallel(SEATS.map(s => () =>
  agent(seatPrompt(s, briefs), { label: 'seat:' + s.name.split(' ')[0], phase: 'Council', schema: SEAT_SCHEMA, ...judge })
))).filter(Boolean)

// The chair is the one stage where reasoning effort earns its cost — it resolves every
// conflict and writes the binding ruling. Keep it on the strong model at high effort.
log('Chair drafting the ruling...')
const ruling = await agent(chairPrompt(briefs, seats),
  { label: 'chair:ruling', phase: 'Final Ruling', schema: RULING_SCHEMA, effort: 'high', ...judge })

return { areas: done, council: seats, ruling }
```

## After the run

Assemble the deliverable from the returned object using `assets/ruling-template.md`:
executive summary (condense `ruling.ruling` without adding claims), Part I the ruling,
Part II seat verdicts with dissents, Part III the full area record, appendix of limits.
If the result is large, read it from the workflow's journal/output file and assemble the
document with a script rather than passing everything through context.

## Mode B (Agent tool, no Workflow)

Same stages and prompts. Batch all leads in one message (parallel), then per-area
debates, then synths, then all seats in one message, then the chair. Barriers between
stages are acceptable here; you lose pipeline overlap but keep independence.
