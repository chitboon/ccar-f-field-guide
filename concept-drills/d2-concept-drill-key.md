# CCAR-F Concept Drill — Domain 2: Tool Design & MCP Integration: key and rationales

Grade in one deferred pass. For every miss, note which it was: didn't know it,
misread the item, or picked the plausible-but-soft option.

---

**1. B** — Tool descriptions are the primary mechanism the model uses for selection; near-identical one-line descriptions make misrouting inevitable. Routing classifiers add machinery around the root cause, merging is a bigger change than needed, and few-shot examples add tokens without fixing the descriptions. *(task 2.1; concept: description_driven_selection; item `d2-n1`)*

---

**2. D** — Valid empty results and access failures must be structurally distinguishable so the agent can escalate real failures and report honest no-matches. Placeholders and relabeled strings hide the real state; auto-escalation worsens the false-positive rate. *(task 2.2; concept: structured_error_signal; item `cc-14`)*

---

**3. A** — Project scope (.mcp.json) is version-controlled and shared on clone; user scope (~/.claude.json) is per-person and never committed. Comments are not a scoping mechanism, CLAUDE.md does not define MCP servers, and user scope cannot be shared. *(task 2.4; concept: project_vs_user_scope; item `d2-n3`)*

---

**4. C** — Uniform strings hide the error category, so the coordinator cannot decide between retrying a transient failure and not retrying a permission refusal. Blind retries hammer permission failures; log levels and suppression do not fix recovery decisions. *(task 2.2; concept: structured_error_signal; item `cc-05`)*

---

**5. B** — Grep searches file contents for a pattern, which is what 'find every call site' requires; Glob matches file paths and names only and says nothing about contents. Reading the whole tree wastes context; a path glob cannot enumerate callers. *(task 2.5; concept: grep_for_call_sites_glob_for_paths; item `d2-n7`)*

---

**6. A** — Restricting allowedTools to role-relevant tools removes the off-role options from the decision surface entirely. Forcing 'any' still permits wrong choices; examples and longer descriptions soften but do not prevent misuse. *(task 2.3; concept: tool_choice_vs_scoping; item `cc-13`)*

---

**7. D** — The latency win comes from removing model turns: every tool call requires the model to reason about the result before the next step, while a resource is read straight by the client. It is not a faster per-call server response, a compression trick, or a billing difference. *(task 2.4; concept: resources_vs_tools_round_trip; item `d2-n6`)*

---

**8. C** — Scoping allowedTools to role-appropriate sets removes the decision surface and prevents misuse by construction. Forcing 'any' still permits wrong choices; better descriptions help selection but do not prevent misuse; post-call rejection happens later than prevention. *(task 2.3; concept: tool_choice_vs_scoping; item `cc-04`)*

---

**9. B** — .mcp.json expands environment variables, so the committed file names the variable and the secret stays in each developer's shell. Placeholders drift out of sync, CLAUDE.md is committed documentation, and base64 is encoding, not secrecy. *(task 2.4; concept: env_var_expansion; item `d2-n4`)*

---

**10. A** — System-prompt wording influences tool selection, and keyword-sensitive phrasing can override well-written tool descriptions. The timing of the change points at the prompt edit; registration, temperature, and truncation are speculative mechanisms with no evidence here. *(task 2.1; concept: keyword_sensitive_tool_selection; item `d2-n2`)*

---

**11. C** — Tools from every configured server are discovered when the connection is established and then available simultaneously; a server whose handshake or startup failed contributes nothing, which is exactly the observed symptom. Ordering in the file, prompt restarts, and name uniqueness are not the discovery mechanism. *(task 2.4; concept: discovery_at_connection_time; item `d2-n5`)*

---

**12. D** — Forced tool selection is the only option that deterministically pins the first call; 'any' guarantees a tool call but not which one, array order is not a selection constraint, and prompt instructions remain probabilistic. *(task 2.3; concept: forced_tool_choice_ordering; item `d2-n8`)*

---
