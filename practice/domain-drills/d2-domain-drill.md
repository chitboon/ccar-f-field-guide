# CCAR-F Domain Drill — Domain 2: Tool Design & MCP Integration

10 items, one correct answer each. Untimed. Answer all 10 first, then
grade against the key in one pass — item by item, reading each rationale,
including the ones you got right. This is a scenario-based drill for this
domain, not a recall check: every stem carries a concrete situation you
must reason about.

---

**1.** `[task 2.1 · tool description quality]` A team is building a customer-support tool that handles refunds, cancellations, and order tracking. The tool's parameter descriptions say 'input: any text' for the action field. The model keeps calling the tool with vague inputs like 'help the customer' instead of structured actions like 'refund: order #4892, amount $34.50.' Why do the parameter descriptions matter here?

A. They generate the tool's complete developer-facing documentation automatically without any further editing needed.
B. They shorten the network round-trip time incurred by each individual tool call.
C. They give the model the constraints and signals it needs to judge when and how a tool applies.
D. They stop malformed JSON payloads from ever reaching the tool's argument parser.

---

**2.** `[task 2.1 · tool misuse fix]` A tool is misused because its parameter descriptions are ambiguous. What is the best fix?

A. Add examples to the parameter descriptions so the model sees exactly when each applies.
B. Lower the model's temperature so it follows the existing instructions more conservatively.
C. Add a confirmation step that asks the user to explicitly approve every call before it is run.
D. Rename the tool to something more memorable so engineers reference it correctly across the codebase.

---

**3.** `[task 2.2 · structured error signal]` A supply-chain agent queries a warehouse inventory tool for SKU #4892 and receives a structured `error_code` of 'NOT_FOUND' with a message field stating 'SKU not in current catalog.' The agent needs to decide whether to retry, search an alternate warehouse, or flag the SKU as discontinued. Why is the structured error_code useful here?

A. It significantly reduces the size of the response payload sent back to the client application.
B. It lets the model choose a properly targeted recovery path instead of guessing at the failure.
C. It automatically and measurably improves the quality of the system's internal logging output.
D. It noticeably speeds up the execution time of the next tool call somewhere in the chain.

---

**4.** `[task 2.2 · error differentiation]` A tool can return either RATE_LIMIT or PERMISSION_DENIED as its error code. How should the model treat the two differently?

A. Treat both as requiring the exact same immediate escalation to a human reviewer.
B. Retry RATE_LIMIT since it's transient; treat PERMISSION_DENIED as a config issue.
C. Retry both error types immediately, using the same fixed backoff schedule regardless.
D. Ignore both error types and continue the workflow as though the call had actually succeeded.

---

**5.** `[task 2.3 · tool removal as control]` A healthcare agent has access to 6 tools including a 'write_prescription' tool. The compliance team requires that prescriptions be generated only through a separate, audited system — never by the agent directly. During testing, the agent invoked `write_prescription` in 3 out of 200 sessions despite prompt warnings. What is the most reliable way to prevent Claude from ever using that tool?

A. Warn Claude in the system prompt that the tool should not be used.
B. Reduce the model's temperature so it explores fewer unexpected actions.
C. Remove the tool from the set of tools passed to the model entirely.
D. Wrap the tool in a confirmation dialog that the user must approve.

---

**6.** `[task 2.3 · structural tool scoping]` A customer support agent can view invoices and, separately, issue billing disputes. The team wants agents to pull up invoice details freely but never file a dispute without a supervisor's review, no matter how the request is phrased in conversation. Which approach best guarantees this split?

A. Give the agent only the view_invoice tool, and route disputes through an approval workflow it can't bypass.
B. Provide both tools but add a system-prompt warning that disputes require supervisor sign-off beforehand.
C. Provide both tools and add post-call logging so that disputes can be audited after the fact later.
D. Provide both tools but ask the model to add a confirmation step before it files any dispute.

---

**7.** `[task 2.4 · MCP scope selection]` A platform team is configuring an agent that will handle customer onboarding. They have MCP servers for email, CRM, billing, and a general web search. The onboarding flow only needs email verification and CRM lookup. Which default should govern which servers to expose?

A. Expose every server that is available so the agent has maximum capability.
B. Expose only the servers the current task actually needs, and nothing more.
C. Expose servers only when the user explicitly names one by name.
D. Rotate which servers are exposed on a schedule to spread the load.

---

**8.** `[task 2.4 · MCP scope for research]` An agent is being configured to help with academic literature research. Which set of MCP servers should it be given?

A. Every server the organization has configured, so that the agent is never blocked at all.
B. A database server only, since research ultimately comes down to stored records in the end.
C. A file-system server only, so the agent can save all of its findings locally.
D. A web-search server and a citation-database server, matching what the task actually requires.

---

**9.** `[task 2.5 · built-in tool selection]` An agent is asked to find every call site of a function named `processRefund` across a 400-file TypeScript monorepo. The team has a custom search script that indexes function definitions, but it only covers the `/src/services/` directory. When should the agent reach for the built-in Grep tool instead of the custom script?

A. When the search needs to cover the full repository, not just the custom script's partial index.
B. Never — the custom implementation always significantly outperforms the built-in tool.
C. Only when the files being searched are unusually large in size.
D. Only when searching plain text files, never when searching source code.

---

**10.** `[task 2.5 · tool combination for refactor]` An agent is refactoring a module: it needs to read existing files, find every call site of a renamed function, edit each one, and then confirm the new imports resolve correctly. Which combination of tools fits this work best?

A. Rely only on custom tools built specifically for this refactor, since built-in tools are too generic for it.
B. Rely only on the built-in Grep tool, and skip the edit and import-validation steps entirely.
C. Use built-in Read, Grep, and Edit tools for the file work, plus a custom validator for imports.
D. Rely only on built-in tools for everything, including the domain-specific import-validation step.

---
