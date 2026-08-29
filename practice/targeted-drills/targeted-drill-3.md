# CCAR-F Targeted Drill 3 — Error Handling & Structured Facts

10 items, one correct answer each. Untimed. Answer all 10 first, then
grade against the key in one pass — item by item, reading each rationale,
including the ones you got right. This drill targets error propagation,
structured facts, retry/validation, and review routing.

---

**1.** `[task 5.3 · error report quality]` A search subagent queries a customer database for order #4892 and returns a generic status of 'search unavailable' after the query fails. The coordinator needs to decide whether to retry, use a cached result, or escalate to a human. What is the main problem with this kind of error report?

A. It hides the failure type and attempted query from the recovery decision.
B. It uses too many words for what should be a single short status code.
C. It takes longer for the subagent to generate than a successful response would.
D. It cannot be logged correctly by most standard monitoring dashboards.

---

**2.** `[task 5.3 · error differentiation]` A document analysis subagent queries a source and gets back zero matching passages, which is a legitimate outcome given the query. Separately, a different subagent's connection to its data source drops mid-query. How should these two situations be reported differently to the coordinator?

A. Both should be reported as errors, since neither subagent produced any content.
B. Report the empty result as valid and the connection drop as an access failure.
C. The empty result should be flagged as an error and the dropped connection ignored.
D. Both should be silently treated as successes so the coordinator is not interrupted.

---

**3.** `[task 5.3 · coverage gap handling]` A research coordinator delegates five subtopics to five subagents. One subagent cannot reach its assigned source at all, while the other four succeed and return usable findings. The coordinator must decide how to proceed with the final report. What is the best approach?

A. Abandon the whole report, since not every subtopic could be fully covered as planned.
B. Silently omit the fifth subtopic so the reader is unaware anything was missing.
C. Synthesize the four successful subtopics and mark the fifth as a coverage gap.
D. Retry all five subagents from scratch before synthesizing anything at all.

---

**4.** `[task 5.3 · local error handling]` A subagent hits a transient network timeout partway through its assigned task. Which behavior represents good local error handling before anything is escalated to the coordinator?

A. Attempt a local retry of the timed-out request before giving up on the subtask.
B. Silently return an empty result as if the subtask had simply found nothing.
C. Escalate to the coordinator immediately with no mention of the timeout.
D. Terminate the entire multi-agent workflow immediately to avoid any further errors.

---

**5.** `[task 5.1 · structured facts protection]` During a long multi-turn investigation spanning 60 tool calls, a coordinator keeps a running set of exact figures, dates, and identifiers — including 3 invoice totals, 5 dates, and 7 customer IDs — in a structured block separate from its summarized narrative history. What does this practice protect against?

A. The coordinator running out of available tools partway through the session.
B. The subagents disagreeing with each other about which source to trust.
C. The user asking for clarification more often than strictly necessary.
D. Numeric details being blurred or dropped when the narrative gets condensed.

---

**6.** `[task 5.1 · lost in the middle]` A synthesis agent receives a very long combined document from several subagents, with the most important findings buried in the middle sections. What risk does this position create?

A. The model may reliably process the start and end but miss the middle.
B. The document will fail to load at all once it exceeds a certain total length.
C. The subagents will be blocked from submitting any further findings afterward.
D. The coordinator will be charged extra for reading content placed in the middle.

---

**7.** `[task 5.1 · lost-in-the-middle mitigation]` A coordinator wants to mitigate the lost-in-the-middle effect while combining several subagent reports into one long input. Which practice directly helps with this?

A. Place key findings summaries near the beginning of the combined input.
B. Randomize the order of subagent reports on every single run.
C. Organize detailed results under explicit section headers.
D. Compress every subagent report into a single unlabeled paragraph.

---

**8.** `[task 4.4 · retry with context]` A structured extraction pulls 3 line items from an invoice: subtotal $1,200, tax $96, and total $1,290. The validation step flags that subtotal + tax ≠ total (off by $6). On retry, what should be included in the follow-up request to guide correction?

A. A brand new document unrelated to the one that originally failed extraction.
B. The original document, the failed extraction, and the validation error found.
C. Only a generic instruction to try harder without any specific detail at all.
D. A request to lower the required confidence threshold for this one extraction.

---

**9.** `[task 4.4 · retry limit]` An extraction pipeline keeps failing to find a required contract renewal date across several retries. Investigation shows the renewal date is simply never mentioned anywhere in the source document. What does this suggest about further retries?

A. One more retry with a stricter schema will very likely surface the missing date.
B. The retry count should be increased substantially until the field is finally found.
C. Retries will not help, since the required information is absent from the source.
D. A completely different extraction model would definitely resolve this specific case.

---

**10.** `[task 5.5 · stratified sampling]` A synthesis pipeline reports 96% field-level accuracy overall, and a manager wants to reduce human review of its output. Before doing so, what practice would reveal whether that headline figure is hiding a weaker segment?

A. Rechecking the same 96% figure again next quarter for consistency.
B. Comparing the 96% figure to a different team's overall accuracy number.
C. Asking the extraction model directly whether it is confident in its own accuracy.
D. Stratified random sampling of the extractions, broken down by document type and field.

---
