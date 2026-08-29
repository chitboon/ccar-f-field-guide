# CCAR-F Targeted Drill 1 — CI & Isolation: key and rationales

Grade in one deferred pass. For every miss, note which it was: didn't know it,
misread the item, or picked the plausible-but-soft option.

---

**1. A** — Plan mode suits large-scale changes with multiple valid architectural approaches, letting the team weigh options before committing. The other three describe small, well-scoped, already-understood changes where direct execution is faster and safer. *(task 3.4; concept: plan_mode; item `t1-01`)*

---

**2. B** — A single, well-scoped change with an obvious fix does not need plan mode's exploration overhead. Plan mode, a repository-wide search, and forcing a strategy choice are all disproportionate to a change this small and clear. *(task 3.4; concept: plan_mode; item `t1-02`)*

---

**3. C** — The fresh container and shared CLAUDE.md govern environment isolation and reproducibility, a separate concern from which commit is compared. The main branch comparison is the code-state axis, and the remaining options misdescribe the two settings entirely. *(task 3.6; concept: ci_isolation; item `t1-03`)*

---

**4. D** — Local inconsistency comes from personal configuration layering on top of the shared project setup, not from which branch is compared. Branch comparison is a code-state setting and would not remove that personal layer, so the proposed fix targets the wrong axis. *(task 3.6; concept: ci_isolation; item `t1-04`)*

---

**5. A** — Batch API suits latency-tolerant large jobs such as scanning millions of files. Synchronous API, a single oversized request, and an interactive session all pay unnecessary synchronous overhead for a task this latency-tolerant. *(task 4.5; concept: batch_vs_sync; item `t1-05`)*

---

**6. B** — Independent review catches subtle defects that self-review misses because the generator's own reasoning biases confirmation. Option A repeats the same bias, option C removes review, and option D changes generation randomness rather than improving verification. *(task 4.6; concept: self_review_vs_independent; item `t1-06`)*

---

**7. A** — Built-in Grep covers the full repository with pattern matching, which is exactly what finding every call site requires. The custom script's partial index misses the directories outside `/src/services/`. The blanket 'never' and the file-size or text-only restrictions aren't real limitations of the built-in tool. *(task 2.5; concept: builtin_tool_selection; item `t1-07`)*

---

**8. C** — A pre-call hook gated on an unforgeable approval flag enforces the rule deterministically. More examples and temperature zero are still probabilistic; post-call reversal is remediation, not prevention. *(task 1.4; concept: programmatic_gate_vs_prompt; item `t1-08`)*

---

**9. C** — Targeted search for entry points and structure gives fast orientation without wasting effort. Reading everything is inefficient at scale, deferring entirely to the user skips work the model can do itself, and modifying before understanding risks breaking something unseen. *(task 5.4; concept: codebase_exploration; item `t1-09`)*

---

**10. D** — Existing examples reveal the local convention directly and fastest. Importing a foreign standard pattern risks a mismatch, deferring everything to the user is slower than it needs to be, and rewriting the framework is a disproportionate and risky first move. *(task 5.4; concept: codebase_exploration; item `t1-10`)*

---
