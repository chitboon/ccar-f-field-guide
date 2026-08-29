# CCAR-F Targeted Drill 4 — Mixed Weak Spots Remix

10 items, one correct answer each. Untimed. Answer all 10 first, then
grade against the key in one pass — item by item, reading each rationale,
including the ones you got right. This is a mixed-topic drill targeting
weak spots across configuration, prompt engineering, codebase exploration,
and context management.

---

**1.** `[task 3.1 · config scope]` A new engineer joins a 12-person team and clones the shared repository. The senior teammate insists the project's Claude Code conventions are documented, but the new engineer's sessions don't follow them. The conventions were placed in the senior teammate's personal `~/.claude/CLAUDE.md` file. Where were the conventions most likely placed?

A. In the senior teammate's personal user-level CLAUDE.md file on their own machine.
B. In the project's root CLAUDE.md file, tracked by version control for the whole team.
C. In a topic-specific rules file under the project's shared rules directory.
D. In a directory-level CLAUDE.md file placed inside a shared package folder.

---

**2.** `[task 3.2 · skill isolation]` A team builds a `/analyze` skill that runs a verbose codebase analysis producing 50+ lines of exploratory output. They want this output to stay isolated from the main conversation so the user sees only the final summary. Which frontmatter option keeps that output isolated?

A. Setting `argument-hint` so the developer is prompted for required parameters.
B. Setting `context` to `fork` so the skill runs in an isolated sub-agent.
C. Restricting `allowed-tools` so only file write operations are permitted.
D. Placing the skill in a personal directory instead of the shared one.

---

**3.** `[task 3.6 · CI isolation axes]` A CI pipeline runs Claude Code to review pull requests. It mounts the repository in a fresh container with the project CLAUDE.md, and separately compares the PR commit against main. The team wants to understand which setting governs environment isolation versus which code gets reviewed. Which setting governs environment isolation?

A. The comparison against the main branch commit.
B. Neither setting relates to isolation or code state at all.
C. The fresh container together with the project CLAUDE.md.
D. Both settings equally govern which code revision is reviewed.

---

**4.** `[task 3.6 · CI reproducibility]` A team's CI review pipeline behaves identically across every run on the same commit, while their local Claude Code sessions differ by laptop. An engineer proposes adding the pull request branch comparison to local sessions too, hoping this will make local output as reproducible as CI. Will this fix the inconsistency?

A. Yes, since branch comparison is what makes CI output reproducible across machines.
B. Yes, but only once every laptop is also running the exact same operating system.
C. No, because CI reproducibility actually depends on which model version is deployed.
D. No, the reproducibility comes from the fresh container and shared CLAUDE.md.

---

**5.** `[task 4.2 · few-shot for consistency]` A customer-support agent's prompt includes 8 detailed rules for when to use the refund tool versus the exchange tool. Despite these rules, the agent inconsistently selects between the two for ambiguous cases like 'the product didn't work as expected.' What technique most directly improves consistency here?

A. Adding a handful of few-shot examples showing the reasoning behind each choice.
B. Rewriting the instructions in even more exhaustive prose detail than before.
C. Lowering the model's temperature to make its choices more deterministic.
D. Splitting the single prompt into two separate, shorter prompt calls instead.

---

**6.** `[task 4.3 · schema for optional data]` An extraction schema requires a shipping date field on every document, but a portion of the source documents never mention a shipping date at all. Runs against those documents keep producing fabricated dates. What schema change addresses this?

A. Add a longer field description instructing the model not to fabricate dates.
B. Make the shipping date field optional rather than required.
C. Increase the model's context window so it can search harder for a date.
D. Switch the shipping date field from a string type to a numeric type instead.

---

**7.** `[task 4.5 · batch vs sync for weekly job]` A finance team asks for every invoice extraction from the past week to be reconciled into a single spreadsheet before the Monday close meeting, and nobody needs to watch the process run. Which API choice matches this task?

A. The synchronous Messages API, since it is generally the more capable option.
B. An interactive session someone checks periodically over the weekend.
C. The Message Batches API, since the weekly deadline tolerates its longer window.
D. A single oversized synchronous request covering every case at once.

---

**8.** `[task 5.2 · explicit human request]` A customer opens a support session by explicitly requesting a human agent, without describing any specific problem yet. The issue they eventually mention sounds like something the automated support agent could likely resolve on its own. What is the correct response?

A. Try to resolve the issue on its own first, escalating only if the customer repeats the request.
B. Explain that the issue is resolvable and ask the customer to reconsider their request.
C. Delay escalation until a self-reported confidence score falls below some threshold.
D. Honor the explicit request for a human immediately, without further investigation first.

---

**9.** `[task 5.4 · search-first validation]` Claude is asked to extend a data validation layer in a codebase that mixes three different validation styles with no written documentation explaining which is preferred. Which action reflects a search-first approach before making changes?

A. Search the codebase for the validation style used most consistently.
B. Ask the user to describe every validation style in detail before looking at any code.
C. Pick whichever style looks most modern and apply it across the whole layer.
D. Rewrite all three styles into one unified validator before investigating further.

---

**10.** `[task 5.4 · event bus exploration]` A large, undocumented codebase uses a custom event bus for communication between modules, and Claude must wire a new module into it correctly. Which step best reflects efficient, evidence-driven exploration?

A. Immediately ask the user to explain the entire event bus design in a single message.
B. Search for existing modules that already publish or subscribe to bus events.
C. Guess at a wiring pattern based on a different, unrelated project the model recalls.
D. Skip the event bus entirely and have the new module call other modules directly.

---
