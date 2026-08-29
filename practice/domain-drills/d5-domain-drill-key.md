# CCAR-F Domain Drill — Domain 5: key and rationales

Grade in one deferred pass. For every miss, note which it was: didn't know it,
misread the item, or picked the plausible-but-soft option.

---

**1. A** — Structured facts support precise, repeated lookups without re-parsing prose each time. Ease of reading, brevity, and completeness of the raw transcript are all secondary to the need for exact, reusable values. *(task 5.1; concept: structured_facts_vs_summarization; item `d5d-01`)*

---

**2. C** — A clarifying question resolves the ambiguity directly. Guessing risks misalignment with intent, escalating skips a step the model can handle itself, and an error is unhelpful when the request is answerable. *(task 5.2; concept: escalation_ambiguity; item `d5d-02`)*

---

**3. B** — Once the user has declined to specify, a transparent choice is the most helpful path forward. Repeating the question adds friction, escalation is unnecessary once the user has spoken, and producing multiple versions defers work back onto the user. *(task 5.2; concept: escalation_ambiguity; item `d5d-03`)*

---

**4. D** — A structured error lets the coordinator understand the impact and decide how to proceed. Silently continuing hides the problem, a raw trace is hard for the coordinator to parse, and a vague note carries no actionable information. *(task 5.3; concept: error_propagation_multiagent; item `d5d-04`)*

---

**5. A** — The two failures need different handling, so collapsing them into one binary success/failure verdict loses information the caller needs. Reporting flat success or flat failure both discard that distinction, and escalating everything skips summarization the coordinator is capable of doing itself. *(task 5.3; concept: error_propagation_multiagent; item `d5d-05`)*

---

**6. C** — Targeted search for entry points and structure gives fast orientation without wasting effort. Reading everything is inefficient at scale, deferring entirely to the user skips work the model can do itself, and modifying before understanding risks breaking something unseen. *(task 5.4; concept: codebase_exploration; item `d5d-06`)*

---

**7. D** — Existing examples reveal the local convention directly and fastest. Importing a foreign standard pattern risks a mismatch, deferring everything to the user is slower than it needs to be, and rewriting the framework is a disproportionate and risky first move. *(task 5.4; concept: codebase_exploration; item `d5d-07`)*

---

**8. B** — Understanding the reasoning behind each pattern avoids breaking dependencies that aren't obvious from the code alone. Picking the most modern one ignores that reasoning, unifying everything is a much larger and riskier task than asked for, and skipping validation entirely doesn't complete the assignment. *(task 5.4; concept: codebase_exploration; item `d5d-08`)*

---

**9. A** — Only a stratified breakdown can reveal whether gains in one segment are masking losses in another. A single aggregate number hides exactly that, and neither dataset size nor the health-check framing changes what the question is actually asking for. *(task 5.5; concept: aggregate_vs_stratified; item `d5d-09`)*

---

**10. C** — Provenance is what makes a claim independently checkable and prevents unverifiable statements from being passed off as established fact. Sounding authoritative, added length, and formatting convention are all surface effects, not the actual reason provenance matters. *(task 5.6; concept: claim_source_provenance; item `d5d-10`)*

---
