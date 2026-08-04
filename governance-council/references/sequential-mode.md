# Mode C — Sequential single-context mode (any AI system)

No subagents, no orchestration — one model runs every role as sequential passes. This is
the portability mode: it works in claude.ai, ChatGPT, Gemini, a local model, or a Claude
Code session that shouldn't spend on parallel agents. It trades true independence for
availability, so the discipline below exists to protect adversarial quality anyway.

## The core problem and its mitigations

One context means the skeptic has read the lead's reasoning *sympathetically* — it knows
why every claim felt right when written. Three mitigations:

1. **Hard role switches.** Each pass begins with the full role prompt (from
   `prompts.md`), including the sentence "disregard any attachment to positions written
   earlier in this conversation; your mandate is only this role's." Write each role's
   output in full before starting the next — no interleaving.
2. **Attack quotas for the skeptic.** In single-context mode the skeptic MUST produce at
   least 3 substantive attacks per area, each implying a fix. Quotas are crude, but they
   defeat the strongest failure mode of this mode: a skeptic pass that politely
   paraphrases the lead. (In multi-agent modes quotas are unnecessary — independence
   does the work.)
3. **Handoff files between stages** (strongest form): write each stage's output to a
   self-contained markdown artifact and start a FRESH session for the next stage, giving
   it only the charter + the artifact. A fresh session genuinely hasn't seen the lead's
   sympathetic reasoning. Use this whenever sessions are cheap (claude.ai projects,
   multiple chats) or the deliberation is consequential. The stable RETURN headings in
   `prompts.md` §7 exist so artifacts parse cleanly across sessions and even across
   different AI systems (lead pass in one tool, skeptic pass in another — cross-model
   skepticism is stronger than same-model).

## Run order

```
Pass 0   Write the charter (confirm with user)
For each area (run areas one at a time, smallest scale that fits the context window):
  Pass 1   Lead      — full seven-part output
  Pass 2   Skeptic   — verdict + critique + additions (quota: ≥3 attacks)
  Pass 3   Builder   — (standard/full scale only)
  Pass 4   Synthesis — decide, never average; emit the council_brief
Then:
  Pass 5   Each seat, one at a time, given ONLY the charter + all council_briefs
           (do not let a seat read another seat's verdict — in single context, this
           means writing all seat verdicts before re-reading any of them)
  Pass 6   Chair — full ruling per assets/ruling-template.md
```

## Context budget management

A full-scale sequential run can exceed the context window. In order of preference:
- Drop to quick scale (3 areas, skeptic only, 3 seats).
- Use handoff files + fresh sessions per stage (removes the ceiling entirely).
- Compress carried-forward material: once an area's synthesis exists, its lead and
  debate passes are no longer needed in context — only the brief travels forward.
  Never compress the charter; it rides in every pass.

## Fidelity ladder (be honest about which rung you're on)

1. Parallel independent agents (Modes A/B) — real independence.
2. Fresh-session handoff files — near-real independence, works anywhere.
3. Single context with role switches + quotas — useful, weakest; disclose in the
   ruling's Limits section: "run in sequential single-context mode; adversarial
   independence is procedural, not structural."

One hard rule at every rung: **never claim independence that didn't happen.** An
untooled model reviewing its own analysis will drift into writing "each seat reviewed
the proposal independently" — that sentence is only true on rungs 1–2. On rung 3 the
truthful sentence is the fidelity note. A ruling that misdescribes its own process has
already failed its evidence discipline on the one fact it fully controls.

Whichever rung, the output contract is identical: the ruling document with verdict,
per-area decisions, kill criteria, budget, roadmap, preserved dissents, and limits.
