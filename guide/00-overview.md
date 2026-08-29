# CCAR-F Overview — Blueprint, Tiers, and How to Use This Guide

**Claude Certified Architect – Foundations** · 60 items · 120 minutes ·
720/1000 to pass · proctored (online or test centre) · $125 · 12-month
validity.

Every scored item is **single-answer: four options, one correct, no
multi-select**. Distractors are closely spaced — usually the plausible
prompt-level or over-engineered alternative to the correct structural
mechanism. Stems are long and scenario-based, typically three to five
sentences with concrete details.

## The blueprint

| Domain | Weight | Items |
|---|---|---|
| 1 · Agentic Architecture & Orchestration | 27% | 16 |
| 2 · Tool Design & MCP Integration | 18% | 11 |
| 3 · Claude Code Configuration & Workflows | 20% | 12 |
| 4 · Prompt Engineering & Structured Output | 20% | 12 |
| 5 · Context Management & Reliability | 15% | 9 |

D1 + D3 + D4 are two-thirds of the exam. Do not let D5's small weight fool
you — its nine items concentrate on a few heavily-tested judgements
(escalation triggers, provenance, lost-in-the-middle), so each one is cheap
to secure.

## The practice tiers

This repo's drills are deliberately tiered by register:

1. **Concept drills** (`concept-drills/`) — direct, no-scenario items, 12 per
   domain, sat in under ten minutes. A fast recall check: do you know the
   mechanism well enough to state it cold? They are intentionally simpler
   than the real exam.
2. **Domain drills and mocks** (`practice/`) — scenario-dense, three-to-five-
   sentence stems with concrete artifacts (a number, an error payload, a
   named tool), matching the real exam's register.

The sequence per domain: concept drill first, then the scenario drill, then a
targeted drill wherever your misses cluster. A gap between "concept drill
feels easy" and "mock feels hard" is real signal — it means you recognize
mechanisms but cannot yet *select* them under scenario pressure, which is
what the exam actually grades.

## The one mental habit that answers most items

When torn between two options, ask: **which one makes the failure impossible
rather than less likely?** Deterministic beats probabilistic throughout this
exam — hooks and gates beat prompt instructions, schemas beat "please return
valid JSON", pinned fact blocks beat better summarization. The plausible-but-
softer option is the distractor's home address.

## Where to go next

- Per-domain refreshers: `guide/d1-…` through `guide/d5-…`
- The judgement traps that cost points: `guide/trap-patterns.md`
- The condensed reference: `practice/detailed-cheatsheet.md`
- Ten minutes before the exam: `practice/exam-day-cram.md`
