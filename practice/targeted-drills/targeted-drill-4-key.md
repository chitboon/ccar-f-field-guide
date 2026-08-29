# CCAR-F Targeted Drill 4 — Mixed Weak Spots Remix: key and rationales

Grade in one deferred pass. For every miss, note which it was: didn't know it,
misread the item, or picked the plausible-but-soft option.

---

**1. A** — User-level CLAUDE.md applies only to that individual and is never shared through version control, so a new teammate would not receive it. Project-level, shared rules, and directory-level files are all part of the tracked project configuration that every teammate does receive. *(task 3.1; concept: config_scope; item `t4-01`)*

---

**2. B** — The `context: fork` option runs a skill in an isolated sub-agent context, keeping verbose exploratory output out of the main conversation. Argument hints, tool restrictions, and personal placement address different concerns entirely. *(task 3.2; concept: commands_and_skills; item `t4-02`)*

---

**3. C** — The fresh container and shared CLAUDE.md govern environment isolation and reproducibility, a separate concern from which commit is compared. The main branch comparison is the code-state axis, and the remaining options misdescribe the two settings entirely. *(task 3.6; concept: ci_isolation; item `t4-03`)*

---

**4. D** — Local inconsistency comes from personal configuration layering on top of the shared project setup, not from which branch is compared. Branch comparison is a code-state setting and would not remove that personal layer, so the proposed fix targets the wrong axis. *(task 3.6; concept: ci_isolation; item `t4-04`)*

---

**5. A** — Few-shot examples that demonstrate reasoning for ambiguous cases directly show the desired pattern, which prose alone often fails to convey consistently. More prose, lower temperature, and splitting the prompt do not address the actual gap between instruction and consistent behavior. *(task 4.2; concept: few_shot; item `t4-05`)*

---

**6. B** — Making the field optional removes the pressure to satisfy a required field when the source information genuinely does not exist. A longer description, more context, or a different data type do not remove the incentive to fabricate a value where none exists. *(task 4.3; concept: schema_design; item `t4-06`)*

---

**7. C** — A weekly deadline comfortably tolerates the Batches API's multi-hour processing window, at lower cost than synchronous calls. General capability, an interactive session, and one oversized request all pay unnecessary synchronous overhead for a task this latency-tolerant. *(task 4.5; concept: batch_vs_sync; item `t4-07`)*

---

**8. D** — An explicit customer request for a human should be honored immediately rather than second-guessed with an investigation the customer did not ask for. Trying to resolve it first, arguing against the request, or relying on a confidence score all override a preference the customer already stated plainly. *(task 5.2; concept: escalation_ambiguity; item `t4-08`)*

---

**9. A** — Searching for the dominant recent pattern grounds the change in evidence already present in the codebase. Asking the user first, picking a style by appearance, and rewriting everything before investigating all skip the available self-service search. *(task 5.4; concept: codebase_exploration; item `t4-09`)*

---

**10. B** — Finding existing publishers and subscribers gives concrete evidence for how the bus is meant to be used. Asking the user for everything up front, guessing from an unrelated project, and bypassing the bus altogether all skip or ignore the working examples already present. *(task 5.4; concept: codebase_exploration; item `t4-10`)*

---
