# DATA DICTIONARY 2026

## 1. Purpose
Define canonical entity and field meaning for the 2026 R&D management system before implementation.

## 2. Conventions
- Primary key: `id` (INT AUTO_INCREMENT).
- Foreign key naming: `<entity>_id`.
- Timestamps: `created_at`, `updated_at` (`TIMESTAMP`, UTC recommended).
- Optional soft delete: `deleted_at` (`TIMESTAMP NULL`).
- Status fields: constrained enum/check values at DB or service layer.
- Audit-sensitive records should be append-only by policy even if updates are technically possible.

## 3. Core Project and Experiment Tables

### Table: `projects`
| Field | Type | Required | Description |
|---|---|---|---|
| id | INT | Yes | Unique project identifier |
| code | VARCHAR(50) | Yes | Human-readable project code |
| name | VARCHAR(255) | Yes | Project title |
| objective | TEXT | Yes | R&D objective statement |
| scope_summary | TEXT | No | Scope baseline summary |
| status | VARCHAR(50) | Yes | Draft, Active, On Hold, Closed |
| start_date | DATE | No | Planned/actual start |
| end_date | DATE | No | Planned/actual completion |
| created_at | TIMESTAMP | Yes | Record creation time |
| updated_at | TIMESTAMP | No | Last update time |

### Table: `experiments`
| Field | Type | Required | Description |
|---|---|---|---|
| id | INT | Yes | Unique experiment identifier |
| project_id | INT | Yes | Parent project |
| title | VARCHAR(255) | Yes | Experiment name |
| uncertainty_statement | TEXT | Yes | Technological uncertainty for SR&ED |
| hypothesis | TEXT | Yes | Testable technical hypothesis |
| procedure | TEXT | Yes | Experimental method |
| expected_result | TEXT | Yes | Predicted outcome |
| actual_result | TEXT | No | Observed outcome |
| analysis | TEXT | No | Human technical interpretation |
| advancement | TEXT | No | Claimed technical advancement |
| status | VARCHAR(50) | Yes | Planned, In Progress, Completed, Failed |
| started_at | TIMESTAMP | No | Execution start |
| completed_at | TIMESTAMP | No | Execution completion |
| created_by_user_id | INT | Yes | Creator |
| created_at | TIMESTAMP | Yes | Record creation time |
| updated_at | TIMESTAMP | No | Last update time |

### Table: `experiment_versions`
| Field | Type | Required | Description |
|---|---|---|---|
| id | INT | Yes | Version entry identifier |
| experiment_id | INT | Yes | Related experiment |
| version_number | INT | Yes | Sequential version |
| changed_by_user_id | INT | Yes | User making the change |
| change_summary | TEXT | Yes | Description of change |
| payload_snapshot | JSON/TEXT | Yes | Serialized full snapshot |
| created_at | TIMESTAMP | Yes | Version timestamp |

## 4. People and Labour Tables

### Table: `people`
| Field | Type | Required | Description |
|---|---|---|---|
| id | INT | Yes | Person record identifier |
| user_id | INT | No | Linked system user account, if any |
| full_name | VARCHAR(200) | Yes | Legal/preferred full name |
| person_type | VARCHAR(50) | Yes | Founder, Employee, Contractor, Advisor |
| role_title | VARCHAR(120) | Yes | Functional role |
| sred_eligibility | VARCHAR(50) | Yes | Eligible, Partially Eligible, Not Eligible |
| irap_eligibility | VARCHAR(50) | Yes | Eligible, Review Required, Not Eligible |
| hourly_rate | DECIMAL(12,2) | No | Standard cost/claim rate |
| currency | VARCHAR(10) | No | Currency code (default CAD) |
| active_from | DATE | Yes | Start date |
| active_to | DATE | No | End date if inactive |
| notes | TEXT | No | Classification notes |
| created_at | TIMESTAMP | Yes | Record creation time |
| updated_at | TIMESTAMP | No | Last update time |

### Table: `labour_entries`
| Field | Type | Required | Description |
|---|---|---|---|
| id | INT | Yes | Labour entry identifier |
| project_id | INT | Yes | Parent project |
| experiment_id | INT | No | Related experiment |
| person_id | INT | Yes | Person performing the work |
| activity_category | VARCHAR(80) | Yes | Experimentation, Analysis, Documentation, Dev |
| sred_category | VARCHAR(50) | No | SR&ED category mapping |
| work_date | DATE | Yes | Date of work |
| hours | DECIMAL(6,2) | Yes | Hours worked |
| rate_used | DECIMAL(12,2) | Yes | Applied rate at time of entry |
| amount | DECIMAL(12,2) | Yes | Computed labour amount |
| description | TEXT | Yes | Work performed summary |
| evidence_ref | VARCHAR(120) | No | Reference to supporting artifact |
| approved_by_user_id | INT | No | Reviewer/approver |
| created_at | TIMESTAMP | Yes | Record creation time |

## 5. Cost and Evidence Tables

### Table: `cost_entries`
| Field | Type | Required | Description |
|---|---|---|---|
| id | INT | Yes | Cost entry identifier |
| project_id | INT | Yes | Parent project |
| experiment_id | INT | No | Related experiment |
| labour_entry_id | INT | No | Linked labour entry if derived |
| category | VARCHAR(50) | Yes | Labour, Materials, Subcontractor, Equipment |
| sred_category | VARCHAR(50) | Yes | SR&ED classification bucket |
| amount | DECIMAL(12,2) | Yes | Cost amount |
| currency | VARCHAR(10) | Yes | Currency code (e.g., CAD) |
| incurred_on | DATE | Yes | Cost date |
| vendor_name | VARCHAR(200) | No | Vendor/supplier |
| reference_note | TEXT | No | Supporting details |
| created_at | TIMESTAMP | Yes | Record creation time |

### Table: `evidence_files`
| Field | Type | Required | Description |
|---|---|---|---|
| id | INT | Yes | Evidence file identifier |
| project_id | INT | Yes | Parent project |
| experiment_id | INT | No | Related experiment |
| labour_entry_id | INT | No | Linked labour evidence |
| uploaded_by_user_id | INT | Yes | Uploader |
| file_name | VARCHAR(255) | Yes | Original file name |
| file_path | TEXT | Yes | Storage path |
| file_hash | VARCHAR(128) | Yes | Integrity hash |
| mime_type | VARCHAR(120) | Yes | Media type |
| size_bytes | BIGINT | Yes | File size |
| tags | TEXT | No | Optional search tags |
| uploaded_at | TIMESTAMP | Yes | Upload timestamp |

## 6. Equipment and Data Collection Tables

### Table: `equipment_assets`
| Field | Type | Required | Description |
|---|---|---|---|
| id | INT | Yes | Equipment identifier |
| asset_tag | VARCHAR(80) | Yes | Internal asset code |
| equipment_name | VARCHAR(200) | Yes | Asset name |
| equipment_type | VARCHAR(80) | Yes | Sensor, Reactor, Pump, Meter, Other |
| manufacturer | VARCHAR(120) | No | Manufacturer |
| model_number | VARCHAR(120) | No | Model reference |
| serial_number | VARCHAR(120) | No | Serial identifier |
| owner_person_id | INT | No | Responsible person |
| location | VARCHAR(150) | No | Storage/usage location |
| status | VARCHAR(50) | Yes | Active, Maintenance, Retired |
| commissioned_on | DATE | No | Date introduced |
| created_at | TIMESTAMP | Yes | Record creation time |
| updated_at | TIMESTAMP | No | Last update time |

### Table: `equipment_calibrations`
| Field | Type | Required | Description |
|---|---|---|---|
| id | INT | Yes | Calibration log identifier |
| equipment_asset_id | INT | Yes | Related equipment |
| calibration_date | DATE | Yes | Calibration completion date |
| calibrated_by_person_id | INT | No | Person or contractor |
| method | VARCHAR(120) | No | Calibration protocol |
| result_status | VARCHAR(50) | Yes | Passed, Failed, Conditional |
| due_next_on | DATE | No | Next calibration due date |
| certificate_file_id | INT | No | Evidence file reference |
| notes | TEXT | No | Calibration notes |
| created_at | TIMESTAMP | Yes | Record creation time |

### Table: `experiment_equipment`
| Field | Type | Required | Description |
|---|---|---|---|
| id | INT | Yes | Link identifier |
| experiment_id | INT | Yes | Experiment |
| experiment_version_id | INT | No | Specific experiment version |
| equipment_asset_id | INT | Yes | Equipment used |
| usage_role | VARCHAR(80) | No | Measurement, Processing, Monitoring |
| started_at | TIMESTAMP | No | Start of use |
| ended_at | TIMESTAMP | No | End of use |
| notes | TEXT | No | Context notes |
| created_at | TIMESTAMP | Yes | Record creation time |

### Table: `data_collection_records`
| Field | Type | Required | Description |
|---|---|---|---|
| id | INT | Yes | Data record identifier |
| project_id | INT | Yes | Parent project |
| experiment_id | INT | Yes | Experiment reference |
| experiment_version_id | INT | No | Version context |
| facility_space_id | INT | No | Physical zone where collection happened |
| equipment_asset_id | INT | No | Source equipment if instrument-derived |
| collected_by_person_id | INT | No | Operator/collector |
| source_type | VARCHAR(50) | Yes | Manual, Instrument, Imported |
| metric_name | VARCHAR(120) | Yes | Measurement label |
| metric_value | DECIMAL(18,6) | Yes | Numeric value |
| metric_unit | VARCHAR(40) | Yes | Unit string |
| min_valid | DECIMAL(18,6) | No | Lower validation bound |
| max_valid | DECIMAL(18,6) | No | Upper validation bound |
| out_of_range_flag | BOOLEAN | Yes | Validation failure flag |
| observed_at | TIMESTAMP | Yes | Measurement time |
| ingestion_batch_id | VARCHAR(80) | No | Import/ingestion batch reference |
| raw_payload | JSON/TEXT | No | Original row/payload for traceability |
| created_at | TIMESTAMP | Yes | Record creation time |

## 7. Facility and Space Tables

### Table: `facility_spaces`
| Field | Type | Required | Description |
|---|---|---|---|
| id | INT | Yes | Facility space identifier |
| project_id | INT | Yes | Parent project |
| space_code | VARCHAR(80) | Yes | Internal zone code |
| space_name | VARCHAR(150) | Yes | Human-readable zone name |
| space_type | VARCHAR(60) | Yes | Bench, Storage, TestArea, SafetyZone, Office |
| status | VARCHAR(50) | Yes | Active, Restricted, OutOfService |
| max_occupancy | INT | No | Capacity guidance |
| notes | TEXT | No | Setup notes and constraints |
| created_at | TIMESTAMP | Yes | Record creation time |
| updated_at | TIMESTAMP | No | Last update time |

### Table: `facility_readiness_checks`
| Field | Type | Required | Description |
|---|---|---|---|
| id | INT | Yes | Readiness check identifier |
| facility_space_id | INT | Yes | Checked space |
| checked_by_person_id | INT | Yes | Person performing check |
| check_date | DATE | Yes | Date of check |
| setup_status | VARCHAR(50) | Yes | Ready, Conditional, NotReady |
| safety_status | VARCHAR(50) | Yes | Pass, Warning, Fail |
| checklist_version | VARCHAR(40) | No | Checklist template version |
| findings | TEXT | No | Findings summary |
| corrective_action | TEXT | No | Required actions |
| evidence_file_id | INT | No | Supporting evidence reference |
| valid_until | DATE | No | Readiness expiry |
| created_at | TIMESTAMP | Yes | Record creation time |

## 8. AI Integration Tables

### Table: `ai_models`
| Field | Type | Required | Description |
|---|---|---|---|
| id | INT | Yes | AI model record identifier |
| provider | VARCHAR(80) | Yes | Model provider |
| model_name | VARCHAR(120) | Yes | Model name |
| model_version | VARCHAR(80) | Yes | Version tag |
| use_case | VARCHAR(120) | Yes | Ingestion analysis, interpretation, anomaly detection |
| prompt_template_version | VARCHAR(80) | No | Prompt template/version reference |
| confidence_policy | VARCHAR(120) | No | Threshold policy identifier |
| active_flag | BOOLEAN | Yes | Whether model is active |
| created_at | TIMESTAMP | Yes | Record creation time |

### Table: `ai_analysis_runs`
| Field | Type | Required | Description |
|---|---|---|---|
| id | INT | Yes | AI analysis run identifier |
| project_id | INT | Yes | Parent project |
| experiment_id | INT | Yes | Experiment context |
| experiment_version_id | INT | No | Version context |
| data_collection_record_id | INT | No | Single-record source if applicable |
| ingestion_batch_id | VARCHAR(80) | No | Batch source reference |
| ai_model_id | INT | Yes | Model/version used |
| trigger_type | VARCHAR(50) | Yes | On Ingest, Scheduled, Manual |
| input_summary | TEXT | Yes | Description of inputs provided |
| output_summary | TEXT | Yes | AI-generated interpretation |
| confidence_score | DECIMAL(5,4) | No | Confidence value (0-1) |
| anomaly_flag | BOOLEAN | Yes | Potential anomaly detected |
| failure_flag | BOOLEAN | Yes | Potential failure condition detected |
| improvement_flag | BOOLEAN | Yes | Potential optimization identified |
| run_status | VARCHAR(50) | Yes | Completed, Failed, Needs Review |
| executed_at | TIMESTAMP | Yes | Run timestamp |
| created_at | TIMESTAMP | Yes | Record creation time |

### Table: `ai_recommendations`
| Field | Type | Required | Description |
|---|---|---|---|
| id | INT | Yes | Recommendation identifier |
| ai_analysis_run_id | INT | Yes | Source AI run |
| recommendation_type | VARCHAR(80) | Yes | Improvement, Risk, Failure, DataQuality |
| recommendation_text | TEXT | Yes | Proposed action |
| severity | VARCHAR(40) | Yes | Low, Medium, High, Critical |
| reviewer_person_id | INT | No | Human reviewer |
| reviewer_decision | VARCHAR(50) | No | Accepted, Rejected, Deferred |
| reviewer_rationale | TEXT | No | Human decision reason |
| decision_at | TIMESTAMP | No | Decision timestamp |
| created_at | TIMESTAMP | Yes | Record creation time |

## 9. Planning and Governance Tables

### Table: `sprints`
| Field | Type | Required | Description |
|---|---|---|---|
| id | INT | Yes | Sprint identifier |
| project_id | INT | Yes | Parent project |
| sprint_name | VARCHAR(120) | Yes | Sprint label |
| goal | TEXT | No | Sprint objective |
| start_date | DATE | Yes | Sprint start |
| end_date | DATE | Yes | Sprint end |
| status | VARCHAR(50) | Yes | Planned, Active, Closed |
| created_at | TIMESTAMP | Yes | Record creation time |

### Table: `backlog_items`
| Field | Type | Required | Description |
|---|---|---|---|
| id | INT | Yes | Backlog item identifier |
| project_id | INT | Yes | Parent project |
| sprint_id | INT | No | Assigned sprint |
| title | VARCHAR(255) | Yes | Work item title |
| description | TEXT | No | Detailed description |
| item_type | VARCHAR(60) | Yes | Uncertainty, Experiment, DevTask, DocTask |
| priority | VARCHAR(40) | Yes | Must, Should, Could |
| status | VARCHAR(50) | Yes | Todo, In Progress, Blocked, Done |
| estimate_points | INT | No | Story points |
| owner_person_id | INT | No | Assigned owner |
| requirement_refs | VARCHAR(255) | No | Comma list of FR/CR/NFR IDs |
| created_at | TIMESTAMP | Yes | Record creation time |
| updated_at | TIMESTAMP | No | Last update time |

### Table: `risks`
| Field | Type | Required | Description |
|---|---|---|---|
| id | INT | Yes | Risk identifier |
| project_id | INT | Yes | Parent project |
| title | VARCHAR(200) | Yes | Risk title |
| description | TEXT | Yes | Risk statement |
| probability_score | INT | Yes | Probability score (1-5) |
| impact_score | INT | Yes | Impact score (1-5) |
| mitigation_plan | TEXT | No | Mitigation actions |
| owner_person_id | INT | No | Risk owner |
| status | VARCHAR(50) | Yes | Open, Monitoring, Closed |
| created_at | TIMESTAMP | Yes | Record creation time |

### Table: `procurements`
| Field | Type | Required | Description |
|---|---|---|---|
| id | INT | Yes | Procurement identifier |
| project_id | INT | Yes | Parent project |
| item_name | VARCHAR(200) | Yes | Purchased item/service |
| vendor_name | VARCHAR(200) | Yes | Vendor |
| category | VARCHAR(80) | Yes | Chemical, Equipment, Service, Software |
| amount | DECIMAL(12,2) | Yes | Cost amount |
| currency | VARCHAR(10) | Yes | Currency code |
| ordered_on | DATE | No | Order date |
| received_on | DATE | No | Receipt date |
| status | VARCHAR(50) | Yes | Requested, Ordered, Received, Closed |
| linked_cost_entry_id | INT | No | Related cost entry |
| created_at | TIMESTAMP | Yes | Record creation time |

## 10. Access and Reporting Tables

### Table: `users`
| Field | Type | Required | Description |
|---|---|---|---|
| id | INT | Yes | User identifier |
| email | VARCHAR(255) | Yes | Login identity |
| password_hash | VARCHAR(255) | Yes | Hashed password |
| role | VARCHAR(50) | Yes | Admin, Founder, Researcher, Finance, Viewer |
| status | VARCHAR(50) | Yes | Active, Disabled |
| last_login_at | TIMESTAMP | No | Last login timestamp |
| created_at | TIMESTAMP | Yes | Record creation time |
| updated_at | TIMESTAMP | No | Last update time |

### Table: `reports`
| Field | Type | Required | Description |
|---|---|---|---|
| id | INT | Yes | Report identifier |
| project_id | INT | Yes | Parent project |
| report_type | VARCHAR(80) | Yes | SRED, IRAP, FedDev, NGen, OCE, BDC |
| period_start | DATE | No | Report period start |
| period_end | DATE | No | Report period end |
| generated_by_user_id | INT | Yes | Generator |
| generation_method | VARCHAR(50) | Yes | Manual, Automated, AI Assisted |
| file_path | TEXT | No | Output file location |
| status | VARCHAR(50) | Yes | Draft, Final |
| created_at | TIMESTAMP | Yes | Record creation time |

## 11. Relationship Summary
- `projects` 1:N `experiments`, `cost_entries`, `labour_entries`, `sprints`, `backlog_items`, `risks`, `procurements`, `reports`.
- `projects` 1:N `facility_spaces`.
- `experiments` 1:N `experiment_versions`, `data_collection_records`, `experiment_equipment`, `ai_analysis_runs`.
- `people` 1:N `labour_entries`, `equipment_assets` (owner), `data_collection_records` (collector), `ai_recommendations` (reviewer).
- `people` 1:N `facility_readiness_checks` (checker).
- `facility_spaces` 1:N `facility_readiness_checks`, `data_collection_records`.
- `equipment_assets` 1:N `equipment_calibrations`, `experiment_equipment`, `data_collection_records`.
- `ai_models` 1:N `ai_analysis_runs`; `ai_analysis_runs` 1:N `ai_recommendations`.
- `users` optionally map to `people` via `people.user_id`.

## 12. Data Quality and Compliance Rules
- Every claim-relevant labour or cost record must be attributable to a person and project.
- AI-generated recommendations cannot be treated as final outcomes without human reviewer decision.
- Measurement units for a metric must be consistent within a project unless explicit conversion metadata is stored.
- Out-of-range measurements must set `out_of_range_flag = true` and trigger review workflow.
- Critical evidence records should include hash and upload metadata for integrity verification.
- Claim-linked experimental activity should include facility zone context when work is performed in physical shop/lab space.
- Work in restricted spaces should require a valid readiness check status before use.

## 13. Open Decisions
- Confirm target DB engine for `JSON` type support (`MyZIM` compatibility vs MariaDB JSON).
- Confirm if immutable ledger/event table is required in addition to entity history tables.
- Confirm PII handling policy for people records and export redaction rules.
- Confirm retention windows for raw AI inputs/outputs and evidence artifacts.
- Confirm facility readiness checklist standard and minimum safety fields required for grant/compliance evidence.
