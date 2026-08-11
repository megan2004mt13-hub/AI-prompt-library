# Iteration Log — Evidence of Prompt Refinement

Prompts were drafted, tested against realistic sample inputs, and revised to reduce inconsistency, hallucination, and liability risk. This log documents the version history for the four highest-risk or highest-impact prompts in the library. The remaining six prompts (2, 5, 6, 7, 8, 10) went through a lighter single-to-two-iteration cycle, mainly refining word count, tone, and adding explicit decision-boundary constraints once testing showed the model would otherwise volunteer opinions that should stay with staff.

---

## Prompt 1 — Maintenance Request Triage

| Version | Change | Issue identified / rationale |
|---|---|---|
| v1 | *"Categorise this maintenance request and tell me if it's urgent."* | Baseline draft. Produced inconsistent category labels between runs and no defined urgency scale, so outputs could not feed the ticketing system reliably. |
| v2 | Added a fixed category list, a 4-level urgency scale, and required JSON output. | Standardised output for downstream use, but the model occasionally invented categories outside the list for ambiguous requests, and safety-critical requests were sometimes under-classified as "Routine" if the tenant's own tone was calm. |
| **v3 (current)** | Added an "Other" fallback with mandatory explanation, hard override rules for safety keywords (gas, flooding, exposed wiring) that force Emergency classification regardless of tone, and two worked few-shot examples. | Closed the gap where calmly-worded but genuinely dangerous requests were under-triaged; few-shot examples reduced category drift by anchoring the model to concrete boundary cases. |

---

## Prompt 3 — Rent Arrears Escalation Sequence

| Version | Change | Issue identified / rationale |
|---|---|---|
| v1 | *"Write an email to a tenant who hasn't paid rent."* | Tone was inconsistent run-to-run — sometimes appropriately firm, sometimes accusatory — and with no day-count input, the model sometimes used legally-loaded language even for a one-day-late reminder. |
| v2 | Introduced tiered tone by days-in-arrears and an instruction not to present the message as a legal notice. | Reduced premature legal tone, but the model still occasionally referenced eviction as a general consequence in the highest tier — not acceptable for an LLM to state without legal sign-off. |
| **v3 (current)** | Replaced free-text Tier 3 legal content with a mandatory placeholder handing off to a human-approved legal template, and added an explicit ban on naming legislation or eviction. | Removes the highest-liability content from model generation entirely while still automating the lower-risk Tier 1–2 drafting, which is the majority of arrears volume. |

---

## Prompt 4 — Property Inspection Report Structuring

| Version | Change | Issue identified / rationale |
|---|---|---|
| v1 | *"Turn these notes into an inspection report."* | Output structure varied between runs (missing sections), and on sparse notes the model filled gaps with plausible-sounding but invented detail. |
| v2 | Fixed section headings, a defined 1–5 condition rating scale, and an explicit "use only stated information / write 'Not noted'" rule. | Stopped most fabrication, but property managers still had to manually extract a separate maintenance ticket list from the finished report, duplicating effort. |
| **v3 (current)** | Added a second structured output (internal maintenance ticket list, tagged to the same category system as Prompt 1) generated in the same pass, plus a "PRIORITY — SAFETY" extraction rule. | Removed the duplicate manual step and ensured safety-relevant findings could not get buried in a long room-by-room narrative. |

---

## Prompt 9 — Compliance Reminder Checklist Generator

| Version | Change | Issue identified / rationale |
|---|---|---|
| v1 | *"List what we need to comply with for rental properties this month."* | The model generated plausible-sounding but unverified and sometimes incorrect regulatory requirements and dates from its own training knowledge — an unacceptable liability risk for a compliance-facing tool. |
| v2 | Reframed the task as formatting only, requiring the user to supply all compliance items and dates rather than asking the model to originate them. | Removed most hallucination risk, but items with an incomplete source were still presented in the output with the same confidence as verified items. |
| **v3 (current)** | Added a mandatory "UNVERIFIED — confirm with compliance officer" flag for any item lacking a clear source in the input. | Ensures the checklist visually distinguishes confirmed from unconfirmed items, keeping a qualified human as the final authority on regulatory content. |

---

## Cross-library risk themes

Three risk themes recur across the library and are handled consistently rather than prompt-by-prompt:

- **Hallucination / fabrication** — mitigated by grounding prompts strictly in supplied data, and by explicit fallback instructions ("Not noted," "UNVERIFIED," "Insufficient data") rather than letting the model fill gaps.
- **Legal, financial and fair-housing liability** — mitigated by hard-coded negative constraints (no legislation citation, no eviction language, no eligibility judgement, no vendor recommendation) and mandatory human hand-off at the highest-risk tiers.
- **Over-automation of judgement calls** — every prompt that could materially affect a tenant, landlord, or legal outcome produces a reviewed draft or an internal flag, never an auto-sent final action.
