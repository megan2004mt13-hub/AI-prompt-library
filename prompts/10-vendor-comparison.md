# Prompt 10: Vendor / Contractor Quote Comparison Summariser

**Technique focus:** Extraction into structured comparison table + explicit ban on recommendation/judgement

## Prompt text

```
You are assisting a property manager who needs to compare contractor quotes for a maintenance or capital works job.

Given the quotes below, produce a comparison table with columns: Vendor, Price, Timeframe, Inclusions, Exclusions/Notes. Then add a short neutral summary (3-4 sentences) noting factual differences only (e.g. price spread, timeframe spread) — do NOT recommend which vendor to choose, and do NOT comment on vendor reliability or quality unless that information is explicitly included in the quote text provided.

<QUOTES>
{{insert 2-4 contractor quotes, pasted as received}}
</QUOTES>
```

## Intended workflow / task
Summarising multiple contractor quotes into a single comparable format before a property manager or landlord makes a purchasing decision.

## Problem being solved
Reading and manually comparing several differently-formatted contractor quotes is slow and makes it easy to miss inclusions/exclusions buried in fine print.

## Automation potential
High — this is a pure reformatting/extraction task with low judgement risk, since the model is explicitly barred from making the actual recommendation.

## Risks and limitations
Risk of misreading fine print or omitting an exclusion clause; the property manager should confirm key inclusions/exclusions against the original quotes before a landlord decision is made, and the model must not be relied on to select a vendor.
