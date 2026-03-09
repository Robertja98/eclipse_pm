# DATA VALIDATION RULES 2026

---
**Document Version:** v1.0  
**Status:** Draft  
**Owner:** Founder  
**Approver:** TBD  
**Approval Date:** TBD  
**Last Updated:** 2026-03-09  

---

## 1. Purpose
Define required fields, units, ranges, and provenance metadata for data collection to enforce data quality gates before export to SR&ED/IRAP reports.

## 2. Data Quality Principles

### Completeness
- All required fields must be populated before record is marked "valid."
- Optional fields may be empty but should be flagged if commonly expected.

### Consistency
- Units must be consistent within a project unless explicit conversion metadata is stored.
- Metric names should follow controlled vocabulary or project-specific naming convention.

### Accuracy
- Values must fall within defined valid ranges (min/max) unless flagged as anomaly.
- Out-of-range values require human review and justification before inclusion in claims.

### Provenance
- Source of data (manual entry, instrument, imported file) must be recorded.
- Operator/collector person must be linked.
- Timestamp of observation must be captured.

### Traceability
- Data must link to experiment, project, and optionally facility space and equipment.
- Version context preserved if data linked to specific experiment version.

## 3. Required Fields (Data Collection Records)

| Field | Required | Validation Rule | Error Message if Missing |
|---|---|---|---|
| project_id | Yes | Must reference valid project | "Data collection must be linked to a project" |
| experiment_id | Yes | Must reference valid experiment | "Data collection must be linked to an experiment" |
| source_type | Yes | Must be Manual, Instrument, or Imported | "Source type must be specified" |
| metric_name | Yes | Non-empty string, max 120 characters | "Metric name is required" |
| metric_value | Yes | Numeric (DECIMAL), finite | "Metric value must be a valid number" |
| metric_unit | Yes | Non-empty string, max 40 characters | "Metric unit is required" |
| observed_at | Yes | Valid timestamp, not future-dated | "Observation timestamp is required and must not be in the future" |
| collected_by_person_id | Recommended | Must reference valid person if provided | "Collector person not found" (warning) |
| equipment_asset_id | Recommended | Must reference valid equipment if source_type = Instrument | "Instrument source should reference equipment" (warning) |
| facility_space_id | Optional | Must reference valid space if provided | N/A |

## 4. Unit Standardization

### Approved Unit Vocabulary (Resin Treatment R&D)
| Measurement Type | Approved Units | Conversion Notes |
|---|---|---|
| Temperature | °C, K | Do not mix; convert if necessary |
| Flow Rate | L/min, mL/min, L/h | Consistent within experiment |
| Pressure | psi, kPa, bar | Consistent within experiment |
| Concentration | mg/L, ppm, % w/v | Specify basis clearly |
| pH | pH units (dimensionless) | Values 0-14 |
| Conductivity | µS/cm, mS/cm | Consistent within experiment |
| Volume | L, mL | Consistent within experiment |
| Mass | g, kg, mg | Consistent within experiment |
| Time | s, min, h | Consistent within experiment |

### Unit Validation Rules
- System should maintain controlled unit vocabulary per measurement type.
- If unit not in approved list, flag for review before accepting.
- Unit consistency check: if same `metric_name` used multiple times in project, warn if different units detected.

## 5. Range Validation

### Min/Max Valid Fields
- `min_valid` and `max_valid` fields in `data_collection_records` define acceptable range.
- If provided, system checks `metric_value` against range.
- If `metric_value < min_valid` or `metric_value > max_valid`, set `out_of_range_flag = true`.

### Default Ranges (Resin Treatment Common Metrics)
| Metric Name | Typical Min | Typical Max | Notes |
|---|---|---|---|
| Feed Temperature | 5 | 95 | °C; adjust for specific process |
| Product pH | 0 | 14 | pH units |
| Inlet Pressure | 0 | 150 | psi; depends on system design |
| Flow Rate | 0.1 | 50 | L/min; depends on system capacity |
| Conductivity | 0 | 10000 | µS/cm; high end for concentrated streams |
| Regeneration Efficiency | 0 | 100 | %; percentage values |

### Out-of-Range Handling
- Out-of-range values are not automatically rejected.
- Flag triggers human review workflow:
  - **Valid Anomaly:** Measurement is correct but unexpected (e.g., breakthrough event, process upset). Reviewer accepts with rationale.
  - **Measurement Error:** Instrument malfunction or data entry mistake. Reviewer flags data for exclusion or correction.
- Reviewer decision recorded in `ai_recommendations` or separate review log.

## 6. Provenance Metadata Rules

### Source Type Validation
- **Manual:** Operator hand-entered value. `collected_by_person_id` required.
- **Instrument:** Auto-captured from sensor/instrument. `equipment_asset_id` strongly recommended.
- **Imported:** Batch upload from file. `ingestion_batch_id` required.

### Timestamp Validation
- `observed_at` must not be future-dated (allow up to 5 minutes future to account for clock skew).
- `observed_at` should be within experiment start/end window (warning if outside, not hard error).

### Operator Attribution
- If `source_type = Manual`, `collected_by_person_id` should be populated.
- If missing, system can default to current logged-in user but flag for confirmation.

### Equipment Context
- If `source_type = Instrument`, `equipment_asset_id` should be populated.
- System can validate equipment is Active status and has valid calibration (warning if not).

## 7. Batch Import Validation

### File Format Requirements
- **CSV:** Header row required, columns: metric_name, metric_value, metric_unit, observed_at (ISO 8601 format), optional: equipment_asset_id, facility_space_id.
- **JSON:** Array of objects with same field names.

### Import Validation Steps
1. Parse file and validate structure (correct columns/fields).
2. Check each row for required field presence.
3. Validate data types (numeric for value, valid timestamp for observed_at).
4. Check unit consistency within batch.
5. Check range validity if min/max defined.
6. Flag rows with errors; optionally allow partial import (valid rows only) or reject entire batch.

### Import Provenance
- `ingestion_batch_id`: Unique identifier for batch (e.g., timestamp + filename hash).
- `raw_payload`: Store original row data (JSON/TEXT) for audit and re-processing if needed.
- Import metadata logged: who imported, when, filename, row count, error count.

## 8. Data Quality Gates (Pre-Export Checks)

### Before SR&ED/IRAP Report Export
System should validate:
- [ ] All data collection records linked to claim have required fields populated.
- [ ] No unresolved out-of-range flags (all reviewed and accepted or excluded).
- [ ] Units consistent within project (or conversions documented).
- [ ] Provenance metadata present (source, operator, timestamp).
- [ ] Linked equipment has valid calibration (if instrument source).
- [ ] No orphaned records (experiment or project deleted but data remains).

### Export Exclusion Rules
- Records with `out_of_range_flag = true` and no reviewer acceptance: exclude or flag for manual review.
- Records missing `metric_unit`: exclude or require correction before export.
- Records with future-dated `observed_at`: exclude or require timestamp correction.

## 9. Unit Conversion and Normalization

### When to Convert
- If same metric measured in different units within project, system can offer conversion.
- Conversion must be logged: original value/unit + converted value/unit + conversion factor.

### Conversion Factors (Examples)
- Temperature: °C to K = +273.15
- Pressure: psi to kPa = ×6.89476
- Flow Rate: L/min to mL/min = ×1000

### Conversion Audit Trail
- Store both original and converted values.
- Export should include conversion metadata for transparency.

## 10. Controlled Vocabularies

### Metric Name Standards
- Maintain project-specific or system-wide controlled vocabulary for `metric_name`.
- Examples: "Inlet Temperature", "Product pH", "Regenerant Flow Rate", "Effluent Conductivity".
- Free-text entry allowed but flag if not in controlled list (for review and potential vocab expansion).

### Equipment Type Standards
- Equipment types in `equipment_assets.equipment_type`: Sensor, Reactor, Pump, Meter, Other.
- Controlled list prevents typos and inconsistency.

### Activity Category Standards
- Labour entry `activity_category` uses controlled list: Experimentation, Analysis, Documentation, Dev, Troubleshooting.
- Prevents misclassification and improves claim accuracy.

## 11. Automated Validation Checks (System Implementation)

### Pre-Save Validation
- Before `data_collection_records` insert/update, run validation:
  - Required fields present?
  - Data types correct?
  - Timestamp valid (not future)?
  - Range check (if min/max defined)?
- If validation fails, return error and block save until corrected.

### Post-Save Warnings
- If optional but recommended fields missing (e.g., `collected_by_person_id` for Manual source), show warning but allow save.
- If unit not in approved vocabulary, show warning and offer to add to project-specific list.

### Periodic Audit Scans
- Weekly or monthly: scan all data collection records for orphaned links, missing provenance, unresolved anomalies.
- Generate report for founder review.

## 12. Acceptance Test Mapping

### Test Case: Required Field Validation
- **Given:** User creates data collection record without `metric_unit`.
- **When:** User attempts to save.
- **Then:** System returns error "Metric unit is required" and blocks save.

### Test Case: Range Validation
- **Given:** Data record with `metric_value = 150`, `min_valid = 0`, `max_valid = 100`.
- **When:** Record saved.
- **Then:** System sets `out_of_range_flag = true` and triggers review workflow.

### Test Case: Unit Consistency Warning
- **Given:** Project has 5 "Inlet Temperature" records in °C.
- **When:** User creates 6th record with "Inlet Temperature" in K.
- **Then:** System warns "Unit mismatch detected for Inlet Temperature" but allows save with flag.

### Test Case: Batch Import
- **Given:** CSV file with 100 rows, 2 rows missing `metric_value`.
- **When:** User imports file.
- **Then:** System reports "98 rows imported, 2 rows rejected due to missing metric_value" and logs error details.

## 13. Compliance Checklist

- [ ] Required field validation enforced at data entry and import.
- [ ] Unit vocabulary defined and consistency checks active.
- [ ] Range validation logic implemented with out-of-range flag.
- [ ] Provenance fields captured (source type, operator, equipment, timestamp).
- [ ] Pre-export data quality gate configured.
- [ ] Batch import validation tested with error handling.
- [ ] Controlled vocabularies maintained for metric names and units.
- [ ] Acceptance tests written and passing for validation rules.

## 14. Open Decisions
- Confirm project-specific unit standards vs. system-wide vocabulary.
- Confirm range defaults per metric or require manual entry per experiment.
- Confirm if unit conversion should be automatic or manual with approval.
- Confirm error handling policy for batch imports: partial success allowed or all-or-nothing?

## 15. Next Steps
1. Implement required field validation in data collection entry form and API.
2. Build unit vocabulary maintenance interface (add/edit approved units).
3. Create range validation check and out-of-range flag workflow.
4. Test batch import with validation and error reporting.
5. Map validation rules to acceptance test cases in `TEST_STRATEGY_2026.md`.
