# Changelog

## v1.0.0 — 2026-08-30

Initial release. Candidate passed CCAR-F at 892/1000 (cut 720) on 2026-08-29.

### Guide

- `guide/00-overview.md` — blueprint, tier structure, mental habit
- `guide/d1-*.md` through `guide/d5-*.md` — five domain pages
- `guide/trap-patterns.md` — 14 trap patterns (7 seed slugs + 7 gap patterns)

### Practice material

- `practice/detailed-cheatsheet.md` — fully rewritten reader-facing, ~4,400 words
- `practice/exam-day-cram.md` — 10-minute pre-exam refresher

### Drills

- `concept-drills/d1…d5-concept-drill.md` + keys — 12 items/domain, 60 total
- `practice/domain-drills/d1…d5-domain-drill.md` + keys — 10 items/domain, 50 total
- `practice/targeted-drills/targeted-drill-1…4.md` + keys — 10 items/drill, 40 total
- `practice/mocks/ccar-f-mock.md` + key — 60 items, full-length mock

### Study guides

- `LESSON-PLAN.md` — 6-phase preparation sequence
- `HOW-TO-PRACTISE.md` — drill loop, concrete-artifact rule, calibration, hands-on reps

### Tools

- `tools/check-item-quality.py` — item quality gate (length balance, position balance, punctuation tell)
- `tools/check-nearclone.py` — near-clone detection (requires private corpus)
- `tools/check-provenance.py` — provenance gate (requires private corpus)

### Quality gates

All 210 items across 9 banks + 1 mock pass:
- Quality gate: correct-is-longest 20–33%, length-rank max ≤40%, ratio 0.90–1.10
- Near-clone gate: max similarity 0.596 (threshold 0.6)
- Provenance gate: 0 runs of 12+ consecutive words from external sources
