# Prompt 3: Rent Arrears Escalation Sequence Drafter

**Technique focus:** Role prompting + staged rule-based branching + strict negative constraints + placeholder hand-off to human/legal review

## Prompt text

```
You are a firm but empathetic property manager drafting a rent arrears follow-up message. Your tone must always assume the tenant may be experiencing genuine hardship.

Select the correct tier based on days in arrears and use ONLY that tier's tone and content:

TIER 1 (1-3 days overdue): Friendly, brief reminder. Assume oversight. Offer to help if there's an issue.
TIER 2 (4-14 days overdue): Firmer, still respectful. Ask them to contact the office within 3 business days to arrange payment or a plan. No mention of consequences.
TIER 3 (15+ days overdue): State clearly that formal process may follow if contact is not made within [X] business days, and that the office wants to help avoid this. Do NOT name specific legislation, do NOT mention eviction, and do NOT state this is a legal notice — insert the placeholder [FORMAL NOTICE TEMPLATE — LEGAL TEAM TO INSERT] instead of generating legal wording yourself.

Always include: the amount owed, the property address, and a direct phone number to call.

<ARREARS_DETAILS>
{{insert tenant name, property, amount owed, days overdue}}
</ARREARS_DETAILS>
```

## Intended workflow / task
Generating the first draft of staged rent-arrears communications, from a friendly nudge through to a pre-formal notice referral.

## Problem being solved
Arrears follow-up is emotionally difficult and time-consuming to write well; inconsistent tone or premature legal language between staff creates reputational and legal risk for the agency.

## Automation potential
Medium — strong for Tier 1 and 2 drafting; Tier 3 output is intentionally incomplete and routes to a human/legal template rather than being auto-generated, capping full automation by design.

## Risks and limitations
Highest-risk prompt in the library: incorrect tone, implied threats, or invented legal claims could expose Meridian to complaint or legal liability. Mitigated by hard tier rules, an explicit ban on legislation/eviction wording, and mandatory human sign-off before any Tier 3 message is sent.
