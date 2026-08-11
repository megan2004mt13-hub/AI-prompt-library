# Prompt 6: Prospective Tenant Enquiry Response & Pre-Qualification

**Technique focus:** Explicit decision-boundary constraint (no eligibility judgement) + compliance guardrail against protected-characteristic handling

## Prompt text

```
You are a leasing coordinator responding to a prospective tenant's enquiry about a rental listing.

Using the listing details and the enquiry message below:
1. Write a friendly response (under 130 words) answering any direct questions the enquirer asked, using only the listing details provided.
2. Extract any pre-qualification signals mentioned by the enquirer (move-in date, household size, pets, employment status) into a short internal note for the leasing team. Do not make any acceptance, rejection, or eligibility judgement yourself — output "For leasing team review" rather than a decision.
3. Do not ask for or reference protected-characteristic information (e.g. nationality, family status, disability, religion) even if the enquirer raises it — acknowledge their message politely and redirect to the listed criteria only.

<LISTING_DETAILS>{{insert}}</LISTING_DETAILS>
<ENQUIRY_MESSAGE>{{insert}}</ENQUIRY_MESSAGE>
```

## Intended workflow / task
First response to prospective tenant enquiries on listing portals, plus structured hand-off notes for the leasing team.

## Problem being solved
High enquiry volumes on popular listings mean slow first responses, which lose prospective tenants to faster-responding competitors; manual note-taking on each enquiry is inconsistent.

## Automation potential
High for the response draft; deliberately not automated for eligibility decisions, which stay entirely human-led for fair housing compliance.

## Risks and limitations
Fair housing / anti-discrimination risk is the primary concern — mitigated by explicitly banning the model from making or implying eligibility decisions and from engaging with protected-characteristic detail; agency must still train staff not to paste discriminatory content into the prompt in the first place.
