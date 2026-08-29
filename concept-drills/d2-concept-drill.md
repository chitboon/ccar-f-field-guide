# CCAR-F Concept Drill — Domain 2: Tool Design & MCP Integration

12 items, one correct answer each. Untimed. Answer all 12 first, then
grade against the key in one pass — item by item, reading each rationale,
including the ones you got right. This is a fast recall check for this domain,
not an exam simulator: the domain drills and mocks carry the scenario register.

---

**1.** `[task 2.1 · tool description quality]` An agent confuses lookup_ticket with lookup_account — both described only as 'Looks up a record' — and calls the wrong one in 15% of turns. What is the highest-leverage first fix?

A. Add a routing classifier that parses each incoming request and pre-selects the tool.
B. Rewrite each description with input formats, example queries, and boundaries versus the other tool.
C. Merge both into a single lookup_record tool that accepts any kind of identifier.
D. Put worked examples of the correct tool choices into the system prompt.

---

**2.** `[task 2.2 · structured error signal]` A refund MCP server returns a valid empty result for ineligible orders but also returns 'not found' as a plain string for missing orders. The agent treats both as failures and escalates. What should the server do?

A. Always return a non-empty placeholder result for every ineligible order that arrives.
B. Change the plain string to 'success' for the ineligible orders it is given.
C. Escalate every ineligible order to a human operator automatically, every time.
D. Return structured metadata distinguishing valid-empty results from errors.

---

**3.** `[task 2.4 · MCP server scope]` A team wants every member to get the same Jira MCP server when they clone the repo, while one developer experiments with a personal scratch server nobody else should see. Which configuration achieves both?

A. The Jira server in the project .mcp.json; the scratch server in that developer's ~/.claude.json.
B. Both servers in .mcp.json, with the scratch server marked as private in a comment beside its entry.
C. Both servers in ~/.claude.json, with onboarding docs telling everyone to copy the Jira block over.
D. The Jira server declared inside CLAUDE.md so that it travels together with the repository docs.

---

**4.** `[task 2.2 · error categories]` An MCP server returns either a permission refusal or a transient gateway timeout as the same plain string 'failed.' What is the chief problem?

A. The server should retry every failure up to the configured maximum number of times.
B. The error message should be logged at debug level and hidden from the coordinator.
C. The coordinator cannot tell retryable transient errors from non-retryable permission refusals.
D. The gateway timeout should be suppressed and reported as a successful call.

---

**5.** `[task 2.5 · built-in tool selection]` Before renaming processOrder( to processOrderV2( with an added region parameter, you must list every file that calls the function. Which tool produces that list?

A. Glob with the pattern **/*processOrder* to find the files whose names match it.
B. Grep for 'processOrder(' across file contents, then Read each match.
C. Read every source file in the repository and note the callers by hand.
D. Glob with **/*.ts to enumerate the whole source tree, then skim each file.

---

**6.** `[task 2.3 · scoped tool access]` A code-review agent has access to 18 MCP tools spanning linting, testing, security, and deployment. During turns that should only lint changed files, it frequently calls a deployment tool. Which intervention directly prevents the role mismatch?

A. Scope the linting agent's allowedTools to lint and read tools.
B. Force tool_choice 'any' on every linting turn so a tool is always used.
C. Add five detailed examples of correct tool selection into the prompt.
D. Increase the verbosity of every tool description across the registry.

---

**7.** `[task 2.4 · MCP resources mechanism]` An agent burns 12 exploratory tool calls just to learn what a document store contains. Exposing the store's catalog as an MCP resource makes this faster. Why, mechanistically?

A. Resources are cached on the server side, so each catalog read returns sooner than a tool call.
B. Resource payloads are compressed by default, so less data crosses the wire on each read.
C. Tool calls are billed per invocation while resource reads from the client go unmetered.
D. A resource is read directly by the client with no model turn, replacing tool calls that each cost a reasoning round-trip.

---

**8.** `[task 2.3 · scoped tool access]` An extraction pipeline has 30 tools but each document type only needs three. Agents sometimes pick tools from unrelated roles. Which fix best targets the misuse?

A. Force tool_choice 'any' on every turn so that some tool is always called.
B. Improve the descriptions of every tool listed in the shared registry.
C. Scope each agent's allowedTools to its role and one narrow crossover.
D. Add a post-call validator that rejects off-role tool selections before they run.

---

**9.** `[task 2.4 · credential management]` A shared .mcp.json needs a GitHub token for the team's MCP server, and the file is committed to git. How should the token be supplied?

A. Commit the file with a placeholder token and ask each developer to substitute the real value locally.
B. Reference ${GITHUB_TOKEN} in the config; the real value stays in each developer's environment.
C. Store the token inside CLAUDE.md, which already travels together with the repository.
D. Base64-encode the token inside .mcp.json so that it is not plainly readable there.

---

**10.** `[task 2.1 · system-prompt keyword effects]` Right after the system prompt gains the sentence 'Always verify information carefully,' the agent starts calling its verify_claim tool even for trivial lookups that never needed it. What is the most likely cause?

A. The keyword 'verify' in the instruction created an unintended association with the verify_claim tool.
B. The verify_claim tool got registered twice in the server, which doubled its selection weight at runtime.
C. The same deployment raised the model's temperature, which loosened its tool choices overall.
D. The tool's own description was accidentally truncated when the new prompt version shipped.

---

**11.** `[task 2.4 · tool discovery verification]` After a second MCP server is added to the project config, the agent never calls any of its tools, though the file parses correctly. What should you verify first?

A. That the model session was restarted with a fresh system prompt after the config edit.
B. That the new server's tools are listed ahead of the first server's entries in .mcp.json.
C. That the server's tools were discovered at connection time and appear in the available tool list.
D. That the new server's tools use names that differ from every built-in tool's name.

---

**12.** `[task 2.3 · tool_choice configuration]` An enrichment pipeline corrupts records whenever any tool runs before extract_metadata. Which configuration guarantees extract_metadata runs first?

A. Set tool_choice to 'any' so that the model must call some tool on the first turn.
B. List extract_metadata first in the tools array so that the model sees it first.
C. Instruct the model in the system prompt that extract_metadata must always be called before any other tool.
D. Force the first turn with tool_choice naming extract_metadata; later steps run in follow-up turns.

---
