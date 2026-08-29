# CCAR-F Concept Drill — Domain 5: Context Management & Reliability

12 items, one correct answer each. Untimed. Answer all 12 first, then
grade against the key in one pass — item by item, reading each rationale,
including the ones you got right. This is a fast recall check for this domain,
not an exam simulator: the domain drills and mocks carry the scenario register.

---

**1.** `[task 5.1 · structured facts vs summarization]` A support case summary keeps shrinking critical order numbers and dates after compression. Which pattern preserves them?

A. Replace the summary with a longer narrative repeating the numbers.
B. Pin a case-facts block that is resent verbatim every turn.
C. Have the model memorize the numbers in its system prompt.
D. Increase the context-window limit before the compression step starts running.

---

**2.** `[task 5.2 · explicit escalation requests]` A customer opens with 'Just transfer me to a human agent' about a routine refund the agent could easily resolve on its own. What should the agent do?

A. Escalate immediately, without first investigating the refund.
B. Resolve the refund quickly, then offer an escalation to a human afterward.
C. Ask the customer once to reconsider their request before starting the escalation process.
D. Check the sentiment and escalate only if frustration is detected.

---

**3.** `[task 5.1 · position effects]` A research report buries the key finding in the middle of a 12-paragraph synthesis. The model misses it. What explains this?

A. The synthesis is too short, so the key finding has no room to appear.
B. The model cannot read numbered paragraphs and skips them entirely.
C. The finding was never present in any of the source documents at all.
D. Positional attention degradation weakens recall of middle context.

---

**4.** `[task 5.5 · aggregate vs stratified]` A validation pipeline reports 99.5% aggregate accuracy but every rejected Arabic document was misclassified. What is the oversight?

A. The aggregate is mathematically incorrect and needs to be fully recomputed.
B. The model needs more Arabic training data before it can be deployed safely.
C. Aggregate accuracy hides weak segments that per-segment reporting would expose.
D. Confidence thresholds are set uniformly too high for low-resource language documents.

---

**5.** `[task 5.3 · error propagation]` A web-search subagent times out midway through a three-source research task, having already retrieved two of the sources. What should it return to the coordinator?

A. Structured error context: the failure type, the attempted query, the two partial results, and alternatives.
B. An empty result set marked as successful, since two of the three sources were found.
C. The string 'search unavailable', letting the coordinator interpret the cause.
D. A raised exception that terminates the entire research workflow at that point.

---

**6.** `[task 5.6 · claim-source provenance]` A synthesis report states that a supplier breached contract in March according to multiple witnesses. Two sources say March 12, one says March 15, and one says the breach date is unconfirmed. The final report drops the source names and presents March 12 as a settled fact for the court filing. What is the main flaw?

A. The report should have chosen the earliest date to avoid confusing the readers of it.
B. The unconfirmed source should have been discarded before the synthesis began.
C. Conflicting sources were flattened into one unattributed claim.
D. The report should list every source only in an appendix for later reference.

---

**7.** `[task 5.6 · fact preservation]` A multi-agent research team synthesizes findings from 40 source documents into a quarterly review. Early drafts keep dropping the exact transaction amounts, contract dates, and party names, even though every subagent captured them accurately. Which pattern most reliably preserves these details in the final report?

A. Let each subagent write a longer narrative summary and concatenate them all together.
B. Carry a structured, schema-validated facts record forward verbatim into the report.
C. Require each subagent to cite source page numbers in every sentence it writes.
D. Increase the coordinator's output token limit before generating the report.

---

**8.** `[task 5.1 · position effects]` A litigation research agent reads 15 depositions in sequence and writes a single chronological summary for the case memo. During review, the legal team notices that crucial admissions from depositions 7 and 8 are routinely omitted, while facts from the first and last depositions are always recalled. Which diagnosis best explains the pattern?

A. Depositions 7 and 8 are simply longer than the other thirteen depositions.
B. The model cannot reliably process more than six documents in a single pass.
C. The summary writer is skipping the middle of the sequence quite intentionally.
D. The middle depositions suffer positional attention decay.

---

**9.** `[task 5.4 · context degradation]` Ninety minutes into exploring a legacy codebase, the agent stops citing the specific classes it found early on and starts talking about 'typical patterns' instead. What is the cheapest effective countermeasure?

A. Restart the exploration session from scratch whenever its answers start sounding generic.
B. Switch to a model with a larger context window for these long exploration tasks.
C. Have the agent keep a scratchpad file of key findings and re-read it as the session lengthens.
D. Lower the temperature so the later answers stay closer to the earlier ones.

---

**10.** `[task 5.2 · policy-gap escalation]` A customer calmly asks the support agent to match a competitor's lower price. Company policy covers only own-site price adjustments and says nothing about competitor matching. What should the agent do?

A. Escalate: the request falls into a policy gap, which is an escalation trigger regardless of sentiment.
B. Apply the own-site price-adjustment policy to this request, since it is the closest available rule on the books.
C. Resolve the case autonomously, because the calm sentiment signals a simple case.
D. Ask the customer to locate and quote the relevant policy text first.

---

**11.** `[task 5.5 · stratified review routing]` An invoice-extraction pipeline reports 96% field-level accuracy across 50,000 documents last quarter. Audit finds that 80% of all errors occur on scanned PDFs with handwritten totals, while typed PDFs are nearly perfect. Which practice would have surfaced the weak segment earliest?

A. A larger aggregate sample, so the error estimate tightens around the 96% figure.
B. Routing every extraction to human review, regardless of its document type.
C. Raising the confidence threshold uniformly across all of the document types.
D. Reporting accuracy per segment — document type, field, and confidence bucket — instead of one aggregate number.

---

**12.** `[task 5.4 · crash recovery]` A three-hour multi-agent codebase exploration dies when the coordinator process crashes. The team wants a restart to lose none of the findings. Which design achieves that?

A. Keep the terminal scrollback so the coordinator can re-read the whole session.
B. Each agent exports structured state to a known location; the coordinator loads that manifest on resume and injects it into agent prompts.
C. Rerun the exploration with longer tool timeouts so the crash does not recur next time.
D. Checkpoint by forking the session every hour into a differently named branch.

---
