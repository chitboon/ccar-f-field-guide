# CCAR-F Targeted Drill 2 — Codebase Exploration: key and rationales

Grade in one deferred pass. For every miss, note which it was: didn't know it,
misread the item, or picked the plausible-but-soft option.

---

**1. A** — Searching for entry points and existing patterns lets Claude ground its plan in how the codebase actually works. Asking the user first skips work the model can do itself, assuming generic conventions ignores this codebase's own choices, and drafting blind risks rework. *(task 5.4; concept: codebase_exploration; item `t2-01`)*

---

**2. B** — Existing callers of the helper show exactly how it is meant to be used, which is faster and more reliable than asking or guessing. Importing an unrelated pattern risks mismatch, and deferring pagination entirely avoids the actual task. *(task 5.4; concept: codebase_exploration; item `t2-02`)*

---

**3. C** — Other routes already using the middleware are working examples of correct configuration, making them the fastest reliable reference. Guessing risks a broken or insecure route, asking first skips available self-service, and disabling authentication introduces a security gap. *(task 5.4; concept: codebase_exploration; item `t2-03`)*

---

**4. D** — Reading nearby existing tests reveals the actual convention directly and quickly. Asking the user is slower than self-service search, a generic style may clash with the project, and skipping the test does not fulfil the task. *(task 5.4; concept: codebase_exploration; item `t2-04`)*

---

**5. A** — Tracing actual runtime usage reveals which path governs the bug's behavior without guessing or over-relying on the user. Assuming recency is unreliable, asking first skips traceable evidence already in the code, and unifying the loaders before understanding them risks a much larger unintended change. *(task 5.4; concept: codebase_exploration; item `t2-05`)*

---

**6. B** — A structured facts block preserves exact values for precise reuse without re-parsing prose. Folding details into a summary risks losing precision, relying on implicit recall is unreliable, and stating facts only at the end misses the steps that need them earlier. *(task 5.1; concept: structured_facts_vs_summarization; item `t2-06`)*

---

**7. A** — This is the lost-in-the-middle effect, where models reliably process the start and end of long inputs but can miss content buried in between. Load failure, submission blocking, and extra charges for middle placement are not real consequences of this position. *(task 5.1; concept: structured_facts_vs_summarization; item `t2-07`)*

---

**8. D** — Reporting what failed, what was attempted, and the partial results already gathered lets the coordinator make an informed recovery decision. An empty result discards useful partial work, a generic message hides the cause, and silence forces the coordinator to guess. *(task 5.3; concept: error_propagation_multiagent; item `t2-08`)*

---

**9. A** — A valid empty result and an access failure carry different meanings and call for different responses, so distinguishing them preserves useful information. Treating them identically, discarding a genuine empty result, or retrying both the same way all lose that distinction. *(task 5.3; concept: error_propagation_multiagent; item `t2-09`)*

---

**10. B** — An aggregate figure can mask uneven performance across segments, so checking rates by file type and module reveals whether specific areas are actually weaker. Stability over time, matching a prior report, or comparing to another tool do not surface hidden segment-level variation. *(task 5.5; concept: aggregate_vs_stratified; item `t2-10`)*

---
