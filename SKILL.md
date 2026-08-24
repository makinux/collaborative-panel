---
name: collaborative-panel
description: >-
  Run a multi-model collaborative brainstorm panel (協調的パネル) — the cooperative
  sibling of adversarial-panel. 2+ panelists (different model families where
  possible) independently GENERATE ideas under different method lenses, then
  cross-pollinate (build on / combine / rescue / find white space), and the
  facilitator synthesizes a ranked, concretized shortlist with cheapest-next-tests
  while preserving wildcards. Use whenever the user wants to EXPAND, ENRICH, or
  ADD TO an idea, proposal, plan, or design rather than attack it — triggers:
  協調的パネル, ブレスト, ブレインストーミング, アイディア出し, アイディアを膨らませて,
  発散, 共創, 加算事項, 追加アイディア, 拡張案, "brainstorm with multiple models",
  "co-develop", "build on this", "what else could we add", "make this richer".
  Also use proactively when the user asks for additions/enrichment to a document
  or proposal that already exists. For attacking, validating, or red-teaming
  claims use adversarial-panel instead; the two compose (diverge here → harden
  there).
---

# Collaborative Panel

Reproduce the divergent half of group creativity explicitly: several models
generate ideas independently, enrich each other's ideas, and a facilitator
(you, the main session) converges the result into a usable shortlist. The
benefit does NOT come from "asking several models for ideas" as such — it
comes from three design properties you must engineer deliberately:

- **Decorrelated divergence.** Two models given the same open prompt produce
  heavily overlapping idea sets — the obvious first five. Independence
  (blind Round 1) plus *different method lenses* per panelist is what buys
  coverage of the idea space. Anchoring is the collaborative analog of the
  adversarial panel's shared blind spot: a panelist who has seen another's
  ideas produces variations, not new regions.
- **Combination surplus.** The ideas neither panelist would produce alone
  come from crossing one's Round-1 ideas with the other's. This only happens
  if Round 2 *requires* builds and combinations — left to themselves, models
  emit praise and paraphrase.
- **Convergence without mush.** Forty raw ideas are not a deliverable, but
  blending them into vague themes destroys them. The facilitator selects and
  concretizes while keeping ideas atomic, attributed, and testable — and
  keeps the weird ones alive in a separate lane instead of sanding them off.

Protect three invariants throughout: **independence** (Round-1 generation is
blind), **additivity** (every Round-2 contribution must add mechanism, scale,
pairing, or application — agreement is fine here, *empty* agreement is not; a
build that adds nothing is praise padding and counts as a failed
contribution), and **hypothesis labeling** (brainstorm output is unvetted by
design; every idea ships with its cheapest verification step, never dressed
as a validated claim).

Conduct everything in the user's language.

## Roles

- **Facilitator** — you, the main session. Frame, assign lenses, dispatch,
  validate, deduplicate, cluster, rank, concretize. Your own ideas are
  allowed (unlike adversarial adjudication) but label them as the
  facilitator's and never let them crowd the panelists' lanes.
- **Panelists** — 2–4 generators. Maximize heterogeneity in the same order
  as adversarial-panel: (1) different model families via external CLI
  (check availability first), (2) different Claude models via the Agent
  tool `model` parameter, (3) same model with forced-divergent method
  lenses. Reuse adversarial-panel's operational mechanics for external
  CLIs: foreground execution with generous timeout, prompts piped via
  stdin from files, `--output-last-message` capture, and a validation gate
  on every return (a status line or error dump is not a contribution —
  re-run, then degrade a tier and disclose).

**Method lenses** (assign 1–2 per panelist, all different): analogy
transfer (import working precedents from other domains/regions/industries);
inversion (turn the strongest weaknesses into products); stakeholder
rotation (generate from each actor's self-interest in turn); asset
recombination (cross existing local/available assets pairwise); constraint
shifting (what becomes possible if constraint X were free, doubled,
forbidden); scale shifting (same idea at 1/10th and 10x). Lenses are the
decorrelation mechanism — without them, heterogeneous weights still produce
the same obvious ideas.

Default: 2 panelists × 2 rounds + facilitator synthesis. Add Round 3 and/or
panelists only for high-stakes or explicitly thorough requests.

## Protocol

### Round 0 — Frame (facilitator, no agents)

Write a self-contained brief: the seed (document, idea, or question), the
goal ("what kind of additions are wanted"), evaluation axes the synthesis
will use (default: novelty × site/context fit × cost-to-test × impact),
hard constraints, and the **idea format**. A fixed format keeps ideas
atomic, comparable, and dedupable:

```
【一行ピッチ】 one line
【メカニズム】 how it actually works
【誰が払う/誰が得る】 payer and beneficiary
【なぜここか】 why THIS site/context and not anywhere else
```

Two brief rules, both load-bearing:

- **Quantity floor.** Require N ≥ 12 ideas per panelist, and require each to
  self-mark a **wildcard** (most speculative) and a **sleeper** (most
  underrated). The floor forces generation past the obvious first five; the
  self-marks protect the tails from being self-censored.
- **Dead-claims list.** If a prior adversarial panel (or other review) has
  killed claims in this space, enumerate them in the brief and forbid
  resurrecting them *as stated* — while explicitly allowing ideas that
  attack the killed claim's premise via a genuinely new mechanism. Without
  this list the brainstorm re-derives refuted material; with it stated too
  broadly, it forbids the best ideas.

Guard against **seed leakage**: give panelists the seed material and the
goal, not a list of the answer categories you expect. An enumerated category
list turns generative panelists into form-fillers, and part of any
"coverage" you get back is your own template echoed.

### Round 1 — Independent divergence (parallel, blind)

Launch all panelists in parallel: brief + their lens assignment + "your
final message is the deliverable; no preamble." No panelist sees another's
output. Vary the brief where the seed allows (different lens, different
stakeholder emphasis, reordered seed sections) — identical framing
correlates generators exactly like it correlates critics.

**Validation gate:** on return, check each output is a substantive idea set
in the required format, meets the floor, and actually uses its assigned
lens. A panelist whose ideas ignore the lens has collapsed into the default
idea space — re-run with the lens made explicit in the first line.

### Round 1.5 — Facilitator scan (lightweight)

Before Round 2, scan both sets: note near-duplicates (to seed the merge
map), spot any idea resting on a factual error (flag it neutrally in the
Round-2 prompt — a factual landmine left uncorrected propagates through
every build on top of it), and pick 2–3 ideas per panelist worth forcing
the OTHER panelist to engage with (the ones most orthogonal to that
panelist's lens). This is a scan, not the adversarial fact-check — minutes,
not a research pass.

### Round 2 — Cross-pollination (parallel)

Give each panelist the other's Round-1 set (and your flags). Require, with
counts:

- **BUILD (≥5):** take another's idea and add something — a mechanism, a
  concrete first customer, a pairing with your own idea, an upgrade path, a
  cheaper variant. State what you added. Praise, restatement, and "I agree,
  this is strong" are not builds.
- **COMBINE (≥2):** fuse one of your ideas with one of theirs into a third
  idea that neither is alone. Name both parents.
- **RESCUE (≥1):** take the idea from the union you consider weakest and
  find the version of it that works (smaller, different payer, different
  site-use). Rescue is the only permitted form of criticism — demolition is
  out of scope here; chain into adversarial-panel if the user wants attack.
- **WHITE SPACE (≥2):** name regions of the idea space the union still
  misses, and put at least one concrete idea in each.
- **SELECT:** end with your top 5 from the whole union (own + other's +
  new), one line of reasoning each, and **the cheapest next test** for each
  (who to call, what to look up, what a first experiment costs).

### Round 3 — Concretization sprint (optional)

Only for high-stakes runs or on request: facilitator picks the shortlist,
each panelist drafts a one-pager per assigned idea (what it looks like
concretely / who pays / first test and its cost / main risk / what kills
it). With 2 panelists this is usually better folded into the synthesis.

### Synthesis (facilitator)

Deduplicate with a merge map (credit both originators), cluster, and rank
the shortlist on the Round-0 axes. Then produce:

```
## 採用候補 (Ranked shortlist)
Top N ideas, each: pitch / mechanism / why-here / cheapest next test /
build-chain credit (who seeded it, who added what — the chain is evidence
of combination surplus, and it tells the user which single-model ideas to
discount).

## ワイルドカード (Wildcards — unranked, preserved)
The 2–4 speculative ideas worth keeping alive, with what would have to be
true for each.

## 空白領域 (White space)
What the panel could not fill — named gaps are deliverables too.

## 次の一手 (Cheapest tests)
The 3–5 verification actions that unlock the most ideas at once.
```

Label the entire output as hypotheses. Do not attach confidence
percentages to ideas (calibration theater on unvetted material); attach
verification steps instead. If the user wants the shortlist hardened,
offer to chain it into adversarial-panel — diverge → converge → attack is
the intended composition of the two skills.

Close by naming the **combination surplus**: which shortlist ideas exist
only because of cross-pollination (traceable to a BUILD/COMBINE/RESCUE, not
to any Round-1 set). If none do, say so — it means the panel ran as two
parallel monologues and the user should know the cross round added nothing.

## Degradation modes

- **No subagent tool**: sequential inline sections, strictly separated;
  write each Round-1 section without re-reading the other. Disclose the
  weaker independence.
- **Only one model family**: lens divergence becomes the only
  decorrelation — assign 2 lenses per panelist and disclose.
- **External CLI fails twice**: drop to next tier, continue, disclose.
- **Long prompts**: pipe via stdin from files (`codex exec - < prompt.md`),
  never as CLI arguments.

## Anti-patterns (engineer against all of these)

- **Anchoring leak** — any panelist seeing another's ideas before finishing
  Round 1. The blind round is the whole point.
- **Obvious-first-five** — no quantity floor, no lenses → both panelists
  return the same five ideas anyone would produce. Convergence here is
  redundancy, not validation.
- **Praise padding** — Round-2 "builds" that restate or compliment. Count
  only contributions that add something nameable.
- **Feasibility theater** — presenting brainstorm output as vetted. Every
  idea ships as a hypothesis with a cheapest test.
- **Convergent mush** — synthesis that blends ideas into themes ("leverage
  synergies around cold climate") instead of keeping them atomic and
  actionable.
- **Seed leakage** — the facilitator's expected-answer categories in the
  brief, echoed back as false coverage.
- **Killed-claim resurrection** — re-proposing what a prior review already
  refuted, in its refuted form.
- **Novelty worship** — ranking by surprise alone; an idea that ignores the
  site/context fit axis is a different proposal, not a good idea for this
  one.
