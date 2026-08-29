# CCAR-F Targeted Drill 2 — Codebase Exploration

10 items, one correct answer each. Untimed. Answer all 10 first, then
grade against the key in one pass — item by item, reading each rationale,
including the ones you got right. This drill targets codebase exploration
strategies, structured facts, error propagation, and review routing.

---

**1.** `[task 5.4 · codebase exploration]` Claude is dropped into a 600-file fintech repository it has never seen. The user asks it to add a feature that generates monthly account statements, touching the PDF renderer, the data-fetch layer, and the email service. What should Claude do before writing any code?

A. Search the repository for entry points and existing patterns the new feature should follow.
B. Ask the user to describe the whole architecture before looking at any files.
C. Assume the newest framework conventions apply and write the feature that way.
D. Write a first draft immediately, then adjust it once problems surface later.

---

**2.** `[task 5.4 · undocumented helper]` A repository uses an undocumented, project-specific pagination helper called `paginateQuery` across its 17 API endpoints. Claude needs to add one more paginated endpoint. The helper has no README and its parameters are not self-documenting. What is the best first move?

A. Ask the user to fully explain how the pagination helper works before proceeding further.
B. Search the codebase for existing endpoints using the same pagination helper.
C. Implement a standard offset-based pagination pattern from another project.
D. Skip pagination for now and add it back once the endpoint is reviewed.

---

**3.** `[task 5.4 · auth middleware]` Claude is asked to add a new authenticated route to a service. It notices the codebase has a custom auth middleware with no README explaining its configuration options. Rather than guessing at the options or asking the user immediately, what should Claude do first?

A. Pick reasonable-looking defaults for authentication and add the new route regardless.
B. Ask the user to walk through every configuration option before proceeding.
C. Search for other routes already using the middleware and read how they configure it.
D. Disable authentication on the new route until someone documents the middleware.

---

**4.** `[task 5.4 · test conventions]` A test suite in an unfamiliar repository uses a naming convention and fixture setup Claude has not seen described anywhere in the project documentation. Claude needs to add a new test for a bug fix. What is the most efficient way to match the existing convention?

A. Ask the user to explain the full testing convention before writing anything.
B. Write the test using a generic framework style unrelated to this project.
C. Leave the new test unwritten and note that conventions were unclear.
D. Read a few existing tests nearby to see the pattern already in use.

---

**5.** `[task 5.4 · configuration loading]` A developer asks Claude to fix a subtle bug in how a service loads configuration values. Claude finds three different places in the codebase where configuration appears to be read, each using a slightly different loading order, and no single document explains which one is authoritative. Before proposing a fix, what should Claude do?

A. Trace how each loading path is actually used at runtime before proposing a fix.
B. Pick the most recently modified file among the three and assume it is authoritative.
C. Ask the user which of the three loading paths is the correct one to trust.
D. Rewrite all three loading paths into one unified configuration loader immediately.

---

**6.** `[task 5.1 · structured facts carry-forward]` During a long codebase investigation spanning 45 tool calls, Claude discovers the exact file paths and function names of 7 modules it will need to edit in later steps. The session is approaching its context limit. How should this information best be carried forward?

A. Folded into a single paragraph summarizing the whole investigation so far.
B. In a persistent structured facts block, apart from the summary.
C. Left implicit, relying on the model to recall it from earlier context.
D. Restated only once at the very end of the investigation session.

---

**7.** `[task 5.1 · lost in the middle]` A synthesis agent receives a very long combined document from several subagents, with the most important findings buried in the middle sections. What risk does this position create?

A. The model may reliably process the start and end but miss the middle.
B. The document will fail to load at all once it exceeds a certain total length.
C. The subagents will be blocked from submitting any further findings afterward.
D. The coordinator will be charged extra for reading content placed in the middle.

---

**8.** `[task 5.3 · partial results]` A coordinator delegates a codebase search to a subagent, which fails partway through because one directory is unreadable. Some results were still gathered before the failure. What should the subagent report back?

A. An empty result set, since the search as a whole did not fully complete.
B. A generic failure message with no further detail about what happened.
C. Nothing at all, letting the coordinator infer failure from a missing reply.
D. A structured report naming what failed and what partial results exist.

---

**9.** `[task 5.3 · mixed outcomes interpretation]` A coordinator sends the same search query to three subagents covering different parts of a monorepo. One subagent returns genuinely no matches for its section, one times out partway through its section, and one completes normally with several matches. How should the coordinator interpret the first two outcomes?

A. Treat the empty result as a valid finding and treat the timeout as needing a retry decision.
B. Treat both the empty result and the timeout the same way, as equally uncertain outcomes.
C. Discard the empty result entirely, since an agent that found nothing added no value.
D. Retry both the empty result and the timeout using the exact same follow-up query.

---

**10.** `[task 5.5 · aggregate vs stratified review]` A team reviewing Claude's codebase change suggestions reports an overall acceptance rate of 97% across the whole project. Before reducing human review further, a lead wants to check whether this figure could be hiding something. What should they check first?

A. Whether the overall 97% figure has stayed the same for the past two weeks.
B. Whether acceptance rates are also consistently high when broken down by file type and module.
C. Whether the same 97% number appears in last quarter's separate report too.
D. Whether 97% is higher than the acceptance rate of a competing internal tool.

---
