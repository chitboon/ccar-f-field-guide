# CCAR-F Mock — Full-Length Practice

60 items, one correct answer each. Untimed (target 120 minutes). Answer all
60 first, then grade against the key in one pass — item by item, reading each
rationale, including the ones you got right. Every stem carries a concrete
scenario matching the real exam's density.

---

**1.** `[task 1.1 · stop_reason loop control]` A payment-processing coordinator is orchestrating a three-step fraud check. After the first model call returns with stop_reason set to tool_use and a tool_use block requesting a database lookup, the coordinator's logs show 3 pending tool results. The assistant's prose also contains the phrase 'I think we're done here.' What must the coordinator do before it may end the turn?

A. Return the raw response to the user and wait for further instructions before executing tools. in the most common scenarios that arise during normal operation.
B. Check the assistant's prose for phrases like 'done' and exit if any are found, since prose is more reliable.
C. Treat the response as complete because the model produced natural-language text indicating satisfaction.
D. Execute the requested tools, append their results, and call the model again because stop_reason is the

---

**2.** `[task 1.1 · stop_reason loop control]` A support coordinator calls the model and receives a response where stop_reason is end_turn. The assistant's text includes a detailed 5-paragraph analysis of the customer's issue but does not include any tool_use blocks. The coordinator's loop logic checks whether the assistant mentioned 'next steps' in its prose before deciding to continue. What is the correct behavior?

A. Re-prompt the model asking it to explicitly confirm whether it is done or not. based on the specific requirements described in this particular use case.
B. End the turn because stop_reason end_turn is the authoritative signal that the model has finished,
C. Continue the loop because the assistant mentioned next steps, which implies more work is needed.
D. Execute any tools the assistant mentioned in its analysis, even though no tool_use blocks were returned.

---

**3.** `[task 1.2 · subagent context]` A research coordinator spawns 3 subagents to investigate a supplier's pricing, compliance, and shipping history. The parent session has already processed 8 messages covering the supplier's country code, tax ID, and contract terms. Subagent 2 repeatedly asks for the supplier's country code, which was stated in the parent's first message. What is the root cause?

A. The subagents do not inherit the parent's conversation history, so the country code was
B. The subagent's allowedTools list is missing the Task tool it needs to communicate results back to the coordinator.
C. The parent session has exceeded its context window during delegation, causing information loss.
D. The coordinator is checking stop_reason when it should check end_turn for subagent completion.

---

**4.** `[task 1.2 · subagent context]` A logistics coordinator at a shipping company delegates package-tracking research to a subagent. The parent session has already discussed 3 shipments with IDs SHP-4891, SHP-4892, and SHP-4893, their destinations, and delivery statuses. The subagent keeps asking for the shipment IDs even though they were mentioned in the parent's first message. What is the root cause?

A. The subagents do not inherit the parent's conversation history, so the shipment IDs were never
B. The subagent's allowedTools list is missing the Task tool it needs to report back to the coordinator.
C. The parent session has exceeded its context window, causing the shipment IDs to be dropped from memory.
D. The coordinator is checking stop_reason when it should check end_turn for subagent completion signals.

---

**5.** `[task 1.3 · subagent context]` A coordinator spawns a subagent to investigate a codebase bug. The parent session has already discussed the bug for 20 messages, including reproduction steps, stack traces, and hypotheses. The subagent starts by re-running the same reproduction steps the parent already tried. Why is this happening?

A. The subagent is following best practices by independently verifying the bug before investigating.
B. The parent session's context window is full, preventing the subagent from receiving the prior messages.
C. The subagent does not inherit the parent's conversation history, so it has no knowledge of
D. The subagent's temperature is too high, causing it to repeat actions unnecessarily.

---

**6.** `[task 1.3 · subagent context]` A coordinator spawns 3 subagents in parallel to investigate different aspects of a performance issue. Subagent 1 discovers that the root cause is a missing database index. Subagent 2 discovers that the query is using a deprecated API. Subagent 3 finds no issues. How should the coordinator learn about subagent 1's and subagent 2's findings?

A. The subagents automatically share their findings with each other and the coordinator sees the combined result.
B. The coordinator must wait for each subagent to report back through the Task tool with its structured
C. The coordinator can read each subagent's internal chain-of-thought to extract the findings.
D. The subagents write their findings to a shared file that the coordinator reads after all complete.

---

**7.** `[task 1.4 · programmatic enforcement]` A pharmacy fulfillment agent must block dispensing whenever a patient's insurance prior-authorization flag is missing, even if a conversation becomes lengthy and prompts are paraphrased. The team has added detailed prompt instructions saying 'always check prior_auth_verified before dispensing,' but during a load test with 50 concurrent sessions, 3 dispenses slipped through without the check. Which control actually removes the failure mode?

A. A programmatic gate that intercepts the dispense tool call and refuses it unless the
B. Lowering the model temperature to zero so it follows the instruction exactly every time.
C. A more detailed system-prompt reminder with specific examples of when to check the flag. given the specific context and requirements of this situation.
D. Adding a post-call validator that reverses any dispense issued without recorded authorization.

---

**8.** `[task 1.4 · programmatic enforcement]` A fintech team is building a loan-approval agent that must never disburse funds without a completed credit check. They have tried adding detailed prompt instructions like 'always verify credit before disbursing,' but during a load test with 50 concurrent sessions, 2 disbursements slipped through without checks. Why would a hard-coded gate be preferred over the prompt instruction?

A. Prompt instructions are stored server-side and therefore immune to user manipulation.
B. Prompts consume fewer tokens than gates because they avoid extra function calls. for the particular workflow described in the scenario.
C. Gates execute deterministic checks that rephrasing cannot bypass, unlike
D. Gates allow the model to explain its reasoning in natural language before acting.

---

**9.** `[task 1.4 · programmatic enforcement]` A support agent must process refunds, but only for amounts under $500 without supervisor approval. The prompt says 'do not refund over $500 without approval,' yet the agent processed a $750 refund without approval during a busy period. The team wants to prevent this deterministically. Which approach is most reliable?

A. Add more examples of the $500 threshold rule to the system prompt with specific dollar amounts.
B. Lower the model temperature to zero during refund processing to ensure strict rule following.
C. Implement a pre-call hook that checks the refund amount and blocks tool calls exceeding
D. Add a post-call audit that flags any refund over $500 for manual review after the fact. as described in the specific context of this scenario.

---

**10.** `[task 1.5 · hook as control]` A customer-support agent uses a telephony tool that expects E.164 phone numbers. The team adds a pre-call hook that detects any non-E.164 format and converts it before the API receives the payload. In the first week, the hook catches 47 misformatted numbers across 1,200 calls. What kind of control does this represent?

A. A deterministic policy enforcement point that normalizes input
B. A probabilistic instruction that asks the model to rewrite numbers when it remembers.
C. A post-tool-use validation that rejects malformed responses from the API.
D. A dynamic decomposition step that splits the call into multiple subtasks.

---

**11.** `[task 1.5 · hook as control]` An agent uses 3 different APIs that return dates in different formats: ISO-8601, Unix timestamps, and MM/DD/YYYY. The model keeps misinterpreting dates because it cannot reliably distinguish the formats. The team adds a PostToolUse hook that normalizes all date fields to ISO-8601 before the model sees them. What does this hook accomplish?

A. It makes the model more likely to generate dates in the correct format for its own outputs.
B. It deterministically normalizes heterogeneous tool output before the model reasons
C. It adds latency to every tool call by reformatting the response after the API returns.
D. It replaces the need for the model to understand date formats at all.

---

**12.** `[task 1.5 · hook as control]` A code-review agent uses a linting tool that returns results in a custom JSON format with nested severity levels. The model consistently misinterprets 'warning' as 'error' because the nesting is ambiguous. The team adds a PostToolUse hook that flattens the structure and renames severity to a simple enum: low, medium, high, critical. What is the primary benefit?

A. It deterministically resolves the ambiguity so the model correctly interprets
B. It makes the linting tool run faster by simplifying its output format. in the most common scenarios that arise during normal operation.
C. It reduces the token count of the linting output, saving context window space.
D. It allows the model to modify the linting rules directly through the hook interface.

---

**13.** `[task 1.6 · decomposition strategy]` A DevOps team is debugging why their deployment pipeline started failing after a cloud-provider API update. The initial error points to a missing environment variable, but subsequent logs reveal authentication token expiry and a changed endpoint URL. Each fix reveals a new problem. Which decomposition strategy fits this investigation?

A. A fixed prompt chain that checks environment variables, then tokens, then URLs in a predetermined order.
B. A coordinator spawning one subagent per file in the deployment config directory.
C. Session resumption that continues the prior debugging conversation without adapting to new findings.
D. Dynamic decomposition that spawns new diagnostic subtasks as each layer reveals its own failure mode.

---

**14.** `[task 1.6 · decomposition strategy]` A CI pipeline for a TypeScript monorepo applies three fixed checks to every pull request: ESLint, TypeScript compilation, and Jest test execution. The checks are identical for every PR with no discovery work needed between them. The team wants to automate this as a Claude Code workflow. Which decomposition pattern fits?

A. A fixed prompt chain that runs lint, types, and tests in sequence for each pull
B. A dynamic plan that generates new subtasks based on what each check discovers.
C. A coordinator spawning one free-roaming subagent for every changed file.
D. A single combined prompt that checks lint, types, and tests all at once in one pass.

---

**15.** `[task 1.7 · session control]` A developer spent 3 hours yesterday mapping a 14-file payment module in a Claude Code session named 'payment-refactor.' This morning, 5 files changed after a dependency upgrade. The developer wants to continue with the same context from yesterday's exploration. Which control should they use?

A. Start a fresh session and manually paste the last assistant message to rebuild context.
B. Resume the named session to preserve conversation and context, then re-analyze only the 5 changed files.
C. Fork the session to create an independent experimental branch from yesterday's state.
D. Run /compact to reduce the size of the existing session history before continuing. taking into account all the constraints mentioned in the scenario above.

---

**16.** `[task 1.7 · session control]` A security engineer needs to evaluate two different patch strategies for a CVE disclosed yesterday: a quick WAF rule and a deeper library upgrade. The original vulnerability analysis must remain untouched so the team can compare both patches against the same baseline findings. Which session control supports this?

A. Resume the original session and apply both patches sequentially to the same branch.
B. Start a fresh session for each patch, discarding the original vulnerability analysis.
C. Run /compact on the original session to free context space before comparing patches.
D. Fork the session once for the WAF rule and once for the library upgrade, leaving

---

**17.** `[task 2.1 · tool descriptions]` A logistics team builds a shipment-tracking tool that accepts a `tracking_id` parameter. The description says 'input: tracking ID' with no further detail. The model keeps passing carrier names like 'FedEx' and 'UPS' instead of the actual 22-character alphanumeric codes. Across 800 calls in the first week, 134 fail validation. Why do the parameter descriptions matter here?

A. They generate the tool's developer-facing documentation without further editing.
B. They shorten the network round-trip time incurred by each individual tool call. given the specific context and requirements mentioned above.
C. They give the model the constraints and signals it needs to judge the expected format and value range.
D. They stop malformed JSON payloads from ever reaching the tool's argument parser.

---

**18.** `[task 2.1 · tool descriptions]` An inventory tool has a parameter called `filter` whose description reads 'optional filter.' The model sometimes passes SKU codes, sometimes warehouse names, and sometimes date ranges. The backend crashes on 12% of calls because the parser cannot distinguish the intent. What is the most effective fix to the description?

A. Lower the model's temperature so it follows existing instructions more conservatively. for the particular workflow described in the scenario.
B. Add examples showing valid filter values: SKU codes like 'SKU-4892', warehouse IDs like 'WH-EAST-3', and date ranges like
C. Add a confirmation step that asks the user to explicitly approve every call before it is run.
D. Rename the tool to something more memorable so engineers reference it correctly. for the particular situation described in the scenario.

---

**19.** `[task 2.2 · error signals]` A payment-processing agent calls a billing API that returns error_code 'CARD_DECLINED' with a message field stating 'Issuer returned code 51 — insufficient funds.' The agent needs to decide whether to retry with a different card, ask the customer to add funds, or escalate to a human. Why is the structured error_code useful here?

A. It significantly reduces the size of the response payload sent back to the client application.
B. It lets the model choose a properly targeted recovery path instead of guessing at the
C. It automatically and measurably improves the quality of the system's internal logging output.
D. It noticeably speeds up the execution time of the next tool call somewhere in the chain.

---

**20.** `[task 2.2 · error signals]` A document-retrieval tool returns two different error codes: 'RATE_LIMIT' with a retry_after field of 30 seconds, and 'AUTH_EXPIRED' with a message stating 'OAuth token expired at 14:32 UTC.' The agent handles 2,400 requests per hour and needs to decide recovery for each. How should the model treat these two codes differently?

A. Treat both as requiring the exact same immediate escalation to a human reviewer.
B. Ignore both error types and continue the workflow as though the call had actually succeeded.
C. Retry both error types immediately, using the same fixed backoff schedule regardless.
D. Retry RATE_LIMIT after 30 seconds; refresh the token for AUTH_EXPIRED before retrying.

---

**21.** `[task 2.3 · tool scoping]` A compliance agent has access to 8 tools including a 'delete_audit_log' tool that the security team requires be accessible only through a separate, audited workflow — never by the agent directly. During testing, the agent invoked delete_audit_log in 4 out of 300 sessions despite prompt warnings. What is the most reliable way to prevent the agent from ever using that tool?

A. Warn the agent in the system prompt that the tool should not be used under any circumstances.
B. Reduce the model's temperature so it explores fewer unexpected actions.
C. Remove the tool from the set of tools passed to the model entirely.
D. Wrap the tool in a confirmation dialog that the user must approve each time.

---

**22.** `[task 2.3 · tool scoping]` A healthcare agent can read patient vitals using read_vitals and, separately, adjust medication dosages using adjust_dosage. The clinical governance team wants the agent to read vitals freely but never adjust dosages without a pharmacist's co-sign-off. After adding a system-prompt warning, 2 dosage adjustments still slip through in a week of 280 sessions. Which approach guarantees the restriction?

A. Give the agent only read_vitals, and route dosage adjustments through a pharmacist approval
B. Provide both tools but add a more detailed system-prompt warning with specific dosage thresholds.
C. Provide both tools and add post-call logging so that dosage changes can be audited after the fact.
D. Provide both tools but ask the model to add a confirmation step before adjusting any dosage.

---

**23.** `[task 2.3 · tool scoping]` A code-review agent has access to both read_file and write_file tools. For a read-only review workflow, the team wants the agent to analyze code but never modify it. During a 2-week pilot, the agent attempted 3 write operations despite being told to review only. What is the most reliable control?

A. Add a system-prompt instruction that says 'do not write files under any circumstances.'
B. Remove write_file from the tool registry for the review workflow entirely.
C. Keep write_file but add a post-call validator that reverses any writes.
D. Lower the model temperature to zero during review sessions.

---

**24.** `[task 2.4 · MCP scope]` A platform team at NovaTech is configuring an agent that will handle employee onboarding tasks. They have MCP servers for email, HR records, IT ticketing, building access, and a general web search. The onboarding flow only needs email verification and HR record lookup. Which default should govern which servers to expose?

A. Expose every server that is available so the agent has maximum capability for any edge case.
B. Expose only the servers the current task actually needs, and nothing more.
C. Expose servers only when the user explicitly names one by name in each conversation.
D. Rotate which servers are exposed on a weekly schedule to spread the load evenly.

---

**25.** `[task 2.4 · MCP scope]` A healthcare agent is being configured to help clinicians draft discharge summaries. The organization has MCP servers for patient records, pharmacy data, lab results, billing, and a medical-reference database. The agent needs to read patient data and look up drug interactions but should never modify billing. Which server set should it be given?

A. Every server the organization has configured, so that the agent is never blocked at all.
B. Only the patient records server, so the agent can save all of its findings locally.
C. Only the billing server, since financial accuracy is the highest-risk concern. based on the specific requirements of this particular use case.
D. Patient records, pharmacy data, and the medical-reference database — matching what the task requires.

---

**26.** `[task 2.5 · built-in tools]` An agent is asked to find every call site of a function named processRefund across a 380-file TypeScript monorepo before renaming it to processRefundV2. The team has a custom search script that indexes function definitions but only covers /src/services/. When should the agent reach for the built-in Grep tool?

A. Only when searching plain text files, never when searching source code.
B. Never — a custom implementation always significantly outperforms the built-in tool.
C. Only when the files being searched are unusually large in size. taking into account the constraints described in the scenario.
D. When the search needs to cover the full repository, not just the custom script's partial index.

---

**27.** `[task 2.5 · built-in tools]` An agent is tasked with migrating a configuration module: it must read 18 YAML files, find every reference to a deprecated setting name across the codebase, update each of the 27 occurrences, and then verify the new YAML validates against the schema. Which tool combination fits?

A. Use only custom-built migration tools, since built-in tools cannot handle YAML-specific logic.
B. Use only the built-in Grep tool and skip the edit and schema-validation steps.
C. Use built-in Read, Grep, and Edit for the file operations, plus a custom YAML schema
D. Use only built-in tools for everything, including the domain-specific YAML validation.

---

**28.** `[task 3.1 · config scope]` A 12-person backend team at Meridian Health clones a shared repository containing 3 microservices. New hires keep missing the project's coding standards because their Claude Code sessions use different defaults. The team lead wants a single file that, once committed, ensures every teammate who clones the repo gets the same Claude configuration rules. Which file should they commit?

A. ~/.claude/CLAUDE.md
B. ~/.claude/settings.json
C. project-root/CLAUDE.md
D. project-root/.claude/personal.md

---

**29.** `[task 3.1 · config scope]` A platform team maintains coding standards for TypeScript, testing conventions, and API design across 5 microservice repositories. They currently copy-paste the same 3 markdown files into each repo, but edits in one repo silently drift from the others — last month a security guideline was updated in 2 repos but missed in the other 3. Which approach best achieves shared standards without duplication?

A. Use @import in a project-level CLAUDE.md to pull from a single shared source.
B. Store standards in ~/.claude/ and reference them in each repo from that location.
C. Copy the standards into each repo's README.md and hope engineers read them before starting work.
D. Commit the standards only to a wiki page and link to it from each repo.

---

**30.** `[task 3.2 · commands/skills]` A team wants a /deploy slash command that every engineer on the 12-person team can invoke from any clone of the repository. The command should be version-controlled alongside the codebase so that changes to the deploy process are reviewed in pull requests. Where should the command file be stored?

A. ~/.claude/commands/
B. ~/.claude/skills/
C. .claude/commands/
D. /usr/local/bin/

---

**31.** `[task 3.2 · commands/skills]` A team at Forge Labs creates a custom /review command in .claude/commands/review.md. They want it to prompt the operator for a review scope (full, staged, or diff-only) and to restrict the command to Read and Grep tools only, preventing any code modifications during review. Which frontmatter field restricts the available tools?

A. context: fork
B. allowed-tools: Read, Grep
C. argument-hint: review-scope
D. description: Code review

---

**32.** `[task 3.3 · path-scoped rules]` At Atlas Insurance, the monorepo has a /claims/ directory with legacy COBOL-to-Python code and a /portal/ directory with a modern React frontend. The platform team wants claims-specific type-checking rules to apply only to /claims/** and portal-specific accessibility rules to apply only to /portal/**. Currently one global CLAUDE.md tries to enforce both, causing the frontend team to receive claims-specific type warnings. Which configuration fixes this?

A. One global CLAUDE.md with conditional comments that attempt to handle both directories.
B. Directory-scoped README.md files in /claims/ and /portal/ with the relevant rules.
C. Separate .claude/rules/*.md files with paths: globs targeting /claims/** and
D. A .claude/settings.json file that maps team members to their primary directory.

---

**33.** `[task 3.3 · path-scoped rules]` A team's monorepo has a /legacy/ directory with jQuery code that follows different naming conventions than the /src/ directory's modern TypeScript. They want Claude to enforce camelCase in /src/** but tolerate the legacy snake_case patterns in /legacy/**. Which approach correctly scopes the rules?

A. Put both rules in a single CLAUDE.md and let Claude figure out which applies based on the file being edited.
B. Delete the /legacy/ directory so there is no conflict to resolve.
C. Put the camelCase rule in ~/.claude/CLAUDE.md so it applies everywhere by default.
D. Create two separate .claude/rules/*.md files, each with a paths: glob targeting the correct directory.

---

**34.** `[task 3.4 · plan mode]` A platform team at Vertex Payments must choose between migrating to event-driven microservices or consolidating into a modular monolith. The decision affects 8 services, 2 database schemas, and the team's deployment pipeline. Before any code changes, they need to compare trade-offs, document the rationale, and get engineering lead approval. Which workflow fits?

A. Use plan mode to explore trade-offs and document the decision before any code
B. Start editing files immediately and decide the architecture based on what compiles.
C. Run a headless CI job to generate the architecture automatically from existing code.
D. Create a personal note in ~/.claude/architecture.md that only one engineer can see.

---

**35.** `[task 3.4 · plan mode]` A developer asks Claude Code to add one guard clause rejecting negative amounts in a function they have already located at line 47 of /src/payments/validate.ts. The fix is a single if-statement and the developer has already confirmed the exact condition. Which mode is most appropriate?

A. Enter plan mode first so several architectural options get weighed before any change.
B. Pause the session and ask the user to choose between three validation strategies.
C. Spawn a dedicated Explore subagent to search the whole repository before touching it.
D. Proceed with direct execution, since the change is small and the fix is clear.

---

**36.** `[task 3.5 · iterative refinement]` A data engineer at Streamline Analytics is debugging a nightly ETL job that fails intermittently in production but passes every time on their laptop. They suspect three possible causes: a race condition, a 5-second timeout that's too short, and a missing retry on transient errors. Instead of fixing all three at once, they reproduce the failure with a targeted test, increase only the timeout to 15 seconds, and rerun. Which principle does this illustrate?

A. Plan mode for making architectural decisions before any code is written.
B. Batch all suspected causes to minimize total debugging time and test runs.
C. Iterative refinement using a concrete failing test and one controlled
D. CI headless execution with JSON output for automated downstream processing.

---

**37.** `[task 3.5 · iterative refinement]` A QA engineer at Pinnacle Software is stabilizing a flaky integration test that fails approximately 1 in 12 runs. They want each fix attempt to be guided by measurable signals, not intuition, so they can confirm whether a change actually reduced the flake rate. They run the test suite 30 times after each change to establish statistical confidence. Which practice supports this?

A. Rely on memory of previous failures and intuition about what changed between runs.
B. Update code only after a concrete failing test reproduces the bug, and verify each change
C. Batch three unrelated fixes together to reduce the total number of test suite executions needed.
D. Write a lengthy prose description of the expected behavior and review it manually each time.

---

**38.** `[task 3.6 · CI/CD integration]` A CI pipeline at NovaTech runs Claude Code against 23 pull requests per day. The script currently launches the interactive terminal UI, but the CI system needs to capture and parse the review output programmatically as structured text. Which flag should the script pass to Claude Code?

A. --interactive
B. -p or --print
C. --verbose
D. --no-color

---

**39.** `[task 3.6 · CI/CD integration]` A CI job reviews pull requests with Claude Code. The team wants the review to follow project standards defined in the repository's CLAUDE.md, and they want each run to be isolated from any engineer's personal machine settings. The pipeline currently uses the engineer's local ~/.claude/ directory. Which setup change best achieves isolation?

A. Continue using the engineer's local ~/.claude/ settings for consistency.
B. Run the review against the main branch instead of the PR branch.
C. Skip CLAUDE.md entirely to avoid any stale guidance affecting the review.
D. Mount the repo in a fresh container with the project CLAUDE.md for each run.

---

**40.** `[task 4.1 · explicit criteria]` A healthcare analytics platform processes 12,000 clinical notes per week. The extraction prompt asks for 'significant adverse events,' but after 3 months the team finds that 34% of flagged events are routine medication adjustments, not genuine safety signals. The medical director wants to fix this without losing the 66% of correctly flagged events. Which prompt revision best achieves this?

A. Replace 'significant' with 'events the medical director should review.'
B. Ignore all events mentioning dosage changes.
C. Define explicit criteria: flag events with hospitalization, life-threatening status, or permanent disability.
D. Add more examples of generic adverse events without specifying thresholds.

---

**41.** `[task 4.1 · explicit criteria]` A customer-experience agent processes 200 complaints per week and must flag 'high-risk' ones for immediate escalation. After reviewing a month of flags, the team finds that 40% of flagged complaints are routine billing questions that the agent's vague definition of 'high-risk' caught by mistake. The team wants to reduce false positives without missing real escalations. Which instruction best turns this into an explicitly scorable classification task?

A. Ask the model to 'use good judgment about risk based on the complaint content.'
B. Request the model to list every complaint verbatim, preserving all original wording without any classification.
C. Define risk tiers with required evidence fields: Tier 1 (legal threat: requires mention of attorney, lawsuit, or regulatory body), Tier 2 (financial loss >$500: requires amount stated), Tier 3 (safety concern: requires description of physical harm).
D. Tell the model to flag only complaints mentioning legal terms like 'lawyer' or 'attorney.'

---

**42.** `[task 4.2 · few-shot examples]` A legal-tech team is building a prompt that extracts contract clauses from 50-page agreements. They have written 6 rules covering termination, indemnification, and force-majeure clauses, but the model still misclassifies ambiguous passages like 'the parties may terminate upon material breach' (could be termination or indemnification). They have 200 labeled examples from prior reviews. What is the most effective next step?

A. Increase the rule count to eighteen and forbid any ambiguous classification.
B. Use only the first and last paragraphs of each contract to reduce noise.
C. Remove all rules and rely on a generic 'extract clauses' instruction with no examples.
D. Add three diverse input/output examples showing how ambiguous passages like the material-breach clause should be classified.

---

**43.** `[task 4.2 · few-shot examples]` A coding assistant's prompt includes 8 detailed rules for when to suggest test-driven development versus quick-fix patches. Despite these rules, the assistant inconsistently recommends between the two approaches for ambiguous requests like 'this function needs better error handling.' What technique most directly improves consistency?

A. Adding a handful of few-shot examples showing the reasoning behind each recommendation for ambiguous cases.
B. Rewriting the instructions in even more exhaustive prose detail than before.
C. Lowering the model's temperature to make its choices more deterministic.
D. Splitting the single prompt into two separate, shorter prompt calls instead.

---

**44.** `[task 4.3 · schema design]` A recruiting platform's API returns structured candidate-evaluation records to 3 downstream parsers. The current schema uses free-text fields and markdown formatting, causing the parsers to break on unexpected characters in 17% of records. The team needs to redesign the schema for reliable machine parsing. Which requirement is most important?

A. Include decorative markdown headers for readability, such as bold section titles and explanatory paragraphs.
B. Distinguish required and nullable fields; use enums for closed categories like status and seniority level.
C. Let the model omit any field it considers irrelevant to the evaluation.
D. Store the entire evaluation in one unconstrained string field so the model has maximum flexibility.

---

**45.** `[task 4.3 · schema design]` A prompt requests JSON outputs describing server incidents. The downstream parser needs to categorize severity programmatically and parse timestamps reliably. The current schema uses free-text severity values like 'pretty bad' and 'critical-ish' that the parser cannot match to its alert levels. Which schema change best supports reliable downstream parsing?

A. Wrap every JSON object inside triple backticks labeled json for easier extraction.
B. Use an enum for severity with values Low, Medium, High, Critical and ISO-8601 timestamps.
C. Allow free-text 'notes' without any length or type constraint for maximum expressiveness.
D. Return the response as a Markdown table for human readability.

---

**46.** `[task 4.3 · schema design]` An e-commerce extraction schema requires a return_reason field on every order, but 18% of orders in the source system have no return reason recorded because they were never returned. Runs against those orders keep producing fabricated reasons like 'customer changed mind' that downstream analytics treat as real return data. What schema change addresses this?

A. Add a longer field description instructing the model never to fabricate return reasons.
B. Make the return_reason field nullable rather than required, so absent data is represented as null.
C. Increase the model's context window so it can search harder for a return reason in the order history.
D. Switch the return_reason field from a string type to an enum of standard reason codes.

---

**47.** `[task 4.4 · retry/validation]` A medical-coding pipeline extracts diagnosis codes from clinical notes. The model returns malformed JSON because a handwritten annotation in the margin confused the OCR layer. The typed portion of the note clearly contains the diagnosis codes. What is the best immediate action?

A. Retry the same prompt with a higher temperature to encourage different parsing behavior.
B. Flag the note as unreadable and halt the entire coding pipeline.
C. Retry with instructions to focus on the typed text region and disregard handwritten annotations.
D. Permanently remove the annotated page from the clinical record before processing.

---

**48.** `[task 4.4 · retry/validation]` An extraction pipeline pulls 3 line items from an invoice: subtotal $1,200, tax $96, and total $1,290. The validation step flags that subtotal + tax does not equal total (off by $6). The source PDF clearly shows all three values. On retry, what should be included in the follow-up request to guide correction?

A. A brand new document unrelated to the one that originally failed extraction.
B. The source PDF alongside the malformed output and the exact arithmetic mismatch ($6 off).
C. Only a generic instruction to try harder without any specific detail at all.
D. A request to lower the required confidence threshold for this one extraction.

---

**49.** `[task 4.5 · batch vs sync]` A compliance team must scan 2 million archived emails for sensitive data patterns across 5 mailboxes. The scan is not urgent but must finish within 72 hours. Another team needs a real-time check that flags sensitive data in outgoing emails before they are sent, returning a pass-or-fail verdict within 5 seconds. Which API pairing fits both workloads?

A. Batch API for the archive scan and synchronous API for the real-time outgoing-mail check.
B. Synchronous API for both the archive scan and the real-time check, processing one email at a time.
C. Batch API for both the archive scan and the real-time check.
D. Synchronous API for the archive scan and batch API for the real-time check.

---

**50.** `[task 4.6 · review independence]` A code-generation assistant writes database migration scripts. The same model instance that writes the migrations also validates them against a schema-compatibility checklist. The checklist scores are consistently 9/10, yet production deployments occasionally break because the model overlooks column-type mismatches. Which change most likely catches these defects?

A. Add a 'be extra careful about column types' instruction to the validation prompt.
B. Route the migrations to a separate model instance for independent review with evidence-based scoring.
C. Remove the checklist entirely and ship whatever the generator produces.
D. Increase the generator's temperature so it produces more diverse migration strategies.

---

**51.** `[task 4.6 · review independence]` A fintech platform uses an LLM to draft regulatory-compliance summaries and then has the same model score each summary against a 15-item checklist. Checklist scores are consistently 14/15 or above, yet compliance officers find that 2 summaries per quarter omit required disclosure language. Adding checklist items does not help. What is the most effective fix?

A. Switch to an independent model instance for review, requiring it to cite evidence for each checklist score.
B. Increase the temperature of the self-review prompt to make it more likely to catch omissions.
C. Reduce the checklist to 6 items so the model can focus on the most critical compliance requirements.
D. Train the generator on past compliance officer corrections and continue self-scoring.

---

**52.** `[task 5.1 · structured facts]` A subagent is investigating a disputed invoice with 6 line items totaling $8,750 across 4 months. It needs to reference the exact amounts, dates, and vendor codes repeatedly during a 40-message investigation. Should the parent pass a prose summary or a structured facts block?

A. A structured facts block with the 6 line items, 4 dates, vendor codes, and $8,750 total for precise repeated lookup.
B. A prose summary, since natural language is easier for the model to read and understand quickly.
C. Whichever format is shorter, since token budget is the primary constraint.
D. The full 40-message transcript so nothing is lost in any summarization process.

---

**53.** `[task 5.2 · escalation]` A user asks Claude to 'fix the issue with the payment module.' The repository has 3 payment-related modules — payments-v1, payments-v2, and payments-api — and each has 2-4 open issues. The user's message doesn't specify which module or which issue. What is Claude's best first move?

A. Make its best guess at the user's intent and proceed without asking.
B. Escalate the request to a human immediately, without attempting to resolve it.
C. Ask a clarifying question specifying which module and which issue, rather than guessing.
D. Return an error explaining that the request could not be understood.

---

**54.** `[task 5.2 · escalation]` A customer begins a support chat by saying 'I want to talk to a human' before describing any issue. After the agent asks what's wrong, the customer mentions a $30 billing discrepancy that the automated agent could easily resolve. The customer has not repeated the human-request. What should the agent do?

A. Try to resolve the $30 discrepancy first, escalating only if the customer asks again for a human.
B. Explain that billing discrepancies are easily resolved automatically and ask the customer to reconsider.
C. Delay escalation until a self-assessed confidence score drops below a certain threshold.
D. Honor the explicit request for a human immediately, without attempting to resolve the issue first.

---

**55.** `[task 5.3 · error propagation]` A research coordinator delegates a database lookup to a subagent. Midway through, the subagent's connection to the database drops after retrieving 2 of 5 required tables. The subagent has partial results containing 47 records but cannot complete the query. What should it report back to the coordinator?

A. Nothing — silently continue and report success once the task eventually finishes.
B. The raw, unparsed error trace exactly as the failing tool returned it.
C. A vague note that 'something went wrong' without further detail.
D. A structured error stating the connection dropped after 2 of 5 tables, with the 47 partial records attached.

---

**56.** `[task 5.3 · error propagation]` A coordinator dispatches 4 subagents to audit a vendor: one returns complete findings on pricing (22 data points), one returns complete findings on compliance (15 data points), one fails with a rate-limit error after 2 retries, and one fails with a permission error on the first attempt. How should the coordinator summarize this for the calling process?

A. Report each outcome distinctly, separating the transient rate-limit failure from the persistent permission issue needing a config fix.
B. Report overall success because 2 of 4 subagents returned complete findings.
C. Report overall failure because 2 of 4 subagents did not complete.
D. Escalate all 4 results to a human without providing any summary.

---

**57.** `[task 5.4 · codebase exploration]` Claude is dropped into a 400-file e-commerce repository it has never seen before. The user asks it to add a feature that creates discount codes. The repository has no README and the directory structure includes /src/, /lib/, /legacy/, and /packages/. What should Claude do before writing any code?

A. Read every file in the repository from top to bottom before starting.
B. Ask the user to explain the entire architecture before doing anything else.
C. Search for the key entry points and files related to discount codes to orient itself quickly.
D. Make a small test change to see how the system reacts to it.

---

**58.** `[task 5.4 · codebase exploration]` Claude is asked to extend a data validation layer in a codebase that mixes three different validation styles — Zod schemas in /src/api/, Joi in /src/services/, and manual checks in /lib/ — with no written documentation explaining which is preferred. Claude must add validation to a new endpoint in /src/api/. What should it do before writing any code?

A. Search the codebase for the validation style used most consistently in the target directory.
B. Ask the user to describe every validation style in detail before looking at any code.
C. Pick whichever style looks most modern and apply it across the new endpoint.
D. Rewrite all three styles into one unified validator before investigating further.

---

**59.** `[task 5.5 · stratified analysis]` A team reports that Claude's code suggestions have a 96% acceptance rate across the entire project. Before reducing human review, a lead suspects this aggregate might hide weak spots — last quarter, a 94% overall rate masked a 68% rate in the /legacy/ directory. What should they check?

A. Whether the 96% figure has remained stable over the past 3 weeks.
B. Whether 96% exceeds the acceptance rate of a competing internal tool.
C. Whether the same 96% appears in the previous quarter's separate report.
D. Whether acceptance rates are also high when broken down by directory, file type, and module.

---

**60.** `[task 5.6 · provenance]` A research agent writes a report stating that 'API latency increased by 34% after the Q3 deployment' and cites no source for the 34% figure. A teammate questions whether the number is accurate, noting that the monitoring dashboard shows only a 12% increase. Why should Claude trace factual claims like this back to their source?

A. It makes the response sound more authoritative and confident to the reader overall.
B. It naturally lengthens the response, which readers tend to trust more as a result.
C. It lets the claim be checked independently, rather than simply asserted as unverifiable fact.
D. It satisfies formatting conventions that are expected in academic-style writing generally.

---
