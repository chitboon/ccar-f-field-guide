# CCAR-F Targeted Drill 3 — Error Handling & Structured Facts: key and rationales

Grade in one deferred pass. For every miss, note which it was: didn't know it,
misread the item, or picked the plausible-but-soft option.

---

**1. A** — A generic status hides the failure type, what was attempted, and any partial results, leaving the coordinator unable to choose an appropriate recovery path. Response length, generation time, and logging compatibility are not the actual problem with this report. *(task 5.3; concept: error_propagation_multiagent; item `t3-01`)*

---

**2. B** — A genuine empty result is a valid outcome, while a dropped connection is an access failure needing a retry decision, so the two deserve different labels. Treating both as errors, suppressing the connection failure, or silently marking both as successes all collapse a distinction the coordinator needs. *(task 5.3; concept: error_propagation_multiagent; item `t3-02`)*

---

**3. C** — Synthesizing the available findings while annotating the unreachable subtopic as a gap preserves both the useful work and honesty about coverage. Abandoning the whole report wastes four good results, silent omission misleads the reader, and retrying everything ignores the four subagents that already succeeded. *(task 5.3; concept: error_propagation_multiagent; item `t3-03`)*

---

**4. A** — Attempting a local retry for a transient failure and only escalating afterward, with details of what was attempted, matches recommended local recovery practice. Returning a false empty result, terminating the whole workflow, and returning an unexplained generic failure all discard information the coordinator needs. *(task 5.3; concept: error_propagation_multiagent; item `t3-04`)*

---

**5. D** — Keeping exact figures in a separate structured block protects them from the imprecision that progressive summarization can introduce into a narrative. Tool availability, subagent disagreement, and clarification frequency are unrelated to this specific practice. *(task 5.1; concept: structured_facts_vs_summarization; item `t3-05`)*

---

**6. A** — This is the lost-in-the-middle effect, where models reliably process the start and end of long inputs but can miss content buried in between. Load failure, submission blocking, and extra charges for middle placement are not real consequences of this position. *(task 5.1; concept: structured_facts_vs_summarization; item `t3-06`)*

---

**7. A** — Placing key findings early directly counters the tendency to miss content buried in a long middle section. Randomizing order adds no benefit, organizing under headers helps with structure but is secondary to placement, and compressing everything into one paragraph removes the structure that helps. *(task 5.1; concept: structured_facts_vs_summarization; item `t3-07`)*

---

**8. B** — Including the original document, the failed output, and the specific validation error lets the model target the actual mistake on retry. An unrelated document, a vague instruction, or lowering the confidence threshold do not address the specific error found. *(task 4.4; concept: retry_validation; item `t3-08`)*

---

**9. C** — Retries are ineffective when the needed information is genuinely absent from the source, since no amount of reprocessing can extract what is not there. A stricter schema, more retries, or a different model cannot recover information the document never contained. *(task 4.4; concept: retry_validation; item `t3-09`)*

---

**10. D** — Stratified sampling by document type and field can surface accuracy problems concentrated in specific segments that an aggregate figure conceals. Rechecking the same overall number, comparing to another team, or asking the model to self-assess do not examine segment-level performance. *(task 5.5; concept: aggregate_vs_stratified; item `t3-10`)*

---
