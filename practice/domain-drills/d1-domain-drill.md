# CCAR-F Domain Drill — Domain 1: Agentic Architecture & Orchestration

10 items, one correct answer each. Untimed. Answer all 10 first, then
grade against the key in one pass — item by item, reading each rationale,
including the ones you got right. This is a scenario-based drill for this
domain, not a recall check: every stem carries a concrete situation you
must reason about.

---

**1.** `[task 1.1 · stop_reason loop control]` A payment-processing coordinator is orchestrating a three-step fraud check. After the first model call returns with `stop_reason` set to `tool_use` and a tool_use block requesting a database lookup, the coordinator's logs show 3 pending tool results. What must the coordinator do before it may end the turn?

A. Execute the requested tools, append their results, and call the model again.
B. Check the assistant's prose for phrases like 'I am done' and exit if any are found.
C. Treat the response as complete because the model produced natural-language text.
D. Return the raw response to the user and wait for further instructions.

---

**2.** `[task 1.2 · coordinator handoff]` A research coordinator spawns a subagent to investigate a supplier's shipping history. The parent session has already processed 8 messages covering pricing, compliance, and reputation data. The subagent repeatedly asks for the supplier's country code, which was stated in the parent's first message. What is the root cause?

A. The subagent's allowedTools list is missing the Task tool it needs to communicate results back to the coordinator.
B. The subagents do not inherit the parent's conversation history, so those facts were never visible to it.
C. The parent session has exceeded its context window during delegation.
D. The coordinator is checking stop_reason when it should check end_turn.

---

**3.** `[task 1.3 · subagent context isolation]` A billing coordinator delegates a disputed invoice to a dedicated Collections subagent. The parent has already negotiated a partial payment plan with the customer over twelve messages. What is the safest way to hand off the essential negotiation context?

A. Let the subagent query the parent session for any message it needs.
B. Forward the complete twelve-message transcript so the subagent sees every turn.
C. Pass a work order with the payment plan, dispute reason, and constraints.
D. Embed the entire parent system prompt into the subagent's instructions.

---

**4.** `[task 1.4 · programmatic enforcement]` A pharmacy fulfillment agent must block dispensing whenever a patient's insurance prior-authorization flag is missing, even if a conversation becomes lengthy and prompts are paraphrased. Which control guarantees the block regardless of prompt wording?

A. A programmatic gate that refuses the dispense call unless the `prior_auth_verified` flag is true.
B. A system-prompt reminder that the model should check prior authorization before dispensing.
C. Lowering the model temperature so it follows instructions more conservatively.
D. Allowing the pharmacist to override the flag by asking in natural language.

---

**5.** `[task 1.4 · programmatic gate vs prompt]` A fintech team is building a loan-approval agent that must never disburse funds without a completed credit check. They have tried adding detailed prompt instructions like 'always verify credit before disbursing,' but during a load test with 50 concurrent sessions, 2 disbursements slipped through without checks. Why would a hard-coded gate be preferred over the prompt instruction?

A. Prompt instructions are stored server-side and therefore immune to user manipulation.
B. Prompts consume fewer tokens than gates because they avoid extra function calls.
C. Gates execute deterministic checks that rephrasing cannot bypass.
D. Gates allow the model to explain its reasoning in natural language before acting.

---

**6.** `[task 1.5 · hook as deterministic control]` A customer-support agent uses a telephony tool that expects E.164 phone numbers. The team adds a pre-call hook that detects any non-E.164 format and converts it before the API receives the payload. In the first week, the hook catches 47 misformatted numbers across 1,200 calls. What kind of control does this represent?

A. A deterministic policy enforcement point that normalizes input around a tool call.
B. A probabilistic instruction that asks the model to rewrite numbers when it remembers.
C. A post-tool-use validation that rejects malformed responses from the API.
D. A dynamic decomposition step that splits the call into multiple subtasks.

---

**7.** `[task 1.6 · decomposition strategy]` A data-platform team at a retail analytics company is investigating why nightly ETL jobs began failing after a cloud-provider maintenance window. The initial logs point to authentication, but later traces suggest rate-limit changes and a possible schema drift in an upstream partner feed. New hypotheses keep emerging as engineers examine each layer in sequence. Which task decomposition strategy is most appropriate?

A. Prompt chaining that executes a fixed sequence of predetermined diagnostic steps.
B. Dynamic decomposition that spawns subtasks based on signals discovered during the investigation.
C. Session resumption that continues the prior day's debugging conversation unchanged.
D. Forking the session so two engineers can run identical checks in parallel.

---

**8.** `[task 1.6 · dynamic decomposition]` A support triage bot receives a vague complaint about a mobile app crash. It first checks the device model, then decides whether to inspect logs, request a screen recording, or route to a human based on what it finds. Which pattern is this?

A. Prompt chaining with a fixed predetermined sequence of triage steps.
B. Dynamic decomposition that chooses next steps based on discovered information.
C. A programmatic gate that blocks all crashes until logs are provided.
D. Subagent context isolation that hides prior turns from the triage bot.

---

**9.** `[task 1.7 · session resumption]` A developer spent 3 hours yesterday mapping a 14-file payment module in a Claude Code session named 'payment-refactor.' This morning, 5 files changed after a dependency upgrade. The developer wants to continue with the same context from yesterday's exploration. Which control should they use?

A. Start a fresh session and manually paste the last assistant message.
B. Fork the session to create an independent experimental branch.
C. Resume the named session to preserve conversation and context.
D. Run /compact to reduce the size of the existing session history.

---

**10.** `[task 1.7 · session forking]` A security engineer at a fintech firm is using Claude Code to evaluate two divergent remediation plans for a newly disclosed vulnerability: a quick runtime patch and a more thorough library refactor. The original investigation must remain untouched so the team can later compare both approaches against the same baseline. Which session control best supports this workflow?

A. Resume the original session so the baseline context is preserved while evaluating plans.
B. Fork the session once for the runtime patch and again for the library refactor.
C. Start a fresh session for each remediation plan to avoid any shared history.
D. Run /compact on the original session before every comparison.

---
