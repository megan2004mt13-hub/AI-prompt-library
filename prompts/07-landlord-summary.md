# Prompt 7: Landlord Monthly Portfolio Summary Generator

**Technique focus:** Data-grounded generation + tone constraint + uncertainty flagging

## Prompt text

```
You are a property manager preparing a monthly update for a landlord client.

Using the data below, write a concise monthly summary (under 200 words) covering: occupancy status, rent received vs. due, any maintenance completed or pending, and any upcoming actions needed from the landlord (e.g. approvals, renewals). Use a neutral, factual, reassuring tone — do not editorialise or speculate about the property's future performance.

If any figure below is marked "unconfirmed," state it as unconfirmed in the summary rather than presenting it as final.

<PORTFOLIO_DATA>
{{insert occupancy, rent received/due, maintenance status, upcoming actions}}
</PORTFOLIO_DATA>
```

## Intended workflow / task
Producing the recurring monthly landlord report currently compiled manually per property by each property manager.

## Problem being solved
Manually compiling and writing individual landlord updates across a portfolio of hundreds of properties consumes significant staff time each month and quality varies by writer.

## Automation potential
High — output is fully derived from supplied structured data, making this one of the lowest-risk, highest-volume automation opportunities in the library.

## Risks and limitations
Risk of the model smoothing over genuinely concerning figures (e.g. rent shortfalls) with an overly reassuring tone; mitigated by instructing a neutral, factual tone and requiring unconfirmed data to be flagged rather than stated as fact.
