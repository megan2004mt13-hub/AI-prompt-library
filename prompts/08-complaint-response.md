# Prompt 8: Tenant Complaint De-escalation Response Drafter

**Technique focus:** Constraint-based liability avoidance + repeat-incident escalation logic

## Prompt text

```
You are a senior property manager responding to an upset or frustrated tenant. Your goal is to de-escalate, acknowledge their frustration genuinely, and set a clear next step — without admitting fault or liability on Meridian's behalf.

Read the tenant's complaint below and write a response that:
1. Opens by acknowledging their frustration specifically (not a generic apology).
2. States one concrete next step and a realistic timeframe.
3. Avoids admitting fault, blaming the landlord, or promising compensation.
4. If the complaint mentions any safety issue, threat of legal action, or repeated unresolved issue (3rd+ time raised), add an internal flag: "ESCALATE TO SENIOR MANAGER — do not send tenant-facing reply without review."

<TENANT_COMPLAINT>
{{insert complaint text and complaint history if known}}
</TENANT_COMPLAINT>
```

## Intended workflow / task
Drafting first-response replies to escalated or emotionally charged tenant complaints for property manager review before sending.

## Problem being solved
Complaint responses are high-stakes and time-pressured; an inconsistent or defensive first response can turn a manageable complaint into a formal dispute or negative review.

## Automation potential
Medium — valuable as a fast, consistent first draft, but intentionally kept human-reviewed given the reputational and legal sensitivity of complaint handling.

## Risks and limitations
Risk of the model inadvertently admitting fault or making a promise the agency cannot keep; mitigated by explicit no-fault/no-compensation constraints and mandatory escalation flagging for repeat or legally-charged complaints; never suitable for auto-send.
