# Iteration Log — Evidence of Prompt Refinement

Every prompt in this library started as a short, plain-language first draft and was tested against realistic sample inputs. Drafts were revised where testing showed inconsistent output, hallucinated detail, tone problems, or a risk of the model making a decision that should stay with staff. This log records the initial draft and every subsequent version for all 10 prompts.

---

## Prompt 1 — Maintenance Request Triage

| Version | Change | Issue identified / rationale |
|---|---|---|
| v1 (initial draft) | *"Categorise this maintenance request and tell me if it's urgent."* | Baseline draft. Produced inconsistent category labels between runs and no defined urgency scale, so outputs could not feed the ticketing system reliably. |
| v2 | Added a fixed category list, a 4-level urgency scale, and required JSON output. | Standardised output for downstream use, but the model occasionally invented categories outside the list for ambiguous requests, and safety-critical requests were sometimes under-classified as "Routine" if the tenant's own tone was calm. |
| **v3 (current)** | Added an "Other" fallback with mandatory explanation, hard override rules for safety keywords (gas, flooding, exposed wiring) that force Emergency classification regardless of tone, and two worked few-shot examples. | Closed the gap where calmly-worded but genuinely dangerous requests were under-triaged; few-shot examples reduced category drift by anchoring the model to concrete boundary cases. |

---

## Prompt 2 — Tenant Maintenance Acknowledgement Email

| Version | Change | Issue identified / rationale |
|---|---|---|
| v1 (initial draft) | *"Write an email to the tenant acknowledging their maintenance request."* | No role framing, no length limit, no input structure. Output tone varied between overly casual and overly formal between runs, and the model sometimes invented a specific repair date or mentioned cost/liability when neither had been confirmed. |
| **v2 (current)** | Added property-manager role framing, a defined `<TICKET_DETAILS>` input block, a 120-word limit, a rule translating internal urgency labels into plain language, an optional safety instruction line, and explicit bans on promising a repair date or mentioning cost/liability. | Testing showed the model would fill gaps with plausible-sounding commitments (dates, reassurances about cost) whenever ticket details were incomplete — the explicit bans removed this risk while keeping the draft fast to review. |

---

## Prompt 3 — Rent Arrears Escalation Sequence

| Version | Change | Issue identified / rationale |
|---|---|---|
| v1 (initial draft) | *"Write an email to a tenant who hasn't paid rent."* | Tone was inconsistent run-to-run — sometimes appropriately firm, sometimes accusatory — and with no day-count input, the model sometimes used legally-loaded language even for a one-day-late reminder. |
| v2 | Introduced tiered tone by days-in-arrears and an instruction not to present the message as a legal notice. | Reduced premature legal tone, but the model still occasionally referenced eviction as a general consequence in the highest tier — not acceptable for an LLM to state without legal sign-off. |
| **v3 (current)** | Replaced free-text Tier 3 legal content with a mandatory placeholder handing off to a human-approved legal template, and added an explicit ban on naming legislation or eviction. | Removes the highest-liability content from model generation entirely while still automating the lower-risk Tier 1–2 drafting, which is the majority of arrears volume. |

---

## Prompt 4 — Property Inspection Report Structuring

| Version | Change | Issue identified / rationale |
|---|---|---|
| v1 (initial draft) | *"Turn these notes into an inspection report."* | Output structure varied between runs (missing sections), and on sparse notes the model filled gaps with plausible-sounding but invented detail. |
| v2 | Fixed section headings, a defined 1–5 condition rating scale, and an explicit "use only stated information / write 'Not noted'" rule. | Stopped most fabrication, but property managers still had to manually extract a separate maintenance ticket list from the finished report, duplicating effort. |
| **v3 (current)** | Added a second structured output (internal maintenance ticket list, tagged to the same category system as Prompt 1) generated in the same pass, plus a "PRIORITY — SAFETY" extraction rule. | Removed the duplicate manual step and ensured safety-relevant findings could not get buried in a long room-by-room narrative. |

---

## Prompt 5 — Lease Renewal Notice & Talking Points Generator

| Version | Change | Issue identified / rationale |
|---|---|---|
| v1 (initial draft) | *"Write a lease renewal email for this tenant."* | Single undifferentiated output. The model sometimes invented a rent-increase justification (e.g. "market rates have risen") with no supporting data, and email length was uncontrolled, running to several paragraphs. |
| **v2 (current)** | Split the output into two parts — a tenant-facing email (under 150 words) and a separate internal-only set of negotiation talking points — and added a rule that talking points must be grounded only in supplied market comparison data, with a fallback message ("Insufficient market data supplied…") when no comparables are given. | Testing showed the model would otherwise "invent" a market justification for a rent increase whenever comparison data was missing, which is a pricing-advice risk; separating tenant-facing content from internal prep also stopped negotiation notes accidentally being copy-pasted straight into tenant emails. |

---

## Prompt 6 — Prospective Tenant Enquiry Response & Pre-Qualification

| Version | Change | Issue identified / rationale |
|---|---|---|
| v1 (initial draft) | *"Respond to this prospective tenant's enquiry and tell me if they qualify."* | The model would directly state an eligibility opinion (e.g. "this applicant seems like a good fit") and, when an enquirer volunteered personal information such as family status, the model sometimes engaged with or commented on it directly — a fair housing / anti-discrimination risk. |
| **v2 (current)** | Removed all eligibility judgement from the model's task — it now outputs "For leasing team review" instead of a decision — and added an explicit instruction never to ask for, reference, or comment on protected-characteristic information even if the enquirer raises it themselves, redirecting instead to the listed criteria. | This is the highest fair-housing-risk prompt in the library outside of formal legal content, so the fix was to remove the judgement task from the model entirely rather than try to constrain a bad decision — the model now only extracts factual pre-qualification signals (move-in date, household size, pets) for a human to assess. |

---

## Prompt 7 — Landlord Monthly Portfolio Summary Generator

| Version | Change | Issue identified / rationale |
|---|---|---|
| v1 (initial draft) | *"Summarise this month's data for the landlord."* | No tone guidance, so summaries sometimes read as overly optimistic even when the underlying data showed a rent shortfall, extended vacancy, or overdue maintenance — softening bad news in a way that could mislead a landlord. |
| **v2 (current)** | Added a neutral, factual, non-speculative tone constraint, a 200-word limit, a fixed list of topics to cover (occupancy, rent received vs. due, maintenance, upcoming landlord actions), and a rule that any figure marked "unconfirmed" in the input must be stated as unconfirmed rather than presented as final. | Testing showed the model's default "reassuring" tone would understate genuinely concerning figures; the neutral-tone rule and unconfirmed-flagging rule were added so the summary reflects the data accurately rather than smoothing it over. |

---

## Prompt 8 — Tenant Complaint De-escalation Response Drafter

| Version | Change | Issue identified / rationale |
|---|---|---|
| v1 (initial draft) | *"Reply to this angry tenant complaint."* | The model's apologies sometimes read as an admission of fault on the agency's behalf, and it occasionally offered a specific remedy (e.g. "we'll waive next month's fee") that no one had authorised. It also had no way of flagging a complaint as serious or repeated. |
| **v2 (current)** | Added explicit constraints not to admit fault, blame the landlord, or promise compensation; required the response to acknowledge the tenant's specific frustration rather than a generic apology; and added a mandatory internal escalation flag for any complaint mentioning a safety issue, a threat of legal action, or a third-or-more repeat of the same issue. | The no-fault/no-compensation constraint removed the main liability risk, while the escalation flag ensures the highest-stakes complaints are routed to a senior manager rather than answered automatically. |

---

## Prompt 9 — Compliance Reminder Checklist Generator

| Version | Change | Issue identified / rationale |
|---|---|---|
| v1 (initial draft) | *"List what we need to comply with for rental properties this month."* | The model generated plausible-sounding but unverified and sometimes incorrect regulatory requirements and dates from its own training knowledge — an unacceptable liability risk for a compliance-facing tool. |
| v2 | Reframed the task as formatting only, requiring the user to supply all compliance items and dates rather than asking the model to originate them. | Removed most hallucination risk, but items with an incomplete source were still presented in the output with the same confidence as verified items. |
| **v3 (current)** | Added a mandatory "UNVERIFIED — confirm with compliance officer" flag for any item lacking a clear source in the input. | Ensures the checklist visually distinguishes confirmed from unconfirmed items, keeping a qualified human as the final authority on regulatory content. |

---

## Prompt 10 — Vendor / Contractor Quote Comparison Summariser

| Version | Change | Issue identified / rationale |
|---|---|---|
| v1 (initial draft) | *"Compare these contractor quotes and tell me which one to pick."* | The model would recommend a vendor, usually based on price alone, without reliably weighing inclusions/exclusions buried in the quote text — risking a property manager acting on an AI recommendation that missed a material exclusion clause. |
| **v2 (current)** | Removed the recommendation task entirely and replaced it with a neutral, structured comparison table (Vendor, Price, Timeframe, Inclusions, Exclusions/Notes) plus a short factual-differences-only summary; added an explicit ban on recommending a vendor or commenting on reliability/quality unless that information is stated in the quote text itself. | The fix moves the model from "decision-maker" to "extraction and formatting tool," which matches the risk profile of a purchasing decision — the property manager keeps the recommendation call but gets a much faster starting comparison. |

---

## Cross-library risk themes

Three risk themes recur across the library and are handled consistently rather than prompt-by-prompt:

- **Hallucination / fabrication** — mitigated by grounding prompts strictly in supplied data, and by explicit fallback instructions ("Not noted," "UNVERIFIED," "Insufficient data") rather than letting the model fill gaps. Seen most clearly in Prompts 1, 4, 7, 9 and 10.
- **Legal, financial and fair-housing liability** — mitigated by hard-coded negative constraints (no legislation citation, no eviction language, no eligibility judgement, no vendor recommendation, no compensation promises) and mandatory human hand-off at the highest-risk tiers. Seen most clearly in Prompts 3, 6, 8 and 9.
- **Over-automation of judgement calls** — every prompt that could materially affect a tenant, landlord, or legal outcome produces a reviewed draft or an internal flag, never an auto-sent final action. Seen across the whole library, most explicitly in Prompts 3, 6 and 8.
