# DATA DICTIONARY 2026

## Purpose
Define canonical field-level meaning for core entities before implementation.

## Conventions
- Primary key: `id` (INT AUTO_INCREMENT).
- Foreign keys: `<entity>_id`.
- Timestamps: `created_at`, `updated_at` (UTC recommended).
- Soft delete (if required): `deleted_at`.

## Table: projects
| Field | Type | Required | Description |
|---|---|---|---|
| id | INT | Yes | Unique project identifier |
| name | VARCHAR(255) | Yes | Project title |
| objective | TEXT | Yes | R&D objective statement |
| scope_summary | TEXT | No | Scope baseline summary |
| status | VARCHAR(50) | Yes | Draft, Active, On Hold, Closed |
| created_at | TIMESTAMP | Yes | Record creation time |
| updated_at | TIMESTAMP | No | Last update time |

## Table: experiments
| Field | Type | Required | Description |
|---|---|---|---|
| id | INT | Yes | Unique experiment identifier |
| project_id | INT | Yes | Parent project |
| hypothesis | TEXT | Yes | Testable technical hypothesis |
| procedure | TEXT | Yes | Experimental method |
| expected_result | TEXT | Yes | Predicted outcome |
| actual_result | TEXT | No | Observed outcome |
| analysis | TEXT | No | Technical interpretation |
| advancement | TEXT | No | Claimed advancement from results |
| uncertainty_statement | TEXT | Yes | Technological uncertainty description |
| created_at | TIMESTAMP | Yes | Record creation time |
| updated_at | TIMESTAMP | No | Last update time |

## Table: experiment_versions
| Field | Type | Required | Description |
|---|---|---|---|
| id | INT | Yes | Version entry identifier |
| experiment_id | INT | Yes | Related experiment |
| version_number | INT | Yes | Sequential version |
| changed_by_user_id | INT | Yes | User making the change |
| change_summary | TEXT | Yes | Description of changes |
| payload_snapshot | JSON/TEXT | Yes | Serialized version snapshot |
| created_at | TIMESTAMP | Yes | Version timestamp |

## Table: cost_entries
| Field | Type | Required | Description |
|---|---|---|---|
| id | INT | Yes | Cost entry identifier |
| project_id | INT | Yes | Parent project |
| experiment_id | INT | No | Related experiment if applicable |
| category | VARCHAR(50) | Yes | Labour, Materials, Subcontractor |
| sred_category | VARCHAR(50) | Yes | SR&ED classification bucket |
| amount | DECIMAL(12,2) | Yes | Cost amount |
| currency | VARCHAR(10) | Yes | Currency code (e.g., CAD) |
| incurred_on | DATE | Yes | Cost date |
| reference_note | TEXT | No | Supporting details |
| created_at | TIMESTAMP | Yes | Record creation time |

## Table: evidence_files
| Field | Type | Required | Description |
|---|---|---|---|
| id | INT | Yes | Evidence file identifier |
| project_id | INT | Yes | Parent project |
| experiment_id | INT | No | Related experiment |
| file_name | VARCHAR(255) | Yes | Original file name |
| file_path | TEXT | Yes | Storage path |
| file_hash | VARCHAR(128) | Yes | Integrity hash |
| mime_type | VARCHAR(120) | Yes | File media type |
| uploaded_by_user_id | INT | Yes | Uploader |
| uploaded_at | TIMESTAMP | Yes | Upload timestamp |

## Open Schema Questions
- Confirm if `payload_snapshot` is native JSON in chosen DB engine.
- Confirm retention policy fields needed for evidence records.
- Confirm whether `deleted_at` is required on regulated entities.
