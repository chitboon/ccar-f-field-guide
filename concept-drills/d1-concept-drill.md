# CCAR-F Concept Drill — Domain 1

12 items, one correct answer each. Untimed. Answer all 12 first, then
grade against the key in one pass — item by item, reading each rationale,
including the ones you got right. This is a fast recall check for this domain,
not an exam simulator: the domain drills and mocks carry the scenario register.

---

**1.** `[task 1.1 · stop_reason loop control]` A tool-use loop terminates whenever the assistant's text contains 'done' instead of checking stop_reason. Which risk is most direct?

A. The loop may end while tool results are pending.
B. The assistant will always refuse to call any tools.
C. The model will gradually forget the system prompt.
D. The loop will run forever because the word 'done' never appears.

---

**2.** `[task 1.5 · programmatic enforcement]` A support agent must issue a refund credit only after a supervisor approves the case. The prompt repeatedly says 'wait for approval,' yet credits still slip through on unapproved cases during busy hours. Which control actually removes the failure mode?

A. Add more 'wait for approval' examples to the system prompt so the pattern is better reinforced.
B. Set the model temperature to zero so it follows the approval instruction exactly.
C. Intercept the credit tool call until a supervisor-controlled approval flag is set.
D. Add a post-call validator that reverses any credit issued without recorded approval.

---

**3.** `[task 1.7 · session resumption]` A prior Claude Code session mapped a 14-file payment module. Five files changed. The remaining context is still trustworthy. What is the best next step?

A. Start a new session and re-read all 14 files from scratch to rebuild the context.
B. Resume the session, name the five changed files, and re-analyze only those.
C. Fork the session into two branches to compare old and new module states.
D. Inject a summary of the earlier findings and run the tests without re-reading files.

---

**4.** `[task 1.3 · subagent context]` A coordinator delegates research to a subagent. The subagent repeatedly asks for facts already discussed in the parent conversation. What is the root cause?

A. The subagent's allowedTools list is missing the Task tool it needs to report back.
B. The coordinator is checking stop_reason when it should check end_turn.
C. The parent session has exceeded its context window during delegation.
D. The subagent does not inherit the parent's conversation history, so those facts were never visible to it.

---

**5.** `[task 1.6 · decomposition strategy]` A review pipeline must apply the same four fixed passes — lint, types, tests, style — to every pull request, with no discovery work between passes. Which decomposition pattern fits best?

A. A fixed prompt chain that runs the four passes in order for each pull request.
B. A dynamic plan that generates new subtasks from whatever each pass discovers.
C. A coordinator spawning one free-roaming subagent for every file under review.
D. A single combined prompt that reviews every aspect of all files in one pass.

---

**6.** `[task 1.7 · session resumption]` Last night's CI review session approved 22 test files for a payment service. This morning, six tests fail after a dependency upgrade. The agent's earlier commentary on the other 16 files is still valid. What should it do next?

A. Fork the session and run the old dependency version in parallel.
B. Resume the session and re-analyze only the six changed tests.
C. Keep the prior verdicts and ignore the six new failures for now.
D. Start a fresh session and re-read all 22 files from scratch.

---

**7.** `[task 1.2 · coordinator handoff]` A research coordinator spawns three subagents to investigate pricing, compliance, and reputation for a single supplier. Each subagent keeps asking the coordinator for the customer's country, even though the original request already stated it. Why is this happening?

A. The coordinator omitted the Task tool from the subagents' allowedTools list.
B. The model's context window is exhausted by the long supplier report.
C. The subagents are running in shared context and overwriting each other.
D. The subagents do not inherit the parent's conversation history.

---

**8.** `[task 1.7 · session forking]` A developer wants to try two different refactoring strategies on the same codebase baseline without affecting the main working session. Which action fits?

A. Resume the working session and apply the two strategies one after another.
B. Start a fresh session and re-run the entire codebase exploration from scratch.
C. Fork the session at the current baseline and test each strategy in its own branch.
D. Copy the project directory and open the duplicate in a second editor window.

---

**9.** `[task 1.2 · task decomposition breadth]` A coordinator researching 'how shipping regulation affects vaccine distribution' delegates three subtasks: refrigerated trucking rules, refrigerated van rules, and cold-storage warehouse rules. Every subagent executes well, yet the final report says nothing about air freight. What is the root cause?

A. The coordinator decomposed the topic too narrowly, omitting transport modes.
B. The synthesis agent merged the three findings without checking coverage gaps.
C. The search subagent issued queries too weak to surface air-freight regulation.
D. The subagents shared context and overwrote one another's findings partway.

---

**10.** `[task 1.4 · enforcement vs prompt]` A refund agent sometimes processes a refund before identity verification completes. The prompt says 'verify first' but compliance is inconsistent. What is the durable fix?

A. Add more examples of verified-first refunds to the system prompt.
B. Gate the refund tool so it cannot run until verification is true.
C. Increase the model temperature so it reasons more carefully.
D. Ask the model to double-check its work in a second pass.

---

**11.** `[task 1.5 · PostToolUse normalization]` Three MCP tools return timestamps as Unix epoch integers, ISO 8601 strings, and 'Mar 3, 2026' prose. The agent keeps mis-ordering events when it compares them. What is the most robust fix?

A. Tell the model in the system prompt that timestamp formats vary by tool.
B. Ask each of the three server maintainers to change their return format.
C. A PostToolUse hook that normalizes every tool result to one timestamp format.
D. Have the model convert each timestamp inline whenever a comparison comes up.

---

**12.** `[task 1.1 · stop_reason loop control]` A CI agent loops until it sees the text 'finished' somewhere in the assistant response. Several times it has stopped while a test tool was still running, leaving results orphaned. Which change fixes the loop control?

A. Wait longer for the string 'finished' to appear in the response.
B. Increase the maximum iteration count before giving up on the loop.
C. Require the assistant to say 'finished' twice in its reply.
D. Check stop_reason and continue while it reads tool_use.

---
