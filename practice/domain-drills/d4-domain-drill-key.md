# CCAR-F Domain Drill — Domain 4: key and rationales

Grade in one deferred pass. For every miss, note which it was: didn't know it,
misread the item, or picked the plausible-but-soft option.

---

**1. D** — Domain 4.1 emphasizes replacing vague qualifiers with explicit categorical criteria. Option D converts 'significant' into concrete thresholds and an in-scope definition. Option A keeps vagueness, option C lacks explicit bounds, and option B sets an arbitrarily narrow scope. *(task 4.1; concept: explicit_criteria; item `d4d-01`)*

---

**2. C** — Explicit categorical criteria and required evidence fields eliminate ambiguity in what counts as 'high-risk.' Option C defines the tiers and required evidence. Option A preserves subjectivity, option B avoids classification entirely, and option D narrows scope using an unstated proxy rather than risk criteria. *(task 4.1; concept: explicit_criteria; item `d4d-02`)*

---

**3. D** — Few-shot examples demonstrating desired shape and edge cases are more effective than additional rules alone. Option D supplies concrete pattern references, while option A multiplies brittle rules, option B discards relevant context, and option C removes guidance. *(task 4.2; concept: few_shot; item `d4d-03`)*

---

**4. B** — An enum enforces a closed set of valid values, which the downstream parser can categorize reliably. Option A introduces markdown wrappers that break parsing, option C removes type constraints, and option D chooses a non-JSON representation. *(task 4.3; concept: schema_design; item `d4d-04`)*

---

**5. B** — A clean schema needs clear required and nullable distinctions plus enums for closed sets. Option A adds parse-breaking wrappers, option C makes output non-deterministic, and option D defeats the purpose of structured output. *(task 4.3; concept: schema_design; item `d4d-05`)*

---

**6. C** — Retry is appropriate when the source contains the information but the model failed structurally. Option C addresses the structural failure by telling the model to ignore the footnote. Option A randomizes output without fixing the cause, option B is wrong because the information is present, and option D destructively alters source documents. *(task 4.4; concept: retry_validation; item `d4d-06`)*

---

**7. A** — Retry is appropriate when the source likely contains the information but the model failed structurally. Option A targets that failure mode by instructing the model to look in the right places. Repeated identical reruns (B) won't help, hiding results (C) discards data, and routing to human review (D) is a later step, not the first. *(task 4.4; concept: retry_validation; item `d4d-07`)*

---

**8. A** — Batch API suits latency-tolerant large jobs such as scanning millions of files, while synchronous API suits interactive pre-merge gates needing immediate feedback. Options B and C misuse synchronous or batch for one workload, and option D reverses the correct pairing. *(task 4.5; concept: batch_vs_sync; item `d4d-08`)*

---

**9. B** — Independent review catches subtle defects that self-review misses because the generator's own reasoning biases confirmation. Option A repeats the same bias, option C removes review, and option D changes generation randomness rather than improving verification. *(task 4.6; concept: self_review_vs_independent; item `d4d-09`)*

---

**10. A** — Independent review by a separate instance with evidence-based justification overcomes the confirmation bias inherent in self-review. Option B introduces randomness without independent scrutiny, option C weakens the checklist, and option D trains the generator but preserves the same self-review blind spot. *(task 4.6; concept: self_review_vs_independent; item `d4d-10`)*

---
