# REQUIREMENTS BASELINE 2026

---
**Document Version:** v1.0  
**Status:** Draft  
**Owner:** Founder  
**Approver:** TBD  
**Approval Date:** TBD  
**Last Updated:** 2026-03-09  

---

## 1. Purpose
Create a clear, auditable baseline of what the system must do and what is currently available in the repository before coding starts.

## 2. Current-State Summary
### Repository status
- Current phase: Architecture finalized, Sprint 1 ready to start.
- Implementation code: Not started.
- Current team baseline: Single founder/operator (1 person) at project start.
- AI integration baseline: Planned for data ingestion, interpretation, and recommendation support.
- Facility baseline: Physical shop/lab setup and space zoning requirements to be documented before build.

### Tech Stack (Finalized)
- **Frontend:** HTML5, CSS3, JavaScript (no framework)
- **Backend:** PHP 8+ (modular MVC style)
- **Database:** ZIM/Zim-X (session-persistent data sets)
- **Architecture Pattern:** Repository Pattern with ZimXClient adapter
- **ORM/Query Layer:** Custom ZimX adapter (see `ZIM_IMPLEMENTATION_GUIDE.md`)
- **Testing:** PHPUnit for unit/integration tests
- **Hosting:** localhost:6002 (ZIM on port 6002, PHP on port 80/8080)
- Available artifacts:
  - `PROJECT_PLAN_2026.md`
  - `PRECODING_EXECUTION_PLAN_2026.md`
  - `ARCHITECTURE_DECISIONS_2026.md`
  - `DATA_DICTIONARY_2026.md`
  - `TEST_STRATEGY_2026.md`
  - `FUNDING_DELIVERABLE_MATRIX_2026.md`
  - `PERSONNEL_GOVERNANCE_2026.md`
  - `AI_REVIEW_POLICY_2026.md`
  - `FACILITY_SETUP_STANDARDS_2026.md`
  - `DATA_VALIDATION_RULES_2026.md`
  - `ZIM_IMPLEMENTATION_GUIDE.md` (Technical reference for ZIM adapter pattern)
  - `ZIM_QUICK_REFERENCE.md` (ZIM command cheat sheet)
  - `SPRINT_1_PLAN.md` (Detailed Sprint 1 roadmap with 6 stories)

### Maturity scale
- `Defined`: Requirement documented and approved in planning docs.
- `Designed`: Architecture/schema/test approach specified.
- `Built`: Implementation completed.
- `Verified`: Tests executed and passed.

## 3. Functional Requirements Baseline
| ID | Requirement | Priority | Current Maturity | Notes |
|---|---|---|---|---|
| FR-001 | Manage projects (create, view, update status/objective/scope) | Must | Defined | Planned in Sprint 1 |
| FR-002 | Manage experiments with structured workflow (hypothesis, method, expected, actual, analysis, advancement) | Must | Defined | Core SR&ED requirement |
| FR-003 | Track experiment versions and change history | Must | Designed | `experiment_versions` in data dictionary |
| FR-004 | Log failures explicitly and link to advancement records | Must | Defined | Required for SR&ED 2026 audits |
| FR-005 | Upload and retain evidence files with metadata | Must | Designed | `evidence_files` table drafted |
| FR-006 | Record activity-based costs (labour, materials, subcontractor) | Must | Designed | `cost_entries` table drafted |
| FR-007 | Manage sprint backlog and sprint execution views | Should | Defined | Planned in Sprint 4 |
| FR-008 | Maintain PMBOK risk register with scoring and mitigation | Must | Defined | PMBOK planner scope |
| FR-009 | Track procurements and vendor records | Should | Defined | PMBOK procurement scope |
| FR-010 | Generate SR&ED-ready narrative report output | Must | Defined | Planned in Sprint 6 |
| FR-011 | Generate funding/grant package outputs (IRAP/FedDev/NGen/OCE/OCI/BDC) | Must | Designed | See FUNDING_DELIVERABLE_MATRIX_2026.md |
| FR-012 | Enforce authentication and role-based access | Must | Defined | Planned in Sprint 1 |
| FR-013 | Register and manage lab equipment (asset ID, type, location, status, owner) | Must | Defined | Required for experiment traceability |
| FR-014 | Track equipment calibration/maintenance history and due dates | Must | Defined | Compliance and data quality control |
| FR-015 | Link equipment used to each experiment run and version | Must | Designed | Requires relationship in data dictionary |
| FR-016 | Capture structured data collection entries (manual and instrument-derived) | Must | Defined | Includes timestamp, source, and operator |
| FR-017 | Validate collected data against required fields, units, and ranges | Must | Designed | Data quality gate before report exports |
| FR-018 | Support import/export of experimental datasets (CSV/JSON) with provenance metadata | Should | Defined | Supports reproducibility and audit review |
| FR-019 | Manage people records (founder/staff/contractors) with role, rate, and eligibility metadata | Must | Defined | Funding and payroll traceability |
| FR-020 | Track labour effort entries by person, experiment/project, and activity category | Must | Designed | Required for SR&ED and IRAP labour claims |
| FR-021 | Support single-person operation mode while remaining multi-user ready | Must | Defined | Bootstrap as one user without blocking future growth |
| FR-022 | Run AI-assisted analysis during/after data ingestion to classify trends, failures, and improvement opportunities | Must | Defined | Human review required before final sign-off |
| FR-023 | Store AI outputs (summary, confidence, recommendations, anomaly flags) linked to dataset and experiment version | Must | Designed | Needed for reproducibility and auditability |
| FR-024 | Allow user feedback/override of AI recommendations and capture final decision rationale | Must | Defined | Keeps human-in-the-loop governance |
| FR-025 | Define and manage facility spaces/zones (bench, storage, test area, safety zone) for operations | Must | Defined | Required for proper setup and operational control |
| FR-026 | Link experiments, equipment, and data collection events to facility location/zone | Must | Designed | Provides context for reproducibility and audits |
| FR-027 | Maintain setup readiness and safety checks for active workspaces | Must | Defined | Supports IRAP/CRA governance evidence |

## 4. Compliance Requirements (SR&ED 2026)
| ID | Requirement | Priority | Current Maturity | Acceptance Signal |
|---|---|---|---|---|
| CR-001 | Capture technological uncertainty explicitly per experiment | Must | Defined | Field present and mandatory |
| CR-002 | Preserve immutable, timestamped activity records | Must | Designed | Append-only audit strategy ADR proposed |
| CR-003 | Maintain full version history for experiment/document changes | Must | Designed | Version entities/events implemented |
| CR-004 | Support machine-readable exports (JSON, CSV) | Must | Defined | Export endpoints implemented and validated |
| CR-005 | Produce CRA-friendly PDF narrative | Must | Defined | Narrative format validated against checklist |
| CR-006 | Log unsuccessful/failed experiments with traceable outcomes | Must | Defined | Failure workflow present and reportable |
| CR-007 | Preserve equipment context and data provenance for each experimental result | Must | Defined | Result records include equipment and source references |
| CR-008 | Retain personnel attribution for technical and cost activities (who did what, when) | Must | Defined | Every claim-linked activity references a person record |
| CR-009 | Preserve AI decision trace (model/version, input source, output time, reviewer decision) for claim-linked records | Must | Defined | AI-assisted records are fully auditable for CRA/IRAP review |
| CR-010 | Preserve facility context for claim-linked activities (where work was performed and setup state) | Must | Defined | Activity records are traceable to a physical zone and readiness status |

## 5. Reporting and Funding Requirements
| ID | Requirement | Priority | Current Maturity | Notes |
|---|---|---|---|---|
| RF-001 | Milestone-based project reporting | Must | Defined | In project/execution plan |
| RF-002 | Commercialization roadmap report | Should | Defined | Structure identified, template pending |
| RF-003 | Technical merit narrative report | Must | Defined | Template pending |
| RF-004 | Budget burn-down report | Must | Defined | Cost module dependency |
| RF-005 | Program-specific export packages | Must | Designed | See FUNDING_DELIVERABLE_MATRIX_2026.md |

## 6. Non-Functional Requirements
| ID | Requirement | Priority | Current Maturity | Target |
|---|---|---|---|---|
| NFR-001 | Modular MVC maintainability | Must | Designed | Clear controller/service/model boundaries |
| NFR-002 | Audit-grade traceability | Must | Designed | Append-only audit + evidence integrity |
| NFR-003 | Data integrity for uploaded evidence | Must | Designed | Hash + timestamp + uploader metadata |
| NFR-004 | Security baseline (auth/session/roles) | Must | Defined | Enforced in all protected routes |
| NFR-005 | Testability (unit/integration/acceptance) | Must | Designed | Test strategy documented |
| NFR-006 | Local development simplicity | Should | Defined | PHP 8+ localhost runtime |
| NFR-007 | Data quality and reproducibility for collected measurements | Must | Defined | Required unit consistency, validation, and provenance |
| NFR-008 | Funding-grade personnel auditability | Must | Defined | Labour and role records are exportable and immutable in history |
| NFR-009 | AI output reliability controls | Must | Defined | Confidence thresholds, anomaly flags, and mandatory human approval checkpoints |
| NFR-010 | Facility setup traceability and safety readiness | Must | Defined | Workspace state and checks are documented and reportable |

## 7. What We Currently Have vs Missing
### Currently have (ready)
- Project vision, objectives, and architecture direction.
- Pre-coding governance, milestones, and phase-gate structure.
- Complete ADRs with finalized decisions (data store + audit strategy accepted).
- Complete data dictionary with all planned tables (core, people, equipment, AI, facility, planning, access/reporting).
- Test strategy and critical compliance test cases.
- Funding deliverable matrix (`FUNDING_DELIVERABLE_MATRIX_2026.md`).
- Personnel governance rules (`PERSONNEL_GOVERNANCE_2026.md`).
- AI review policy (`AI_REVIEW_POLICY_2026.md`).
- Facility setup standards (`FACILITY_SETUP_STANDARDS_2026.md`).
- Data validation rules (`DATA_VALIDATION_RULES_2026.md`).
- Approval metadata headers on all planning documents.

### Missing (before implementation gate)
- Sprint 1 backlog with story-level acceptance criteria and estimates.
- Formal approval signatures on planning documents (status currently "Draft").

## 8. Phase-Gate Readiness Statement
### Ready now
- All governance and policy documentation complete.
- Requirements baseline finalized.
- Data dictionary covers all planned entities.
- Architecture decisions finalized.
- Ready to create Sprint 1 backlog and begin implementation planning.

### Not ready yet
- Not ready to start coding until Sprint 1 backlog is defined and planning documents are formally approved.

## 9. Immediate Actions (Ordered)
1. ✓ ~~Create and approve `FUNDING_DELIVERABLE_MATRIX_2026.md`~~ (Complete)
2. ✓ ~~Add approval/version metadata header to all planning documents~~ (Complete)
3. ✓ ~~Finalize ADR-002 and ADR-003 from `Proposed` to `Accepted`~~ (Complete)
4. ✓ ~~Expand `DATA_DICTIONARY_2026.md` for all core planned tables~~ (Complete)
5. ✓ ~~Define validation rules for data collection~~ (Complete - see `DATA_VALIDATION_RULES_2026.md`)
6. ✓ ~~Define personnel data standards~~ (Complete - see `PERSONNEL_GOVERNANCE_2026.md`)
7. ✓ ~~Define AI review policy~~ (Complete - see `AI_REVIEW_POLICY_2026.md`)
8. ✓ ~~Define facility setup standards~~ (Complete - see `FACILITY_SETUP_STANDARDS_2026.md`)
9. **Produce Sprint 1 backlog with requirement traceability** (`FR-*`, `CR-*`, `NFR-*`) - NEXT ACTION
10. **Obtain formal approval on all planning documents** (move from Draft to Approved status)
