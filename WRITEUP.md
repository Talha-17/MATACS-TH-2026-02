# AI Proposal Triage Design

## 1. What Should Be AI vs. Rules?

Use deterministic code for file type and size checks, schema validation, mandatory eligibility checks, weight validation, score calculations, sorting, and audit logging.

Use AI to extract structured facts and supporting evidence from proposals, identify missing or conflicting information, flag suspicious instructions, and draft explanations for reviewers. AI must not independently determine final scores, change eligibility rules, or make the final award decision.

Administrators configure criteria, weights, mandatory requirements, and scoring formulas by procurement category.

## 2. How Do You Ensure Consistency and Explainability?

Store each extracted field with its value, supporting quote or document location, and confidence or review status. Validate AI output against a strict JSON schema and reject unsupported values. Missing or conflicting information remains unknown until reviewed.

For experience, verify project dates and capacities against the required five-year window before awarding points. A total project count without sufficient date evidence must be flagged for verification rather than automatically treated as fully eligible experience.

For team and approach, award credit for each check only when the proposal contains supporting evidence. Missing or ambiguous evidence receives no automatic credit and is flagged for reviewer confirmation; it must not be interpreted as proof that the bidder lacks the capability.

Use deterministic scoring and a documented tie-break rule so identical validated inputs and configuration versions produce identical results. Record the model, prompt, extraction, criteria, and scoring versions.

Show reviewers each criterion's weight, normalized score, weighted contribution, supporting evidence, and any missing or conflicting information.

### Scoring Formulas

1. Experience (30%): `min(verified_eligible_project_count / 14, 1) × 100`. Count only commercial installations of at least 100 kW within the applicable five-year window. Verify project dates and capacities before scoring.
2. Price (25%): `(lowest eligible stated price / proposal price) × 100`. Use proposals that pass mandatory checks and have a stated price within budget. An over-budget proposal does not qualify as compliant for ranking.
3. Timeline (15%): `((16 − completion_weeks) / 16) × 100` for a stated timeline of 16 weeks or less. An unstated timeline is unknown, not zero, and requires review.
4. Warranty (15%): Average the panel warranty normalized against the highest sample value (25 years) and workmanship warranty normalized against the highest sample value (12 years).
5. Team and approach (15%): Four equally weighted evidence checks: named project lead, defined crew, safety plan, and monitoring plan. Award credit only when supporting evidence is present; flag unresolved evidence for review. Production configuration must define acceptable evidence for each check.

The price normalization baseline is the lowest stated price among proposals that pass mandatory checks and are within budget, including a proposal subsequently flagged because its timeline is unknown. This assumption must be documented and applied consistently.

## 3. How Do You Keep a Human in the Loop?

Require a reviewer to confirm extracted facts and mandatory checks before relying on the ranking. Flag missing, conflicting, low-confidence, or suspicious evidence for review.

Allow reviewers to correct extracted information, request clarification, and override recommendations with a recorded reason. The system presents an advisory ranking; a human makes the final award decision.

Maintain an audit log containing the file hash, uploader, timestamps, extracted fields and evidence, criteria version, prompt and model versions, rule and scoring versions, scores, rankings, flags, reviewer corrections, overrides and reasons, and final decision.

## 4. What If Criteria Change After Scoring?

Store criteria as versioned configuration by procurement category. Validate weights, mandatory requirements, and formulas before publishing a new version.

When criteria change, preserve the original results and rescore using the new version. Retain both sets of results and their audit history, clearly identifying which criteria version produced each ranking. Never silently change historical scores or compare rankings calculated under different versions as though they were equivalent.

## 5. What Are the Main Risks?

1. Extraction and OCR errors: Validate extracted fields and require evidence-backed review.
2. Hallucinations: Reject unsupported claims and retain source quotes or document locations.
3. Prompt injection: Treat proposal text as untrusted input. Ignore embedded instructions attempting to change scores or rankings.
4. Misleading claims: Verify licences, WSIB clearance, insurance, project references, and other required evidence with appropriate sources.
5. Bias and inconsistent treatment: Apply the same published criteria and deterministic formulas to all bidders.
6. Criteria changes and nondeterminism: Version configuration, prompts, models, and scoring logic; preserve audit records.
7. Privacy, security, and cost: Restrict access, protect uploaded documents, minimize retained data, and monitor AI usage.
8. Incomplete mandatory information: Do not silently assume compliance. Flag uncertainty for review and prevent unsupported eligibility decisions.

## Sample Decisions and Limitations

1. Northshore Solar (A): Passes the stated mandatory checks and has enough supplied information for a provisional score, subject to verification of project dates and evidence.
2. BrightPath (B): Ineligible under the stated rule because the bidder's own Ontario electrical contractor licence is not confirmed; the proposal says a licensed subcontractor would be used, with confirmation after award.
3. Lakeview (C): Passes the stated mandatory checks and can be provisionally scored. Its embedded instruction to rank the proposal first is treated as untrusted content and ignored.
4. Essex Green (D): Flagged and not ranked because its completion timeline is unknown. Other stated mandatory checks pass.
5. SunGrid (E): Passes the stated mandatory checks and has enough supplied information for a provisional score, subject to verification of project dates and evidence.

The assessment brief does not prescribe all formulas or missing-evidence rules. The formulas and assumptions above are proposed design choices and should be approved by the procurement administrator before operational use. Scores are provisional until the required evidence has been verified.
