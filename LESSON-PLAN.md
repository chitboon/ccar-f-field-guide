# CCAR-F Lesson Plan

A structured sequence for preparing for the Claude Certified Architect –
Foundations exam. Designed for someone who already holds the CCAO-F cert,
has hands-on Claude Code experience, and has skimmed Academy material.

## The sequence

### Phase 1: Orientation (30 min)

1. Read `guide/00-overview.md` — understand the blueprint, the tier structure,
   and the one mental habit (deterministic beats probabilistic).
2. Read `practice/detailed-cheatsheet.md` — the golden rules in Section 1
   answer 70% of questions. Sections 2–6 are per-domain deep dives.

### Phase 2: Concept drills (1–2 hours)

Sit all five concept drills in order. These are direct, no-scenario items —
a fast recall check, not an exam simulator.

1. `concept-drills/d1-concept-drill.md` — 12 items, ~8 min
2. `concept-drills/d2-concept-drill.md` — 12 items, ~8 min
3. `concept-drills/d3-concept-drill.md` — 12 items, ~8 min
4. `concept-drills/d4-concept-drill.md` — 12 items, ~8 min
5. `concept-drills/d5-concept-drill.md` — 12 items, ~8 min

After each drill: grade against the key, read every rationale (including
the ones you got right). Note any items where the answer felt obvious rather
than reasoned — those are recognition-only and may mask gaps.

**Target:** 90%+ on each concept drill before moving to Phase 3. If a domain
is below 80%, re-read the corresponding `guide/dN-*.md` page and re-sit.

### Phase 3: Domain drills (2–3 hours)

Sit the domain drills for domains where concept-drill scores were below 90%,
or where you want scenario-depth practice. These are Olivier-density
scenario stems matching the real exam's register.

1. `practice/domain-drills/d1-domain-drill.md` — 10 items, ~12 min
2. `practice/domain-drills/d2-domain-drill.md` — 10 items, ~12 min
3. `practice/domain-drills/d3-domain-drill.md` — 10 items, ~12 min
4. `practice/domain-drills/d4-domain-drill.md` — 10 items, ~12 min
5. `practice/domain-drills/d5-domain-drill.md` — 10 items, ~12 min

After each drill: grade, read rationales, note misses by type:
- **Didn't know it** → re-read the guide page
- **Misread the item** → slow down on scenario stems
- **Picked the plausible-but-soft option** → review `guide/trap-patterns.md`

### Phase 4: Targeted drills (1 hour)

Sit targeted drills for the weak spots identified in Phase 3.

1. `practice/targeted-drills/targeted-drill-1.md` — CI & Isolation
2. `practice/targeted-drills/targeted-drill-2.md` — Codebase Exploration
3. `practice/targeted-drills/targeted-drill-3.md` — Error Handling & Structured Facts
4. `practice/targeted-drills/targeted-drill-4.md` — Mixed Weak Spots

Sit only the drills that cover your miss clusters. Skip drills where your
domain-drill scores were already high.

### Phase 5: Mock exam (2 hours)

Sit the full-length mock under timed conditions (120 minutes).

`practice/mocks/ccar-f-mock.md` — 60 items, matching the real exam's
blueprint weights (D1=16, D2=11, D3=12, D4=12, D5=9).

After the mock: grade, read all rationales, and compare your score to the
720/1000 passing threshold. If below 720, return to Phase 3 for the weakest
domains.

### Phase 6: Final review (30 min before exam)

1. `practice/exam-day-cram.md` — the 10-minute refresher
2. `guide/trap-patterns.md` — the 14 trap patterns
3. `practice/detailed-cheatsheet.md` Section 1 — the golden rules

## Calibration note

A correct multiple-choice answer is not proof of mechanism understanding.
When reviewing rationales, self-report honestly: did the correct answer feel
obvious (recognition), or did you reason through the why (mechanism)? Items
where you picked the right answer for the wrong reason are soft spots the
exam can expose with a different scenario framing.

## Hands-on reps

Some objectives benefit from actual tool use, not just written drills:

- **Glob vs Grep** — run `grep -r "functionName(" .` in a real repo and
  enumerate call sites. Then try `glob **/*.ts` and see that it matches
  file paths, not file contents. The distinction transfers directly to
  exam items about codebase exploration.
- **Plan mode vs direct execution** — try both in a real Claude Code session.
  Plan mode for a multi-file refactor; direct execution for a one-line fix.
  The felt difference cements the selection criteria.
- **`--print` flag** — run `claude -p "explain this code"` in a terminal and
  see the headless output. Compare to the interactive session experience.
