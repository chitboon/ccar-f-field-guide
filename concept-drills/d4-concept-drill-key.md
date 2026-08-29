# CCAR-F Concept Drill — Domain 4: Prompt Engineering & Structured Output: key and rationales

Grade in one deferred pass. For every miss, note which it was: didn't know it,
misread the item, or picked the plausible-but-soft option.

---

**1. D** — Strict schemas via tool use eliminate JSON syntax errors but say nothing about whether values are mutually consistent. Semantic errors — items not summing, values in wrong fields — require a validation layer, such as comparing a calculated total against the stated total. *(task 4.3; concept: syntax_vs_semantic_validity; item `d4-04`)*

---

**2. B** — Categorical inclusion and exclusion boundaries define the decision the model must make; 'be conservative' does not — the model cannot operationalize it. Self-rated confidence is poorly calibrated, stronger adverbs change nothing, and a bigger model inherits the same undefined criteria. *(task 4.1; concept: inclusion_exclusion_boundaries; item `d4-01`)*

---

**3. C** — Batch processing trades latency for cost: roughly half price, with a processing window of up to 24 hours and no latency guarantee. That fits latency-tolerant, non-blocking work like overnight reports, and disqualifies anything a human or a pipeline is actively waiting on. *(task 4.5; concept: batch_vs_sync_split; item `d4-10`)*

---

**4. A** — Tool use with a forced tool_choice makes structured output the only legal continuation and validates it against the schema. Prefilling the assistant message with '{' forces the completion to stay in JSON but guarantees nothing about the fields. Prompt-only instructions are a request, not a constraint. *(task 4.3; concept: prefill_on_strictness_spectrum; item `d4-05`)*

---

**5. D** — A high-false-positive category poisons trust in every other category the tool produces; removing the noisy category is the direct repair while its criteria are reworked. Relabeling severity, polishing the accurate category, and lowering cadence all leave the noisy category running. *(task 4.1; concept: category_trust_repair; item `d4-02`)*

---

**6. C** — Retry-with-error-feedback guides the model toward correcting malformed or misplaced output; it cannot conjure data the document does not contain. The correct design is a nullable field so the model can return null rather than fabricate a date. *(task 4.4; concept: retry_vs_absent_information; item `d4-08`)*

---

**7. B** — Few-shot examples are the most effective technique for consistent formatting on varied inputs: they demonstrate ambiguous-case handling that prose cannot fully enumerate. Temperature reduces run-to-run noise but not layout confusion; retry loops repair failures instead of preventing them. *(task 4.2; concept: few_shot_format_stability; item `d4-03`)*

---

**8. A** — A required field the document cannot satisfy forces the model to choose between failing validation and inventing data — and it will invent. Nullable fields give it an honest way out. Prompt pleas, looser types, and retries all leave the structural pressure in place. *(task 4.3; concept: schema_honesty_nullable; item `d4-06`)*

---

**9. D** — Self-review fails because the model's own reasoning is still in context, anchoring it to its earlier decisions. A second instance sees only the artifact, not the justification. Context size, thinking modes, and temperature are not the mechanism. *(task 4.6; concept: independent_instance_review; item `d4-12`)*

---

**10. B** — An extensible enum — 'other' plus a detail string, and 'unclear' for ambiguity — keeps routing honest by giving non-conforming documents a truthful bucket. Removing the enum loses the categories entirely, and multiplying fixed categories just moves the boundary problem. *(task 4.3; concept: enum_other_detail_pattern; item `d4-07`)*

---

**11. A** — custom_id exists to correlate batch requests with their results, making targeted resubmission cheap and safe. Full resubmission doubles cost, failures are not positionally clustered, and raising max_tokens is a guess about the failure cause. *(task 4.5; concept: custom_id_resubmission; item `d4-11`)*

---

**12. C** — Without a structured field naming the triggering construct, dismissal analysis is anecdote collection; with it, false-positive patterns become countable and fixable. Longer prose, severity guesses, and expiry do not connect dismissals back to prompt causes. *(task 4.4; concept: detected_pattern_feedback; item `d4-09`)*

---
