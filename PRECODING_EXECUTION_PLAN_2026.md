# PRE-CODING EXECUTION PLAN 2026

## 1. Objective
Define a delivery-ready plan before coding begins for the Mixed Bed Resin R&D Management Application, ensuring SR&ED 2026, PMBOK, Agile, and funding requirements are traceable from requirement to evidence.

## 2. Scope Baseline
### In Scope
- Web application planning for experiment management, costing, evidence vault, and reporting.
- SR&ED-compliant experiment and failure documentation workflows.
- PMBOK planning modules (scope, schedule, cost, risk, procurement, communication).
- Agile sprint and backlog planning model.
- Grant/funding reporting preparation (IRAP, FedDev, NGen, OCE/OCI, BDC).

### Out of Scope (Phase 1)
- Production deployment and cloud infrastructure automation.
- Advanced BI analytics beyond required operational dashboards.
- Third-party LIMS/ERP integrations.

## 3. Planning Assumptions
- Runtime environment is local development (`localhost`) with PHP 8+.
- Data store is MyZIM or MariaDB with interchangeable repository/model layer.
- Team can execute two-week sprints with regular planning/review ceremonies.
- Compliance evidence will be captured continuously, not retroactively.

## 4. Governance Model
### Delivery Method
- Hybrid PMBOK + Agile.
- PMBOK defines control baselines and governance artifacts.
- Agile defines iterative execution and incremental delivery.

### Cadence
- Sprint length: 2 weeks.
- Weekly stakeholder summary: status, risks, budget, next milestones.
- End-of-sprint review: increment demo + compliance evidence checkpoint.

### Decision Ownership
- Product/Research Lead: requirements and scientific direction.
- Technical Lead: architecture and code-level design decisions.
- Compliance Lead: SR&ED evidence completeness and audit readiness.
- Finance/Operations Lead: costing model and procurement controls.

## 5. Work Breakdown Structure (WBS)
### WBS 1.0 Program Setup
- 1.1 Finalize scope statement and success criteria.
- 1.2 Define governance and communication plan.
- 1.3 Establish traceability strategy and document templates.

### WBS 2.0 Solution Design
- 2.1 Confirm MVC module boundaries.
- 2.2 Define routing conventions and controller responsibilities.
- 2.3 Define data model and table ownership.
- 2.4 Define evidence storage and retention rules.

### WBS 3.0 Compliance Design
- 3.1 Define SR&ED experiment schema and mandatory fields.
- 3.2 Define immutable audit log behavior.
- 3.3 Define report outputs (JSON, CSV, PDF narrative).
- 3.4 Define failure logging and advancement traceability.

### WBS 4.0 Delivery Planning
- 4.1 Build release roadmap from Sprint 1 to Sprint 6.
- 4.2 Define backlog taxonomy and prioritization policy.
- 4.3 Define Definition of Ready and Definition of Done.
- 4.4 Define test strategy and acceptance gates.

### WBS 5.0 Funding Readiness
- 5.1 Define deliverable matrix for BDO and grant programs.
- 5.2 Define technical merit narrative structure.
- 5.3 Define commercialization roadmap sections.
- 5.4 Define budget burn-down and risk reporting format.

## 6. Milestones and Exit Criteria
### M1 - Planning Baseline Approved
Exit criteria:
- Scope baseline signed off.
- Governance model approved.
- Initial risk register populated.

### M2 - Architecture Baseline Approved
Exit criteria:
- Module boundaries documented.
- Folder structure and coding standards approved.
- Routing and data model reviewed.

### M3 - Compliance Baseline Approved
Exit criteria:
- SR&ED data capture fields finalized.
- Audit log requirements finalized.
- Reporting output requirements finalized.

### M4 - Sprint Plan Approved
Exit criteria:
- Sprint 1 to Sprint 3 backlog drafted and estimated.
- Acceptance criteria and test strategy approved.
- Capacity plan and role assignment confirmed.

### M5 - Funding Pack Framework Approved
Exit criteria:
- Deliverable map for IRAP/FedDev/NGen/OCE/OCI/BDC approved.
- Narrative templates ready.
- Budget and risk reporting templates ready.

## 7. Traceability Matrix (Requirement -> Module -> Evidence)
| Requirement | Module/Artifact | Evidence Produced |
|---|---|---|
| Technological uncertainty documentation | Experiment Notebook, SR&ED Engine | Hypothesis entries, uncertainty statements, timestamps |
| Expected vs actual outcomes | Experiment Notebook | Experiment records + version history |
| Failure logging | SR&ED Engine, Evidence Vault | Failure records, corrective notes, audit log |
| Activity-based costing | Cost Attribution Engine | Cost entries by activity category |
| PMBOK risk tracking | PMBOK Planner | Risk register with probability/impact/mitigation |
| Agile execution proof | Agile Sprint Manager | Sprint board snapshots, backlog history, burndown |
| Grant package readiness | Report Generator | Program-specific export package and narratives |

## 8. Definition of Ready (DoR)
A backlog item is ready when:
- Problem statement is clear and testable.
- SR&ED relevance is identified (if applicable).
- Acceptance criteria are measurable.
- Data requirements are identified.
- Dependencies and risks are listed.
- Estimated effort is assigned.

## 9. Definition of Done (DoD)
A backlog item is done when:
- Functional behavior matches acceptance criteria.
- Evidence artifacts are attached (if compliance relevant).
- Audit fields and timestamps are validated.
- Cost attribution is recorded where applicable.
- Documentation is updated.
- Reviewer approval is captured.

## 10. Risk Register (Initial)
| ID | Risk | Probability | Impact | Mitigation | Owner |
|---|---|---|---|---|---|
| R1 | Incomplete SR&ED evidence during execution | Medium | High | Mandatory templates + sprint evidence gate | Compliance Lead |
| R2 | Scope growth beyond sprint capacity | High | Medium | Strict backlog triage and release slicing | Product Lead |
| R3 | Data model churn after development starts | Medium | High | Architecture sign-off before coding | Technical Lead |
| R4 | Cost tracking inconsistency | Medium | High | Standardized cost categories and validations | Finance Lead |
| R5 | Reporting outputs miss grant format requirements | Low | High | Early mock exports and stakeholder review | Product + Compliance |

## 11. Pre-Coding Deliverables Checklist
- `PROJECT_PLAN_2026.md` validated and baselined.
- Architecture decision log created.
- Data dictionary drafted for core tables.
- API route catalog documented.
- Test strategy drafted (unit, integration, acceptance).
- Risk register approved.
- Funding deliverable matrix approved.

## 12. Phase Gate Approval Record
| Gate | Approver | Date | Status | Notes |
|---|---|---|---|---|
| Planning Baseline |  |  | Pending |  |
| Architecture Baseline |  |  | Pending |  |
| Compliance Baseline |  |  | Pending |  |
| Sprint Plan |  |  | Pending |  |
| Funding Framework |  |  | Pending |  |

## 13. Immediate Next Documentation Tasks
1. Create `ARCHITECTURE_DECISIONS_2026.md` with ADR-style entries for routing, ORM/data access, and audit-log strategy.
2. Create `DATA_DICTIONARY_2026.md` for all core tables and required fields.
3. Create `TEST_STRATEGY_2026.md` including compliance-focused acceptance tests.
4. Create `FUNDING_DELIVERABLE_MATRIX_2026.md` mapped to IRAP/FedDev/NGen/OCE/OCI/BDC.
5. Baseline all planning docs with version `v1.0` and approval metadata.
