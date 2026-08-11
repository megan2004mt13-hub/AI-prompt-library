# Prompt 4: Property Inspection Report Structuring

**Technique focus:** Grounding-only instruction (no fabrication) + fixed structured output + dual-output design + safety-priority extraction

## Prompt text

```
You are a property inspection assistant. Convert the property manager's raw, informal inspection notes below into a structured report. Use ONLY information present in the notes — never infer or invent condition details, measurements, or causes that are not stated. Where information for a section is missing, write "Not noted during this inspection."

Produce two outputs:

1. LANDLORD SUMMARY — headings: Property & Date, Overall Condition Rating (1-5, based only on stated observations), Room-by-Room Notes, Issues Identified, Recommended Actions.

2. INTERNAL MAINTENANCE TICKET LIST — a short bullet list of any issue that requires a trade/contractor, each tagged with the category used in the maintenance triage system (Plumbing, Electrical, Heating/Cooling, Appliance, Structural, Pest Control, Locks/Security, Landscaping, Other).

If any note mentions a potential safety hazard (mould, exposed wiring, structural damage, gas), list it first under "PRIORITY — SAFETY" in both outputs.

<RAW_NOTES>
{{insert inspection notes}}
</RAW_NOTES>
```

## Intended workflow / task
Turning a property manager's shorthand inspection notes (voice memo transcript or scribbled checklist) into a polished landlord report and an internal maintenance ticket list in one pass.

## Problem being solved
Writing up formal inspection reports from raw notes is one of the most time-consuming recurring tasks for property managers, and quality/detail varies significantly by individual writing speed and style.

## Automation potential
High — strong fit because the source material (notes) is supplied in full, minimising the model's need to infer or fabricate information.

## Risks and limitations
Risk of the model quietly filling gaps or over-interpreting vague notes despite instructions; mitigated by the explicit "use only stated information" rule and "Not noted" fallback, but a property manager must still proofread against the original notes before the report is sent to the landlord.
