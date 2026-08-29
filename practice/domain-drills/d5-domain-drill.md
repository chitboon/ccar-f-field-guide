# CCAR-F Domain Drill — Domain 5: Context Management & Reliability

10 items, one correct answer each. Untimed. Answer all 10 first, then
grade against the key in one pass — item by item, reading each rationale,
including the ones you got right. This is a scenario-based drill for this
domain, not a recall check: every stem carries a concrete situation you
must reason about.

---

**1.** `[task 5.1 · structured facts vs summarization]` A subagent needs to look up exact order totals and dates repeatedly during a long investigation of a disputed $12,400 invoice. Should it receive a prose summary of the case or a structured facts block?

A. A structured facts block, so exact values are looked up precisely, not re-parsed from prose.
B. A prose summary, since natural language is generally easier for the model to read and understand quickly in most contexts.
C. Whichever format is shorter, since token budget matters more than the format actually used.
D. The full raw transcript, so that nothing gets lost in any kind of summarization process.

---

**2.** `[task 5.2 · escalation ambiguity]` A user asks Claude to 'fix the issue with the payment module.' The repository has 3 payment-related modules — `payments-v1`, `payments-v2`, and `payments-api` — and each has open issues. The user's message doesn't specify which module or which issue. What is Claude's best first move?

A. Make its best guess at the user's intent and proceed without asking.
B. Escalate the request to a human immediately, without attempting to resolve it.
C. Ask a clarifying question rather than guessing or escalating prematurely.
D. Return an error explaining that the request could not be understood.

---

**3.** `[task 5.2 · escalation after clarification]` After Claude asks for clarification, the user replies 'just pick one, I don't care.' What should Claude do next?

A. Refuse to proceed and ask the clarifying question again in different words.
B. Choose one reasonable option and say plainly which one it picked and why.
C. Escalate the decision to a manager rather than choosing on its own.
D. Produce three separate versions so the user can pick after the fact.

---

**4.** `[task 5.3 · error propagation]` A research coordinator delegates a database lookup to a subagent. Midway through, the subagent's connection to the database drops after retrieving 2 of 5 required tables. The subagent has partial results but cannot complete the query. What should it report back to the coordinator?

A. Nothing at all — silently continue working and report success once the task eventually finishes later.
B. The raw, unparsed error trace exactly as the failing tool returned it.
C. A vague note that 'something went wrong' without further detail.
D. A structured error stating what failed, what partial results exist, and the impact on the larger task.

---

**5.** `[task 5.3 · mixed outcomes]` A coordinator dispatches three subagents in parallel: one completes successfully, one fails with a rate-limit error, and one fails with a permission error. The coordinator must decide how to summarize this for the calling process. What is the correct response?

A. Report each outcome, distinguishing the transient failure from the persistent one needing a fix.
B. Report overall success, since at least one of the three subagents did complete successfully.
C. Report overall failure, since two of the three subagents did not complete their tasks.
D. Escalate all three results to a human without providing any further summarization at all.

---

**6.** `[task 5.4 · codebase exploration first move]` Claude is dropped into a 400-file e-commerce repository it has never seen before. The user asks it to add a feature that creates discount codes. The repository has no README and the directory structure includes `/src/`, `/lib/`, `/legacy/`, and `/packages/`. What should Claude do before writing any code?

A. Read every file in the repository from top to bottom before starting.
B. Ask the user to explain the entire architecture before doing anything else.
C. Search for the key entry points and files related to discount codes to orient itself quickly.
D. Make a small test change to see how the system reacts to it.

---

**7.** `[task 5.4 · pattern discovery]` A codebase uses a custom, undocumented dependency-injection framework. Claude needs to add a new feature that fits the existing pattern. What should it do first?

A. Apply a standard dependency-injection pattern from another framework and hope it fits.
B. Ask the user to fully explain the framework before writing any code.
C. Rewrite the injection framework to something more conventional before proceeding.
D. Search the codebase for existing examples of the pattern already in use.

---

**8.** `[task 5.4 · multiple patterns]` While exploring a codebase, Claude finds three different validation patterns used in different modules, each written by a different team at a different time. It has been asked to add validation to a fourth module. What should it prioritize before writing any code?

A. Immediately adopt whichever of the three existing patterns looks the most modern.
B. Understand why each pattern exists before deciding which one, if any, to follow.
C. Unify all three patterns into one before adding the fourth module's validation logic.
D. Skip validation for the new module until the whole team agrees on one single pattern.

---

**9.** `[task 5.5 · aggregate vs stratified]` A team wants to know whether a new prompt change improved accuracy overall, or only for certain kinds of requests. The overall accuracy moved from 91% to 93%, but the team suspects some request types may have regressed. Which reporting approach answers that?

A. Stratified accuracy by type, since one number can hide gains and losses across segments.
B. A single aggregate accuracy number, since that is simply the easiest thing to report overall.
C. Stratified accuracy, but only when the dataset happens to be small enough to segment easily.
D. Aggregate accuracy, restricted only to health-check-style monitoring rather than full evaluation.

---

**10.** `[task 5.6 · claim provenance]` A research agent writes a report stating that 'API latency increased by 34% after the Q3 deployment.' The report cites no source for the 34% figure. A teammate questions whether the number is accurate. Why should Claude trace factual claims like this back to their source?

A. It makes the response sound more authoritative and confident to the reader overall.
B. It naturally lengthens the response, which readers tend to trust more as a result.
C. It lets the claim be checked independently, rather than simply asserted as unverifiable fact.
D. It satisfies formatting conventions that are expected in academic-style writing generally.

---
