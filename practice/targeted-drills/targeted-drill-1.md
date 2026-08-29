# CCAR-F Targeted Drill 1 — CI & Isolation

10 items, one correct answer each. Untimed. Answer all 10 first, then
grade against the key in one pass — item by item, reading each rationale,
including the ones you got right. This drill targets CI/CD integration,
plan mode vs direct execution, and isolation patterns.

---

**1.** `[task 3.4 · plan mode vs direct execution]` A team at a fintech startup is deciding how to route a Claude Code request. They have 4 pending tasks: a migration touching 42 files with 3 competing approaches, a one-line crash fix, a variable rename across 5 call sites, and a new guard clause with an obvious implementation. Which situation calls for plan mode?

A. A migration touches dozens of files and several competing implementation approaches exist.
B. A crash trace names one exact line and the needed one-line fix is already obvious to the engineer.
C. A teammate wants one variable renamed consistently across a few nearby call sites.
D. A single function needs one new guard clause with an already clear implementation.

---

**2.** `[task 3.4 · direct execution]` A developer asks Claude Code to add one guard clause rejecting negative amounts in a function they have already located. What is the appropriate mode here?

A. Enter plan mode first so several architectural options get weighed before any change.
B. Proceed with direct execution, since the change is small and the fix is clear.
C. Spawn a dedicated Explore subagent to search the whole repository before touching it.
D. Pause the session and ask the user to choose between three validation strategies.

---

**3.** `[task 3.6 · CI isolation]` A CI pipeline runs Claude Code to review pull requests. The team configures the pipeline to mount the repository inside a fresh container together with the project CLAUDE.md, and separately compares the PR commit against main. What specifically provides environment isolation and reproducibility, separate from which code revision gets reviewed?

A. Running the review against the main branch commit instead of the pull request commit.
B. Reusing the engineer's personal machine settings so review style matches their local setup.
C. Mounting the repository inside a fresh container together with the project CLAUDE.md.
D. Comparing the PR commit against the main branch commit in the same container.

---

**4.** `[task 3.6 · CI isolation]` A team's CI review pipeline behaves identically across every run on the same commit, while their local Claude Code sessions differ by laptop. An engineer proposes adding the pull request branch comparison to local sessions too, hoping this will make local output as reproducible as CI. Will this fix the inconsistency?

A. Yes, since branch comparison is what makes CI output reproducible across machines.
B. Yes, but only once every laptop is also running the exact same operating system.
C. No, because CI reproducibility actually depends on which model version is deployed.
D. No, the reproducibility comes from the fresh container and shared CLAUDE.md.

---

**5.** `[task 4.5 · batch processing]` A security team must scan three million archived log files for suspicious token patterns. The scan is not time-critical but must complete within 48 hours. Which API approach fits this workload?

A. Batch API, since the weekly deadline tolerates its longer processing window.
B. Synchronous API, processing every file one request at a time.
C. A single oversized synchronous request covering every case at once.
D. An interactive session someone checks periodically over the weekend.

---

**6.** `[task 4.6 · self-review vs independent]` A generative coding assistant drafts unit tests for a new authentication module. The same model instance that wrote the code also reviews it and consistently approves the tests. Bugs still reach production. Which change is most likely to catch the remaining defects?

A. Review again with a 'be critical' prefix.
B. Route the tests to a separate model instance for independent review.
C. Remove all automated review and ship whatever the generator produces.
D. Increase the generator temperature so it writes different tests next time.

---

**7.** `[task 2.5 · built-in tool selection]` An agent needs to find every call site of a function named `calculateTax` across a 380-file TypeScript monorepo before renaming it to `calculateTaxV2`. The team has a custom search script that indexes function definitions but only covers `/src/services/`. When should the agent reach for the built-in Grep tool?

A. When the search needs to cover the full repository, not just the custom script's partial index.
B. Never — a custom implementation always significantly outperforms the built-in tool.
C. Only when the files being searched are unusually large in size.
D. Only when searching plain text files, never when searching source code.

---

**8.** `[task 1.4 · programmatic enforcement]` A support agent must issue a refund credit only after a supervisor approves the case. The prompt repeatedly says 'wait for approval,' yet during a load test with 50 concurrent sessions, 3 credits slip through on unapproved cases. Which control actually removes the failure mode?

A. Add more 'wait for approval' examples to the system prompt so the pattern is better reinforced.
B. Set the model temperature to zero so it follows the approval instruction exactly.
C. Intercept the credit tool call until a supervisor-controlled approval flag is set.
D. Add a post-call validator that reverses any credit issued without recorded approval.

---

**9.** `[task 5.4 · codebase exploration]` Claude is dropped into a 400-file e-commerce repository it has never seen before. The user asks it to add a feature that creates discount codes. The repository has no README and the directory structure includes `/src/`, `/lib/`, `/legacy/`, and `/packages/`. What should Claude do before writing any code?

A. Read every file in the repository from top to bottom before starting.
B. Ask the user to explain the entire architecture before doing anything else.
C. Search for the key entry points and files related to discount codes to orient itself quickly.
D. Make a small test change to see how the system reacts to it.

---

**10.** `[task 5.4 · pattern discovery]` A codebase uses a custom, undocumented dependency-injection framework. Claude needs to add a new feature that fits the existing pattern. What should it do first?

A. Apply a standard dependency-injection pattern from another framework and hope it fits.
B. Ask the user to fully explain the framework before writing any code.
C. Rewrite the injection framework to something more conventional before proceeding.
D. Search the codebase for existing examples of the pattern already in use.

---
