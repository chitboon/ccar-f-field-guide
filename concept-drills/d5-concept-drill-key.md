# CCAR-F Concept Drill — Domain 5: Context Management & Reliability: key and rationales

Grade in one deferred pass. For every miss, note which it was: didn't know it,
misread the item, or picked the plausible-but-soft option.

---

**1. B** — A verbatim case-facts block survives summarization because it travels outside the summarized history. Longer narratives are compressed harder; window limits and memorization do not structurally protect facts. *(task 5.1; concept: structured_facts_vs_summarization; item `cc-08`)*

---

**2. A** — An explicit request for a human is an unconditional escalation trigger; attempting resolution first overrides the customer's stated preference to save a trivial case. Sentiment screening and negotiation both delay what was directly asked for. *(task 5.2; concept: honor_explicit_human_request; item `d5-n1`)*

---

**3. D** — Lost-in-the-middle attention degrades recall for information positioned mid-context, while beginnings and ends are processed reliably. Length is not the issue, the model can read numbering, and the finding was present in the sources. *(task 5.1; concept: lost_in_the_middle; item `cc-09`)*

---

**4. C** — A near-perfect aggregate can conceal a segment that fails every single time; only accuracy broken down by document type and field exposes it. The aggregate may be computed correctly, and training data or thresholds are speculative secondary causes. *(task 5.5; concept: aggregate_vs_stratified; item `cc-10`)*

---

**5. A** — The coordinator can only make an intelligent recovery — retry, reroute, or proceed with annotated gaps — if it receives the failure type, what was attempted, and the partial results. Empty-as-success suppresses the failure, a generic string hides it, and a fatal exception discards recoverable work. *(task 5.3; concept: structured_error_propagation; item `d5-n3`)*

---

**6. C** — Multi-source synthesis must preserve attribution and conflict, especially for a filing. Selecting the earliest date, discarding the uncertain source, and banishing attribution to an appendix all erase provenance that the reader needs. *(task 5.6; concept: claim_source_provenance; item `cc-19`)*

---

**7. B** — Structured, schema-validated fact records resist the compression that narrative summaries invite, and carrying them verbatim keeps exact values intact. Longer narratives compress worse, page citations do not preserve values, and token limits do not fix attention loss. (Single-answer conversion of the two-pattern original.) *(task 5.6; concept: structured_facts_vs_summarization; item `cc-17s`)*

---

**8. D** — Lost-in-the-middle attention degrades recall for the middle of a long sequence while the beginning and end stay reliable — exactly the observed pattern. There is no evidence of length effects, hard document limits, or intentional skipping. *(task 5.1; concept: lost_in_the_middle; item `cc-18`)*

---

**9. C** — The symptom is context degradation in an extended session: early findings fall out of effective context. A scratchpad file persists those findings across the boundary and can be reloaded on demand. Restarts discard the work, larger windows only delay the effect, and temperature is unrelated. *(task 5.4; concept: scratchpad_persistence; item `d5-n4`)*

---

**10. A** — Escalation triggers include policy gaps, not just angry customers or complex mechanics: where policy is silent, the agent should not improvise an answer. Stretching the nearest policy and sentiment-based routing are both misreads; the customer is not responsible for quoting policy. *(task 5.2; concept: policy_gap_vs_sentiment; item `d5-n2`)*

---

**11. D** — Stratified reporting by document type, field, and confidence bucket exposes a failing segment that the 96% aggregate conceals, and enables targeted review routing. A bigger aggregate sharpens the wrong number; universal review wastes capacity; a uniform threshold assumes the segment mix is homogeneous. (Single-answer conversion of the three-practice original.) *(task 5.5; concept: aggregate_vs_stratified; item `cc-20s`)*

---

**12. B** — Structured state exports plus a coordinator-loaded manifest make recovery deterministic: every agent's findings survive the process and are re-injected on resume. Scrollback is unstructured and fragile, longer timeouts treat the symptom, and hourly forks capture conversation state, not per-agent working state. *(task 5.4; concept: manifest_state_recovery; item `d5-n5`)*

---
