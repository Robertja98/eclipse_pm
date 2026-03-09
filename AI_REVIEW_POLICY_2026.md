# AI REVIEW POLICY 2026

---
**Document Version:** v1.0  
**Status:** Draft  
**Owner:** Founder  
**Approver:** TBD  
**Approval Date:** TBD  
**Last Updated:** 2026-03-09  

---

## 1. Purpose
Define confidence thresholds, approval requirements, and override rationale rules for AI-assisted data ingestion, analysis, and interpretation to maintain human-in-the-loop governance and audit defensibility.

## 2. AI Use Cases in System

### Data Ingestion Analysis
**Trigger:** Data collection records ingested (manual, instrument, or batch import).  
**AI Role:** Classify incoming data for trends, anomalies, or out-of-range values.  
**Output:** Flags and summary recommendations.

### Interpretation Support
**Trigger:** Experiment completion or user-requested analysis.  
**AI Role:** Generate preliminary technical interpretation of experimental results.  
**Output:** Analysis summary, confidence score, advancement suggestions.

### Anomaly Detection
**Trigger:** Real-time or scheduled batch analysis of data streams.  
**AI Role:** Identify measurements or patterns inconsistent with baseline or expected range.  
**Output:** Anomaly flag, severity, recommended action.

### Failure Prediction
**Trigger:** Experiment in progress or post-execution review.  
**AI Role:** Assess likelihood of failure condition based on historical patterns.  
**Output:** Failure flag, confidence score, suggested mitigation.

### Improvement Recommendations
**Trigger:** Post-experiment review or periodic optimization scan.  
**AI Role:** Propose process improvements, parameter adjustments, or efficiency opportunities.  
**Output:** Recommendation text, severity, expected impact.

## 3. Confidence Score Framework

### Score Range
- **0.00 - 1.00** (decimal scale)
- **0.00 - 0.49:** Low confidence
- **0.50 - 0.74:** Medium confidence
- **0.75 - 0.89:** High confidence
- **0.90 - 1.00:** Very high confidence

### Interpretation Guidelines
- **Low (<0.50):** AI output flagged for mandatory human review; not usable without override.
- **Medium (0.50-0.74):** AI output requires human review before acceptance; default action is "needs review."
- **High (0.75-0.89):** AI output may be provisionally accepted but should be reviewed within workflow.
- **Very High (≥0.90):** AI output may be auto-accepted for non-critical actions; manual review still recommended for claim-linked records.

## 4. Approval and Review Requirements

### Mandatory Human Review (No Auto-Accept)
**Applies to:**
- Any AI analysis linked to SR&ED or IRAP claim-critical experiments.
- Failure flags or anomaly detections with severity = "High" or "Critical."
- AI recommendations that suggest significant process changes or safety concerns.
- Any AI output with confidence score <0.50.

**Reviewer Requirements:**
- Must be a person with `sred_eligibility = 'Eligible'` or technical role.
- Reviewer must document decision: Accepted, Rejected, Deferred.
- Reviewer must provide rationale for rejection or modification.

### Recommended Human Review (Optional Auto-Accept)
**Applies to:**
- AI outputs with confidence ≥0.75 for routine data quality checks.
- Improvement recommendations with severity = "Low" or "Medium."
- Anomaly flags for non-critical measurements.

**Reviewer Requirements:**
- Review recommended but not blocking.
- If auto-accepted, log acceptance with timestamp and AI model version.

### Deferred Review Queue
**Trigger:** AI output marked "Needs Review" but not immediately blocking.  
**Process:**
- Outputs enter deferred queue.
- Reviewer processes queue periodically (daily or weekly per project policy).
- Outputs remain flagged until decision recorded.

## 5. Override and Rejection Rules

### Rejection Workflow
1. Reviewer examines AI output (summary, confidence, flags).
2. Reviewer determines AI output is incorrect, incomplete, or inappropriate.
3. Reviewer selects `reviewer_decision = 'Rejected'`.
4. Reviewer provides `reviewer_rationale` (free text, minimum 20 characters).
5. System logs rejection with timestamp and reviewer person ID.

### Acceptance Workflow
1. Reviewer examines AI output.
2. Reviewer confirms AI output is accurate and appropriate.
3. Reviewer selects `reviewer_decision = 'Accepted'`.
4. Reviewer optionally provides `reviewer_rationale` (recommended for claim-linked records).
5. System logs acceptance with timestamp and reviewer person ID.

### Deferred Workflow
1. Reviewer determines more information or time is needed.
2. Reviewer selects `reviewer_decision = 'Deferred'`.
3. Reviewer provides `reviewer_rationale` explaining what is needed or when re-review will occur.
4. Output remains in "Needs Review" status until re-reviewed.

### Modification Workflow
1. Reviewer accepts AI output concept but modifies details (e.g., adjusts recommendation text).
2. Reviewer selects `reviewer_decision = 'Accepted'` and provides modified output in separate field or comment.
3. System logs both original AI output and human-modified version for audit.

## 6. AI Model Governance

### Model Registration
- All AI models used must be registered in `ai_models` table.
- Required metadata: `provider`, `model_name`, `model_version`, `use_case`.
- Optional: `prompt_template_version`, `confidence_policy`.

### Model Versioning
- Each AI analysis run records `ai_model_id` to preserve traceability.
- Model upgrades require new `ai_model` record with incremented `model_version`.
- Historical analyses remain linked to model version used at time of execution.

### Model Deactivation
- If model performance degrades or policy changes, set `active_flag = false`.
- Deactivated models cannot be used for new analysis runs.
- Existing results remain valid for audit but flagged as using deprecated model.

### Prompt Template Versioning
- If AI relies on structured prompts, version prompts separately.
- Link prompt version to `ai_models` record via `prompt_template_version`.
- Changes to prompt templates require new model version entry.

## 7. Severity-Based Review Logic

### Severity Levels (AI Recommendations)
- **Low:** Minor optimization, non-blocking suggestion.
- **Medium:** Moderate improvement opportunity, may affect efficiency or quality.
- **High:** Significant issue or risk, should be addressed promptly.
- **Critical:** Safety concern, compliance risk, or experiment-blocking issue.

### Review Requirements by Severity
| Severity | Auto-Accept Allowed | Review Deadline | Approver Level |
|---|---|---|---|
| Low | Yes (if confidence ≥0.75) | Within 7 days | Any technical user |
| Medium | No | Within 3 days | Technical role or founder |
| High | No | Within 1 day | Founder or senior technical role |
| Critical | No | Immediate (same day) | Founder only |

## 8. Audit Trail and Traceability

### Required Logged Fields (per AI analysis run)
- AI model provider, name, version
- Input source (data collection record, experiment, batch)
- Trigger type (on ingest, scheduled, manual)
- Execution timestamp
- Confidence score
- Flags (anomaly, failure, improvement)
- Output summary
- Run status (completed, failed, needs review)

### Required Logged Fields (per AI recommendation)
- Recommendation type and severity
- Recommendation text (original AI output)
- Reviewer person ID
- Reviewer decision (accepted, rejected, deferred)
- Reviewer rationale
- Decision timestamp

### Export Requirements for SR&ED/IRAP Claims
- If AI was used in experiment interpretation, export must include:
  - AI model name and version
  - AI output summary
  - Human reviewer decision and rationale
  - Date of AI execution and review
- Narrative must state "AI-assisted analysis was reviewed and approved by [person] on [date]."

## 9. Data Quality Gates

### Pre-Analysis Validation
- AI analysis only runs on data that passes basic validation:
  - Required fields present
  - Values within min/max valid ranges (unless anomaly detection is the goal)
  - Proper units and formats
- Invalid data triggers manual review before AI processing.

### Post-Analysis Validation
- AI outputs checked for:
  - Confidence score present and within 0-1 range
  - Output summary not empty
  - Recommendation text not generic/boilerplate
- Failed validation triggers "Needs Review" status.

### Anomaly Flag Handling
- If AI sets `out_of_range_flag = true` on data collection record, human must review before data is used in claims.
- Anomalies may be valid measurements (e.g., unexpected breakthrough result) or errors (e.g., instrument malfunction).
- Reviewer documents whether anomaly is accepted as valid or data is flagged for exclusion/correction.

## 10. Training and Competency

### Reviewer Training Requirements
- Reviewers must understand AI output interpretation (confidence scores, flags, severity).
- Reviewers must know when to accept, reject, or defer.
- Reviewers must be trained on rationale documentation standards.

### System User Roles and AI Permissions
- **Founder / Admin:** Can review and decide on all AI outputs.
- **Technical Role:** Can review AI outputs for assigned projects/experiments.
- **Viewer Role:** Can view AI outputs but cannot approve or reject.

## 11. Continuous Improvement

### AI Performance Monitoring
- Periodically review AI acceptance vs. rejection rates.
- High rejection rates may indicate model drift, poor training data, or inappropriate use case.
- Low confidence scores across many runs may indicate model needs retraining or replacement.

### Feedback Loop
- Reviewer decisions and rationale feed back into AI model evaluation.
- Track cases where AI was wrong (rejected) vs. correct (accepted) to measure accuracy.
- Use feedback to refine prompts, confidence thresholds, or switch models.

## 12. Compliance Checklist

- [ ] AI models registered in `ai_models` table with version and use case.
- [ ] Confidence score thresholds configured per use case.
- [ ] Review workflow enforced for claim-critical AI outputs.
- [ ] Reviewer training completed for all technical users.
- [ ] Audit trail captured for all AI analysis runs and reviewer decisions.
- [ ] Export logic includes AI traceability fields when applicable.
- [ ] Anomaly and failure flag workflows tested and validated.
- [ ] Severity-based review deadlines communicated to team.

## 13. Open Decisions
- Confirm preferred AI model provider(s) and specific models for each use case.
- Confirm confidence threshold policy (use defaults above or customize per project).
- Confirm if external AI review (e.g., BDO consultant review of AI-assisted claims) is required.
- Confirm frequency of AI model performance review (quarterly recommended).

## 14. Next Steps
1. Map AI review policy to acceptance test cases in `TEST_STRATEGY_2026.md`.
2. Implement AI analysis workflow with confidence check gates in service layer.
3. Create reviewer interface for AI recommendation queue.
4. Schedule AI model registration and prompt template versioning as part of Sprint setup.
