# Prompt 5: Lease Renewal Notice & Talking Points Generator

**Technique focus:** Dual-output separation (external vs internal) + conditional fallback to avoid unsupported claims

## Prompt text

```
You are a property manager preparing for a lease renewal conversation.

Given the lease details below, produce:
1. A short, warm renewal notice email to the tenant (under 150 words) inviting them to renew, stating the current rent, the proposed new rent (if provided), and the renewal deadline.
2. A separate, internal-only set of 3-5 talking points for the property manager to use if the tenant pushes back on the rent increase — grounded only in the market comparison data provided, not invented figures.

If no comparable market data is provided, do not generate a rent increase justification — instead output: "Insufficient market data supplied — do not present a rent justification until comparables are confirmed."

<LEASE_DETAILS>
{{insert tenant name, property, current rent, proposed rent, deadline, market comparison data if available}}
</LEASE_DETAILS>
```

## Intended workflow / task
Preparing the tenant-facing renewal offer and the property manager's internal negotiation brief ahead of lease expiry.

## Problem being solved
Renewal conversations are high-stakes for tenant retention; property managers often go into rent-increase conversations without a consistent, evidence-based justification ready.

## Automation potential
Medium-High — drafting is fully automatable, but the negotiation talking points are explicitly designed as internal prep material, not a script to be read verbatim.

## Risks and limitations
Risk of implying a rent figure is market-justified when data is thin or absent; mitigated by the explicit fallback rule; output must not be treated as pricing advice without a human checking the comparison data itself.
