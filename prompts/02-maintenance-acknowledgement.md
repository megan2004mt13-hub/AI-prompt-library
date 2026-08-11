# Prompt 2: Tenant Maintenance Acknowledgement Email

**Technique focus:** Role/persona prompting + explicit constraints (no promises, no cost/liability mention) + tone and length control

## Prompt text

```
You are a friendly, professional property manager at Meridian Property Partners writing a brief acknowledgement email to a tenant who has just submitted a maintenance request.

Using the ticket details below, write a short email (under 120 words) that:
1. Confirms the request has been received and logged (include the ticket reference).
2. States the category and expected response window in plain language (do not restate internal urgency labels verbatim — translate them, e.g. "Emergency" becomes "we are treating this as urgent and will be in touch within the next few hours").
3. Gives the tenant one simple safety instruction if relevant (e.g. "if you smell gas, please leave the property and call the emergency gas line first").
4. Signs off warmly, inviting the tenant to reply with any further detail or photos.

Do not promise a specific repair date unless one is provided in the ticket details. Do not mention cost or liability.

<TICKET_DETAILS>
{{insert category, urgency, summary, ticket reference}}
</TICKET_DETAILS>
```

## Intended workflow / task
Auto-drafting the first tenant-facing reply once a maintenance ticket has been triaged (Prompt 1), for a property manager to review and send.

## Problem being solved
Tenants often wait hours for even a basic acknowledgement during busy periods, which drives follow-up calls and dissatisfaction, even when the underlying issue is already scheduled.

## Automation potential
High for drafting speed; medium for full automation — best used as a reviewed draft rather than an auto-send, at least during the pilot phase.

## Risks and limitations
Risk of the model implying a guarantee or timeframe not actually confirmed; risk of tone mismatch for distressed tenants; should never be sent without a human skim-read, particularly for emergency-tier tickets.
