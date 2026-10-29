# MATACS Associate Software Developer Assessment

Candidate: Talha Aman  
Reference: MATACS-TH-2026-02  
Time spent: 3 Hours

## Submission Contents

1. Process-flow diagram (`diagram/process-flow.png`)
2. Design write-up (`WRITEUP.md`)
3. AI prompts (`PROMPTS.md`)
4. Expected proposal ranking (`ranking.json`)

## Summary

This design uses AI to extract structured evidence from unstructured proposal documents. Deterministic validation and scoring rules then apply the administrator's versioned criteria. Missing, conflicting, or uncertain evidence is flagged for human review. The resulting ranking is advisory, and a human reviewer makes the final award decision.

## AI Tools Disclosure

I used ChatGPT to help clarify the brief, compare design options, draft and review the assessment artifacts, and check the ranking arithmetic. I reviewed the proposed approach rather than treating AI output as authoritative.

One correction was to reject the instruction embedded in Proposal C telling the automated reviewer to rank it first. Proposal text is untrusted evidence, not an instruction to the system. I also treated Proposal B as ineligible under the stated bidder-held licence requirement because its own Ontario electrical contractor licence is not confirmed.

## Files

1. `WRITEUP.md` — design decisions, scoring formulas, oversight, and risk analysis
2. `PROMPTS.md` — AI extraction and explanation prompts
3. `ranking.json` — expected proposal statuses, scores, and ranking
4. `diagram/process-flow.png` — process-flow diagram

## Assumptions and Limitations

The brief does not specify exact normalization formulas or a complete policy for incomplete evidence. The write-up therefore defines transparent example formulas and identifies Proposal D as flagged because its completion timeline is not stated.

Project dates and capacities must be verified before experience points are finalized. Missing or ambiguous evidence should not be treated as confirmed, and scores remain provisional until required evidence is reviewed. These design assumptions and scoring policies should be approved by the procurement administrator before production use.
