# CCAR-F Domain Drill — Domain 2: key and rationales

Grade in one deferred pass. For every miss, note which it was: didn't know it,
misread the item, or picked the plausible-but-soft option.

---

**1. C** — Parameter descriptions are the model's only source of the constraints and applicability signals it needs to reason about when and how a tool should be used. Latency, documentation generation, and payload validation are separate, unrelated concerns. *(task 2.1; concept: tool_description_quality; item `d2d-01`)*

---

**2. A** — Concrete examples fix the root cause by clarifying the model's reasoning directly. Temperature, confirmation steps, and renaming all address symptoms rather than the ambiguity itself. *(task 2.1; concept: tool_description_quality; item `d2d-02`)*

---

**3. B** — Structured error codes signal the specific failure mode, letting the model choose a targeted recovery action. Payload size, execution speed, and logging quality are unrelated to why the signal is useful. *(task 2.2; concept: structured_error_signal; item `d2d-03`)*

---

**4. B** — The two codes signal different root causes: RATE_LIMIT is transient and worth retrying, while PERMISSION_DENIED is a persistent configuration problem that retrying cannot resolve. Treating them identically wastes retries or escalates unnecessarily. *(task 2.2; concept: structured_error_signal; item `d2d-04`)*

---

**5. C** — Removing the tool from the registry is a deterministic, structural control. Prompt warnings, temperature, and confirmation dialogs are all still probabilistic and can be talked around. *(task 2.3; concept: tool_choice_vs_scoping; item `d2d-05`)*

---

**6. A** — Structurally removing the dispute tool is the only approach that holds regardless of phrasing. Prompt warnings, post-hoc logging, and self-confirmation are all prompt-level controls the model itself could be talked past. *(task 2.3; concept: tool_choice_vs_scoping; item `d2d-06`)*

---

**7. B** — Scoping to what the task needs keeps the tool surface small and avoids tool-selection confusion. Exposing everything adds noise, naming-only is too narrow to be a general rule, and rotation serves no discrimination purpose. *(task 2.4; concept: mcp_scope_selection; item `d2d-07`)*

---

**8. D** — The two servers that map directly to the task's needs are search and citations. Every-server access is excessive, and either single-purpose option alone leaves a genuine capability gap. *(task 2.4; concept: mcp_scope_selection; item `d2d-08`)*

---

**9. A** — Built-in Grep covers the full repository with pattern matching, which is exactly what finding every call site requires. The custom script's partial index misses the directories outside `/src/services/`. The blanket 'never' and the file-size or text-only restrictions aren't real limitations of the built-in tool. *(task 2.5; concept: builtin_tool_selection; item `d2d-09`)*

---

**10. C** — Built-in tools handle the generic file operations well; the import-validation logic is domain-specific and belongs in a custom tool. All-custom and all-built-in both misplace where the domain logic should live, and skipping steps leaves the refactor unfinished. *(task 2.5; concept: builtin_tool_selection; item `d2d-10`)*

---
