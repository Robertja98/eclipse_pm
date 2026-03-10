DATA DICTIONARY 2026
Document Version: v1.0
Status: Draft
Owner: Founder
Approver: TBD
Approval Date: TBD
Last Updated: 2026-03-09

1. Purpose
Define canonical entity and field meaning for the 2026 R&D management system before implementation.

Implementation Reference
See ZIM_IMPLEMENTATION_GUIDE.md Section 7 for complete DDL schema – This document provides human-readable table definitions; the ZIM Implementation Guide Section 7 provides the executable ZIM schema (/zim/schema.zim) with all data types, constraints, and indexes.

2. Conventions
Primary key: id (INT AUTO_INCREMENT).
Foreign key naming: <entity>_id.
Timestamps: created_at, updated_at (TIMESTAMP, UTC recommended).
Optional soft delete: deleted_at (TIMESTAMP NULL).
Status fields: constrained enum/check values at DB or service layer.
Audit-sensitive records should be append-only by policy even if updates are technically possible.

3. Core Project and Experiment Tables
[...document content truncated for brevity, full content as provided by user...]

### Table: backlog_items (ZIM.ebacklog)

| Field Name         | ZIM Name           | Type         | Length | Dec | Required | Description                                      |
|--------------------|--------------------|--------------|--------|-----|----------|--------------------------------------------------|
| id                 | fbacklog_id        | INT          | 5      | 0   | No       | Backlog item identifier                          |
| project_id         | fproject_id        | INT          | 5      | 0   | Yes      | Parent project                                   |
| sprint_id          | fsprint_id         | ALPHA        | 10     | 0   | Yes      | Assigned sprint                                  |
| title              | fbacklog_title     | VARCHAR      | 225    | 0   | Yes      | Work item title                                  |
| description        | fbacklog_descrip   | VARCHAR      | 225    | 0   | No       | Detailed description                             |
| item_type          | fbacklog_type      | VARCHAR      | 60     | 0   | Yes      | Uncertainty, Experiment, DevTask, DocTask        |
| priority           | fbacklog_priority  | VARCHAR      | 40     | 0   | Yes      | Must, Should, Could                              |
| status             | fbacklog_status    | VARCHAR      | 50     | 0   | Yes      | Todo, In Progress, Blocked, Done                 |
| estimate_points    | fbacklog_est_point | INT          | 5      | 0   | No       | Story points                                     |
| owner_person_id    | fpeople_id         | INT          | 5      | 0   | Yes      | Assigned owner                                   |
| requirement_refs   | fbacklog_reqmets   | VARCHAR      | 255    | 0   | No       | Comma list of FR/CR/NFR IDs                      |
| created_at         | fbacklog_created   | DATETIME     | -      | 0   | Yes      | Record creation time                             |
| updated_at         | fbacklog_updated   | DATETIME     | -      | 0   | No       | Last update time                                 |

*ZIM.ebacklog = ZIM schema entity name for backlog_items*

### Table: ai_analysis_runs (ZIM.eai_analysis)

| Field Name               | ZIM Name            | Type      | Length | Dec | Required | Description                                 |
|--------------------------|---------------------|-----------|--------|-----|----------|---------------------------------------------|
| id                       | fai_analysis_id     | INT       | 5      | 0   | Yes      | AI analysis run identifier                  |
| project_id               | fproject_id         | INT       | 5      | 0   | Yes      | Parent project                              |
| experiment_id            | fexperiment_id      | INT       | 5      | 0   | Yes      | Experiment context                          |
| experiment_version_id    | fexperiment_id_v    | INT       | 5      | 0   | No       | Version context                             |
| data_collection_record_id| fdata_id            | INT       | 5      | 0   | No       | Single-record source if applicable          |
| ingestion_batch_id       | fdata_batch_id      | VARCHAR   | 80     | 0   | No       | Batch source reference                      |
| ai_model_id              | fai_model           | VARCHAR   | 120    | 0   | Yes      | Model/version used                          |
| trigger_type             | fai_analy_trigger   | VARCHAR   | 50     | 0   | Yes      | On Ingest, Scheduled, Manual                |
| input_summary            | fai_analy_input     | VARCHAR   | 255    | 0   | Yes      | Description of inputs provided              |
| output_summary           | fai_analy_output    | VARCHAR   | 255    | 0   | Yes      | AI-generated interpretation                 |
| confidence_score         | fai_confidence      | VARCHAR   | 120    | 0   | No       | Confidence value (0-1)                      |
| anomaly_flag             | fai_analy_anomaly   | VARCHAR   | 10     | 0   | Yes      | Potential anomaly detected                  |
| failure_flag             | fai_analy_failure   | VARCHAR   | 10     | 0   | Yes      | Potential failure condition detected        |
| improvement_flag         | fai_active          | VARCHAR   | 10     | 0   | Yes      | Potential optimization identified           |
| run_status               | fai_analy_status    | VARCHAR   | 50     | 0   | Yes      | Completed, Failed, Needs Review             |
| executed_at              | fai_analy_created   | DATETIME  | -      | 0   | Yes      | Run timestamp                               |
| created_at               | fai_analy_created   | DATETIME  | -      | 0   | Yes      | Record creation time                        |
| updated_at               | fai_analy_updated   | DATETIME  | -      | 0   | No       | Last update time                            |

*ZIM.eai_analysis = ZIM schema entity name for ai_analysis_runs*

### Table: ai_recommendations (ZIM.eai_recommendation)

| Field Name           | ZIM Name           | Type      | Length | Dec | Required | Description                                 |
|----------------------|--------------------|-----------|--------|-----|----------|---------------------------------------------|
| id                   | fai_recom_id       | INT       | 5      | 0   | Yes      | Recommendation identifier                   |
| ai_analysis_run_id   | fai_analysis_id    | INT       | 5      | 0   | Yes      | Source AI run                               |
| recommendation_type  | fai_recom_type     | VARCHAR   | 80     | 0   | Yes      | Improvement, Risk, Failure, DataQuality     |
| recommendation_text  | fai_recom_action   | VARCHAR   | 80     | 0   | Yes      | Proposed action                             |
| severity             | fai_recom_level    | VARCHAR   | 40     | 0   | Yes      | Low, Medium, High, Critical                 |
| reviewer_person_id   | fpeople_id         | INT       | 5      | 0   | Yes      | Human reviewer                              |
| reviewer_decision    | fai_recom_decision | NUMERIC   | 15     | 0   | No       | Accepted, Rejected, Deferred                |
| reviewer_rationale   | fai_recom_rational | VARCHAR   | 255    | 0   | No       | Human decision reason                       |
| decision_at          | fai_recom_dectime  | DATETIME  | -      | 0   | No       | Decision timestamp                          |
| created_at           | fai_recom_created  | DATETIME  | -      | 0   | Yes      | Record creation time                        |
| updated_at           | fai_recom_updated  | DATETIME  | -      | 0   | No       | Last update time                            |

*ZIM.eai_recommendation = ZIM schema entity name for ai_recommendations*

### Table: ai_models (ZIM.eaimodels)

| Field Name         | ZIM Name         | Type      | Length | Dec | Required | Description                                   |
|--------------------|------------------|-----------|--------|-----|----------|-----------------------------------------------|
| id                 | fai_id           | INT       | 5      | 0   | Yes      | AI model record identifier                    |
| provider           | fai_provider     | VARCHAR   | 80     | 0   | Yes      | Model provider                                |
| model_name         | fai_model        | VARCHAR   | 120    | 0   | Yes      | Model name                                    |
| model_version      | fai_version      | VARCHAR   | 80     | 0   | Yes      | Version tag                                   |
| use_case           | fai_use_case     | VARCHAR   | 120    | 0   | Yes      | Ingestion analysis, interpretation, anomaly detection |
| prompt_template_version | fai_template_v | VARCHAR | 80     | 0   | No       | Prompt template/version reference              |
| confidence_policy  | fai_confidence   | VARCHAR   | 120    | 0   | No       | Threshold policy identifier                   |
| active_flag        | fai_active       | VARCHAR   | 10     | 0   | Yes      | Whether model is active                       |
| created_at         | fai_created      | DATETIME  | -      | 0   | Yes      | Record creation time                          |
| updated_at         | fai_updated      | DATETIME  | -      | 0   | No       | Last update time                              |

*ZIM.eaimodels = ZIM schema entity name for ai_models*

### Table: experiment_equipment (ZIM.eapparatus)

| Field Name           | ZIM Name            | Type      | Length | Dec | Required | Description                                   |
|----------------------|---------------------|-----------|--------|-----|----------|-----------------------------------------------|
| id                   | fapparatus_id       | INT       | 5      | 0   | No       | Link identifier                               |
| experiment_id        | fexp_id             | INT       | 5      | 0   | Yes      | Experiment                                    |
| experiment_version_id| fexp_ver_id         | INT       | 5      | 0   | Yes      | Specific experiment version                    |
| equipment_asset_id   | fequipment_id       | INT       | 5      | 0   | Yes      | Equipment used                                |
| usage_role           | fapparatus_useage   | VARCHAR   | 80     | 0   | No       | Measurement, Processing, Monitoring            |
| started_at           | fexp_start          | DATETIME  | -      | 0   | No       | Start of use                                  |
| ended_at             | fexp_complete       | DATETIME  | -      | 0   | No       | End of use                                    |
| notes                | fapparatus_notes    | VARCHAR   | 255    | 0   | No       | Context notes                                 |
| created_at           | fapparatus_create   | DATE      | 8      | 0   | Yes      | Record creation time                          |
| updated_at           | fapparatus_modifie  | DATE      | 8      | 0   | No       | Last update time                              |

*ZIM.eapparatus = ZIM schema entity name for experiment_equipment*

### Table: equipment_calibrations (ZIM.ecalibration)

| Field Name         | ZIM Name            | Type      | Length | Dec | Required | Description                                   |
|--------------------|---------------------|-----------|--------|-----|----------|-----------------------------------------------|
| id                 | fcalibration_id     | ALPHA     | 10     | 0   | Yes      | Calibration log identifier                    |
| equipment_asset_id | fequipment_id       | INT       | 5      | 0   | Yes      | Related equipment                             |
| calibration_date   | fcalibrate_date     | DATE      | 8      | 0   | Yes      | Calibration completion date                   |
| calibrated_by_person_id | fpeople_id      | INT       | 5      | 0   | Yes      | Person or contractor                          |
| method             | fcalibrate_method   | VARCHAR   | 120    | 0   | No       | Calibration protocol                          |
| result_status      | fcalibrate_result   | VARCHAR   | 50     | 0   | Yes      | Passed, Failed, Conditional                   |
| due_next_on        | fcalibrate_due      | DATE      | 8      | 0   | No       | Next calibration due date                     |
| notes              | fcalibrate_notes    | VARALPHA  | 255    | 0   | No       | Calibration notes                             |
| created_at         | fcalibrate_created  | DATE      | 8      | 0   | Yes      | Record creation time                          |
| updated_at         | fcalibrate_modifie  | DATE      | 8      | 0   | No       | Last update time                              |

*ZIM.ecalibration = ZIM schema entity name for equipment_calibrations*

### Table: cost_entries (ZIM.ecost_entries)

| Field Name         | ZIM Name           | Type      | Length | Dec | Required | Description                                   |
|--------------------|--------------------|-----------|--------|-----|----------|-----------------------------------------------|
| id                 | fcost_id           | INT       | 5      | 0   | Yes      | Cost entry identifier                         |
| project_id         | fproject_id        | INT       | 5      | 0   | Yes      | Parent project                                |
| experiment_id      | fexperiment_id     | INT       | 5      | 0   | No       | Related experiment                            |
| labour_entry_id    | flabour_id         | INT       | 5      | 0   | No       | Linked labour entry if derived                |
| category           | fcost_category     | VARCHAR   | 50     | 0   | Yes      | Labour, Materials, Subcontractor, Equipment   |
| sred_category      | fcost_sred_cat     | VARCHAR   | 100    | 0   | Yes      | SR&ED classification bucket                   |
| amount             | fcost_amount       | NUMERIC   | 10     | 2   | Yes      | Cost amount                                   |
| currency           | fcost_currency     | VARCHAR   | 10     | 0   | Yes      | Currency code (e.g., CAD)                     |
| incurred_on        | fcost_incurred     | DATE      | 8      | 0   | Yes      | Cost date                                     |
| vendor_name        | fcost_vendor_name  | VARCHAR   | 200    | 0   | No       | Vendor/supplier                               |
| reference_note     | fcost_reference    | VARCHAR   | 200    | 0   | No       | Supporting details                            |
| created_at         | fcost_created      | DATETIME  | -      | 0   | Yes      | Record creation time                          |
| updated_at         | fcost_modified     | DATETIME  | -      | 0   | No       | Last update time                              |

*ZIM.ecost_entries = ZIM schema entity name for cost_entries*

### Table: data_collection_records (ZIM.edata)

| Field Name               | ZIM Name           | Type      | Length | Dec | Required | Description                                   |
|--------------------------|--------------------|-----------|--------|-----|----------|-----------------------------------------------|
| id                       | fdata_id           | INT       | 5      | 0   | No       | Data record identifier                        |
| project_id               | fproject_id        | INT       | 5      | 0   | Yes      | Parent project                                |
| experiment_id            | fexperiment_id     | INT       | 5      | 0   | Yes      | Experiment reference                          |
| experiment_version_id    | fexp_ver_id        | INT       | 5      | 0   | Yes      | Version context                               |
| equipment_asset_id       | fequipment_id      | INT       | 5      | 0   | Yes      | Source equipment if instrument-derived         |
| facility_space_id        | fstation_id        | INT       | 5      | 0   | No       | Physical zone where collection happened        |
| collected_by_person_id   | fpeople_id         | INT       | 5      | 0   | Yes      | Operator/collector                            |
| source_type              | fdata_source_type  | VARCHAR   | 50     | 0   | Yes      | Manual, Instrument, Imported                  |
| metric_name              | fdata_label        | VARCHAR   | 120    | 0   | Yes      | Measurement label                             |
| metric_value             | fdata_value        | NUMERIC   | 15     | 2   | Yes      | Numeric value                                 |
| metric_unit              | fdata_unit         | VARCHAR   | 40     | 0   | Yes      | Unit string                                   |
| min_valid                | fdata_min          | NUMERIC   | 15     | 2   | No       | Lower validation bound                        |
| max_valid                | fdata_max          | NUMERIC   | 15     | 2   | No       | Upper validation bound                        |
| out_of_range_flag        | fdata_outlier      | VARCHAR   | 15     | 0   | Yes      | Validation failure flag                       |
| observed_at              | fdata_time         | DATETIME  | -      | 0   | Yes      | Measurement time                              |
| ingestion_batch_id       | fdata_batch_id     | VARCHAR   | 80     | 0   | No       | Import/ingestion batch reference              |
| raw_payload              | fdata_payload      | VARCHAR   | 255    | 0   | No       | Original row/payload for traceability         |
| created_at               | fdata_created      | DATETIME  | -      | 0   | Yes      | Record creation time                          |
| updated_at               | fdata_updated      | DATETIME  | -      | 0   | No       | Last update time                              |

*ZIM.edata = ZIM schema entity name for data_collection_records*

### Table: equipment_assets (ZIM.eequipment)

| Field Name         | ZIM Name           | Type      | Length | Dec | Required | Description                                   |
|--------------------|--------------------|-----------|--------|-----|----------|-----------------------------------------------|
| id                 | fequipment_id      | INT       | 5      | 0   | Yes      | Equipment identifier                          |
| asset_tag          | fequip_asset_tag   | VARCHAR   | 80     | 0   | Yes      | Internal asset code                           |
| equipment_name     | fequip_name        | VARCHAR   | 200    | 0   | Yes      | Asset name                                    |
| equipment_type     | fequip_type        | VARCHAR   | 80     | 0   | Yes      | Sensor, Reactor, Pump, Meter, Other           |
| manufacturer       | fequiq_manufactur  | VARCHAR   | 120    | 0   | No       | Manufacturer                                  |
| model_number       | fequip_model       | VARCHAR   | 120    | 0   | No       | Model reference                               |
| serial_number      | fequip_serial_num  | VARCHAR   | 120    | 0   | No       | Serial identifier                             |
| owner_person_id    | fequip_owner       | INT       | 5      | 0   | No       | Responsible person                            |
| location           | fequip_location    | VARCHAR   | 150    | 0   | No       | Storage/usage location                        |
| status             | fequip_status      | VARCHAR   | 20     | 0   | Yes      | Active, Maintenance, Retired                  |
| commissioned_on    | fequip_commission  | DATE      | 8      | 0   | No       | Date introduced                               |
| created_at         | fequip_created     | DATETIME  | -      | 0   | Yes      | Record creation time                          |
| updated_at         | fequip_modified    | DATETIME  | -      | 0   | No       | Last update time                              |

*ZIM.eequipment = ZIM schema entity name for equipment_assets*

### Table: evidence_files (ZIM.eevidence)

| Field Name         | ZIM Name           | Type      | Length | Dec | Required | Description                                   |
|--------------------|--------------------|-----------|--------|-----|----------|-----------------------------------------------|
| id                 | fevidence_id       | INT       | 5      | 0   | Yes      | Evidence file identifier                      |
| project_id         | fproject_id        | INT       | 5      | 0   | Yes      | Parent project                                |
| experiment_id      | fexperiment_id     | INT       | 5      | 0   | Yes      | Related experiment                            |
| labour_entry_id    | flabour_id         | INT       | 5      | 0   | Yes      | Linked labour evidence                        |
| file_name          | fevidence_file_nam | VARCHAR   | 255    | 0   | No       | Original file name                            |

*ZIM.eevidence = ZIM schema entity name for evidence_files*

### Table: experiment_versions (ZIM.eexper_version)

| Field Name         | ZIM Name           | Type      | Length | Dec | Required | Description                                   |
|--------------------|--------------------|-----------|--------|-----|----------|-----------------------------------------------|
| id                 | fexp_ver_id        | INT       | 5      | 0   | Yes      | Version entry identifier                      |
| experiment_id      | fexperiment_id     | INT       | 5      | 0   | Yes      | Related experiment                            |
| version_number     | fexp_ver_versi     | INT       | 5      | 0   | Yes      | Sequential version                            |
| changed_by_user_id | fexp_ver_chang     | INT       | 5      | 0   | Yes      | User making the change                        |
| change_summary     | fexp_ver_ch_sum    | VARALPHA  | 255    | 0   | Yes      | Description of change                         |
| payload_snapshot   | fexp_ver_payload   | VARCHAR   | 10     | 0   | Yes      | Serialized full snapshot                      |
| created_at         | fexp_ver_create    | DATETIME  | -      | 0   | Yes      | Version timestamp                             |

*ZIM.eexper_version = ZIM schema entity name for experiment_versions*

### Table: experiments (ZIM.eexperiment)

| Field Name             | ZIM Name            | Type      | Length | Dec | Required | Description                                   |
|------------------------|---------------------|-----------|--------|-----|----------|-----------------------------------------------|
| id                     | fexp_id             | INT       | 5      | 0   | Yes      | Unique experiment identifier                  |
| project_id             | fproject_id         | INT       | 5      | 0   | Yes      | Parent project                                |
| title                  | fexp_title          | VARCHAR   | 255    | 0   | Yes      | Experiment name                               |
| uncertainty_statement  | fexpt_statement     | ALPHA     | 255    | 0   | Yes      | Technological uncertainty for SR&ED           |
| hypothesis             | fexp_hypothesis     | ALPHA     | 255    | 0   | Yes      | Testable technical hypothesis                 |
| procedure              | fexp_procedure      | ALPHA     | 255    | 0   | No       | Experimental method                           |
| expected_result        | fexp_expec_result   | ALPHA     | 255    | 0   | No       | Predicted outcome                             |
| actual_result          | fexp_actual_result  | ALPHA     | 255    | 0   | No       | Observed outcome                              |
| analysis               | fexp_analysis       | ALPHA     | 255    | 0   | No       | Human technical interpretation                |
| advancement            | fexpt_advancement   | ALPHA     | 255    | 0   | No       | Claimed technical advancement                 |
| status                 | fexp_status         | VARCHAR   | 50     | 0   | No       | Planned, In Progress, Completed, Failed       |
| started_at             | fexp_start          | DATETIME  | -      | 0   | No       | Execution start                               |
| completed_at           | fexp_complete       | DATETIME  | -      | 0   | No       | Execution completion                          |
| created_by_user_id     | fexp_created_by     | INT       | 5      | 0   | No       | Creator                                       |
| created_at             | fexp_created        | DATE      | 8      | 0   | Yes      | Record creation time                          |
| updated_at             | fexp_updated        | DATE      | 8      | 0   | No       | Last update time                              |
| experiment_version_id  | fexp_ver_id         | INT       | 5      | 0   | Yes      | Version context                               |

*ZIM.eexperiment = ZIM schema entity name for experiments*

### Table: facility_spaces (ZIM.efacility)

| Field Name         | ZIM Name           | Type      | Length | Dec | Required | Description                                   |
|--------------------|--------------------|-----------|--------|-----|----------|-----------------------------------------------|
| id                 | fstation_id        | INT       | 5      | 0   | No       | Facility space identifier                     |
| project_id         | fproject_id        | INT       | 5      | 0   | Yes      | Parent project                                |
| space_code         | fstation_code      | VARCHAR   | 80     | 0   | Yes      | Internal zone code                            |
| space_name         | fstation_name      | VARCHAR   | 150    | 0   | Yes      | Human-readable zone name                      |
| space_type         | fstaion_type       | VARCHAR   | 60     | 0   | Yes      | Bench, Storage, TestArea, SafetyZone, Office  |
| status             | fstation_status    | VARCHAR   | 50     | 0   | Yes      | Active, Restricted, OutOfService              |
| notes              | fstation_notes     | VARCHAR   | 255    | 0   | No       | Setup notes and constraints                   |
| created_at         | fstation_created   | DATETIME  | -      | 0   | Yes      | Record creation time                          |
| updated_at         | fstation_updated   | DATETIME  | -      | 0   | No       | Last update time                              |

*ZIM.efacility = ZIM schema entity name for facility_spaces*

### Table: labour_entries (ZIM.elabour_entries)

| Field Name         | ZIM Name             | Type      | Length | Dec | Required | Description                                   |
|--------------------|----------------------|-----------|--------|-----|----------|-----------------------------------------------|
| id                 | flabour_id           | INT       | 5      | 0   | Yes      | Labour entry identifier                       |
| project_id         | fproject_id          | INT       | 5      | 0   | Yes      | Parent project                                |
| experiment_id      | fexperiment_id       | INT       | 5      | 0   | Yes      | Related experiment                            |
| person_id          | fpeople_id           | INT       | 5      | 0   | Yes      | Person performing the work                    |
| activity_category  | flabour_activity     | VARCHAR   | 80     | 0   | Yes      | Experimentation, Analysis, Documentation, Dev |
| sred_category      | flabour_sred_cat     | VARCHAR   | 50     | 0   | No       | SR&ED category mapping                        |
| work_date          | flabour_work_date    | DATE      | 8      | 0   | Yes      | Date of work                                  |
| hours              | flabour_hours        | NUMERIC   | 8      | 2   | Yes      | Hours worked                                  |
| rate_used          | flabour_rate         | NUMERIC   | 10     | 2   | Yes      | Applied rate at time of entry                 |
| amount             | flabour_amount       | NUMERIC   | 10     | 2   | Yes      | Computed labour amount                        |
| description        | flabour_desc         | VARCHAR   | 255    | 0   | Yes      | Work performed summary                        |
| evidence_ref       | flabour_references   | VARCHAR   | 120    | 0   | No       | Reference to supporting artifact              |
| approved_by_user_id| flabour_approval     | INT       | 5      | 0   | No       | Reviewer/approver                             |
| created_at         | flabour_created      | DATETIME  | -      | 0   | No       | Record creation time                          |
| updated_at         | flabour_modified     | DATETIME  | -      | 0   | No       | Last update time                              |

*ZIM.elabour_entries = ZIM schema entity name for labour_entries*

### Table: people (ZIM.epeople)

| Field Name         | ZIM Name           | Type      | Length | Dec | Required | Description                                   |
|--------------------|--------------------|-----------|--------|-----|----------|-----------------------------------------------|
| id                 | fpeople_id         | INT       | 5      | 0   | Yes      | Person record identifier                      |
| user_id            | fpeople_user_id    | INT       | 5      | 0   | No       | Linked system user account, if any            |
| full_name          | fpeople_name       | VARCHAR   | 200    | 0   | Yes      | Legal/preferred full name                     |
| role_title         | fpeople_role       | VARCHAR   | 50     | 0   | Yes      | Functional role                               |
| sred_eligibility   | fpeople_sred       | VARCHAR   | 50     | 0   | Yes      | Eligible, Partially Eligible, Not Eligible    |
| irap_eligibility   | fpeople_irap       | ALPHA     | 10     | 0   | No       | Eligible, Review Required, Not Eligible       |
| hourly_rate        | fpeople_rate_hr    | ALPHA     | 10     | 0   | No       | Standard cost/claim rate                      |
| currency           | fpeople_currency   | ALPHA     | 10     | 0   | No       | Currency code (default CAD)                   |
| active_from        | fpeople_start      | DATE      | 8      | 0   | Yes      | Start date                                    |
| active_to          | fpeopl_finish      | DATE      | 8      | 0   | No       | End date if inactive                          |
| notes              | fpeople_notes      | VARCHAR   | 255    | 0   | No       | Classification notes                          |
| created_at         | fpeople_created    | DATETIME  | -      | 0   | Yes      | Record creation time                          |
| updated_at         | fpeople_updated    | DATETIME  | -      | 0   | No       | Last update time                              |
| email              | fpeople_email      | VARCHAR   | 255    | 0   | Yes      | Email address                                 |
| password_hash      | fpeople_pasword    | VARCHAR   | 255    | 0   | Yes      | Hashed password                               |
| status             | fpeople_status     | VARCHAR   | 255    | 0   | No       | Status (Active, Disabled, etc.)               |
| last_login_at      | fpeople_last_log   | DATETIME  | -      | 0   | No       | Last login timestamp                          |

*ZIM.epeople = ZIM schema entity name for people*

### Table: procurements (ZIM.eprocurement)

| Field Name         | ZIM Name           | Type      | Length | Dec | Required | Description                                   |
|--------------------|--------------------|-----------|--------|-----|----------|-----------------------------------------------|
| id                 | fprocurement_id    | INT       | 5      | 0   | Yes      | Procurement identifier                        |
| project_id         | fproject_id        | INT       | 5      | 0   | Yes      | Parent project                                |
| item_name          | fprocur_item       | VARCHAR   | 200    | 0   | Yes      | Purchased item/service                        |
| vendor_name        | fprocur_vendor     | VARCHAR   | 200    | 0   | Yes      | Vendor                                        |
| category           | fprocur_category   | VARCHAR   | 80     | 0   | Yes      | Chemical, Equipment, Service, Software        |
| amount             | fprocur_amount     | NUMERIC   | 12     | 0   | Yes      | Cost amount                                   |
| currency           | fprocur_currency   | VARCHAR   | 10     | 0   | Yes      | Currency code                                 |
| ordered_on         | fprocur_ordered    | DATE      | 8      | 0   | No       | Order date                                    |
| received_on        | fprocur_received   | DATE      | 8      | 0   | No       | Receipt date                                  |
| status             | fprocur_status     | VARCHAR   | 50     | 0   | Yes      | Requested, Ordered, Received, Closed          |
| linked_cost_entry_id| fcost_id          | INT       | 5      | 0   | Yes      | Related cost entry                            |
| created_at         | fprocur_created    | DATETIME  | -      | 0   | Yes      | Record creation time                          |
| updated_at         | fprocur_updated    | DATETIME  | -      | 0   | No       | Last update time                              |

*ZIM.eprocurement = ZIM schema entity name for procurements*

### Table: projects (ZIM.eprojects)

| Field Name         | ZIM Name           | Type      | Length | Dec | Required | Description                                   |
|--------------------|--------------------|-----------|--------|-----|----------|-----------------------------------------------|
| id                 | fproject_id        | INT       | 5      | 0   | Yes      | Unique project identifier                     |
| code               | fproject_code      | VARCHAR   | 50     | 0   | Yes      | Human-readable project code                   |
| name               | fproject_title     | VARCHAR   | 255    | 0   | Yes      | Project title                                 |
| objective          | fproject_objective | ALPHA     | 10     | 0   | Yes      | R&D objective statement                       |
| scope_summary      | fproject_scope     | ALPHA     | 10     | 0   | No       | Scope baseline summary                        |
| status             | fproject_status    | VARCHAR   | 50     | 0   | Yes      | Draft, Active, On Hold, Closed                |
| start_date         | fproject_start     | DATE      | 8      | 0   | No       | Planned/actual start                          |
| end_date           | fproject_end       | DATE      | 8      | 0   | No       | Planned/actual completion                     |
| created_at         | fproject_create    | DATETIME  | -      | 0   | Yes      | Record creation time                          |
| updated_at         | fprojec_update     | DATETIME  | -      | 0   | No       | Last update time                              |

*ZIM.eprojects = ZIM schema entity name for projects*

### Table: reports (ZIM.ereports)

| Field Name         | ZIM Name           | Type      | Length | Dec | Required | Description                                   |
|--------------------|--------------------|-----------|--------|-----|----------|-----------------------------------------------|
| id                 | freport_id         | INT       | 5      | 0   | No       | Report identifier                             |
| project_id         | fproject_id        | INT       | 5      | 0   | Yes      | Parent project                                |
| report_type        | freport_type       | VARCHAR   | 80     | 0   | No       | SRED, IRAP, FedDev, NGen, OCE, BDC            |
| period_start       | freport_start      | DATE      | 8      | 0   | No       | Report period start                           |
| period_end         | freport_end        | DATE      | 8      | 0   | No       | Report period end                             |
| generated_by_user_id| fpeople_id        | INT       | 5      | 0   | Yes      | Generator                                     |
| generation_method  | freport_generated  | VARCHAR   | 50     | 0   | Yes      | Manual, Automated, AI Assisted                |
| file_path          | freport_file_path  | VARCHAR   | 50     | 0   | No       | Output file location                          |
| status             | freport_status     | VARCHAR   | 50     | 0   | Yes      | Draft, Final                                  |
| created_at         | freport_created    | DATE      | 8      | 0   | Yes      | Record creation time                          |
| updated_at         | freport_updated    | DATE      | 8      | 0   | No       | Last update time                              |

*ZIM.ereports = ZIM schema entity name for reports*

### Table: risks (ZIM.erisks)

| Field Name         | ZIM Name           | Type      | Length | Dec | Required | Description                                   |
|--------------------|--------------------|-----------|--------|-----|----------|-----------------------------------------------|
| id                 | frisk_id           | INT       | 5      | 0   | Yes      | Risk identifier                               |
| project_id         | fproject_id        | INT       | 5      | 0   | Yes      | Parent project                                |
| title              | frisk_title        | VARCHAR   | 200    | 0   | Yes      | Risk title                                    |
| description        | frisk_description  | VARCHAR   | 255    | 0   | Yes      | Risk statement                                |
| probability_score  | frisk_probability  | VARCHAR   | 255    | 0   | Yes      | Probability score (1-5)                       |
| impact_score       | frisk_impact_score | VARCHAR   | 255    | 0   | Yes      | Impact score (1-5)                            |
| mitigation_plan    | frisk_mitigation   | VARCHAR   | 255    | 0   | No       | Mitigation actions                            |
| owner_person_id    | fpeople_id         | INT       | 5      | 0   | No       | Risk owner                                    |
| status             | frisk_status       | VARCHAR   | 50     | 0   | Yes      | Open, Monitoring, Closed                      |
| created_at         | frisk_created      | DATETIME  | -      | 0   | Yes      | Record creation time                          |
| updated_at         | frisk_updated      | DATETIME  | -      | 0   | No       | Last update time                              |

*ZIM.erisks = ZIM schema entity name for risks*

### Table: sprints (ZIM.esprint)

| Field Name         | ZIM Name           | Type      | Length | Dec | Required | Description                                   |
|--------------------|--------------------|-----------|--------|-----|----------|-----------------------------------------------|
| id                 | fsprint_id         | INT       | 5      | 0   | Yes      | Sprint identifier                             |
| project_id         | fproject_id        | INT       | 5      | 0   | Yes      | Parent project                                |
| sprint_name        | fsprint_name       | VARCHAR   | 120    | 0   | Yes      | Sprint label                                  |
| goal               | fsprint_goal       | VARCHAR   | 255    | 0   | No       | Sprint objective                              |
| start_date         | fsprint_start      | DATE      | 8      | 0   | Yes      | Sprint start                                  |
| end_date           | fsprint_end        | DATE      | 8      | 0   | Yes      | Sprint end                                    |
| status             | fsprint_status     | VARCHAR   | 50     | 0   | Yes      | Planned, Active, Closed                       |
| created_at         | fsprint_created    | DATETIME  | -      | 0   | Yes      | Record creation time                          |
| updated_at         | fspring_updated    | DATETIME  | -      | 0   | No       | Last update time                              |

*ZIM.esprint = ZIM schema entity name for sprints*

### Table: facility_readiness_checks (ZIM.estation_check)

| Field Name         | ZIM Name             | Type      | Length | Dec | Required | Description                                   |
|--------------------|----------------------|-----------|--------|-----|----------|-----------------------------------------------|
| id                 | fstation_check_id    | INT       | 5      | 0   | Yes      | Readiness check identifier                    |
| facility_space_id  | fstation_id          | INT       | 5      | 0   | Yes      | Checked space                                 |
| checked_by_person_id| fpeople_id          | INT       | 5      | 0   | Yes      | Person performing check                       |
| check_date         | fstation_chk_date    | DATE      | 8      | 0   | Yes      | Date of check                                 |
| setup_status       | fstation_status      | VARCHAR   | 50     | 0   | Yes      | Ready, Conditional, NotReady                  |
| safety_status      | fstation_safety      | VARCHAR   | 50     | 0   | Yes      | Pass, Warning, Fail                           |
| checklist_version  | fstation_chklist_v   | VARCHAR   | 40     | 0   | No       | Checklist template version                    |
| findings           | fstation_notes       | VARCHAR   | 255    | 0   | No       | Findings summary                              |
| corrective_action  | fstation_correctiv   | VARCHAR   | 255    | 0   | No       | Required actions                              |
| evidence_file_id   | fevidence_id         | INT       | 5      | 0   | No       | Supporting evidence reference                 |
| valid_until        | fstation_expiry      | DATE      | 8      | 0   | No       | Readiness expiry                              |
| created_at         | fstation_created     | DATETIME  | -      | 0   | Yes      | Record creation time                          |
| updated_at         | fstation_updated     | DATETIME  | -      | 0   | No       | Last update time                              |

*ZIM.estation_check = ZIM schema entity name for facility_readiness_checks*

---
