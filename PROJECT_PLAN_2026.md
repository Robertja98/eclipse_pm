# PROJECT PLAN 2026 - Mixed Bed Resin R&D Management Application

---
**Document Version:** v1.0  
**Status:** Draft  
**Owner:** Founder  
**Approver:** TBD  
**Approval Date:** TBD  
**Last Updated:** 2026-03-09  

---

## 1. Project Purpose
This application supports R&D activities for improving mixed bed resin treatment. It integrates PMBOK planning, Agile execution, SR&ED 2026 compliance, and BDO/grant funding requirements. It provides experiment tracking, cost attribution, evidence storage, and automated reporting.

## 2. Tech Stack (Finalized for Implementation)
- **Frontend:** HTML5, CSS3, JavaScript (vanilla, no framework)
- **Backend:** PHP 8+ (modular MVC architecture)
- **Database:** ZIM/Zim-X (session-persistent data sets on localhost:6002)
- **Architecture Pattern:** Repository Pattern with ZimXClient adapter
- **Persistence Layer:** Custom ZimXClient for ZIM command execution and session management
- **Testing:** PHPUnit with mock repositories and real ZIM integration tests
- **Hosting:** localhost (PHP on port 80/8080, ZIM on port 6002)

### ZIM Advantages for This Project
- **Session-persistent data sets** align with sprint/batch experimental execution
- **Built-in audit/version support** for SR&ED compliance
- **Simple local deployment** suitable for R&D lab environment
- **No complex ORM dependency** suitable for custom compliance workflows

For implementation details, see:
- `ZIM_IMPLEMENTATION_GUIDE.md` – Architecture, repository patterns, examples
- `ZIM_QUICK_REFERENCE.md` – Common ZIM commands and repository patterns
- `SPRINT_1_PLAN.md` – Detailed 2-week roadmap with 6 stories

## 3. Iteration Roadmap (6 Sprints)
See `SPRINT_1_PLAN.md` for detailed technical breakdown of authentication and identity management in Sprint 1.

## 4. Core Objectives
- Document resin treatment R&D in a SR&ED compliant format.
- Track experiments, failures, iterations, and advancements.
- Attribute labour, materials, and subcontractor costs to activities.
- Provide PMBOK aligned planning tools.
- Support Agile sprints and backlog management.
- Generate SR&ED T661 ready narratives.
- Produce BDO/IRAP/FedDev/NGen ready project packages.
- Maintain immutable, timestamped evidence.

## 5. 2026 SR&ED Requirements (Integrated Into System Design)
### Mandatory compliance features
- Explicit documentation of technological uncertainty.
- Structured experiment workflow:
  - Hypothesis
  - Method
  - Expected vs. actual results
  - Analysis
  - Advancement achieved
- Timestamped entries with immutable logs.
- Version history for all experiments and documents.
- Activity based costing tied to SR&ED categories.
- Machine readable exports (JSON + CSV).
- CRA friendly PDF narrative generator.
- Failure logging (required for 2026 audits).

### Expanded 2026 eligibility
- Process optimization qualifies if uncertainty is documented.
- Automation and software development qualify if technical uncertainty exists.
- Data driven experimentation is explicitly recognized.

## 6. BDO & Grant Funding Requirements (2026 Standards)
### Required deliverables
- Milestone based project plan.
- Commercialization roadmap.
- Technical merit narrative.
- Budget burn down tracking.
- Risk register with probability/impact scoring.
- Evidence of iterative experimentation.
- Exportable reports for:
  - IRAP
  - FedDev
  - NGen
  - OCE/OCI
  - BDC Tech Loans

## 7. PMBOK Integration
### Integration Management
Project charter, change log, versioning.

### Scope Management
Resin treatment objectives, requirements repository.

### Schedule Management
Milestones, Gantt style timeline, sprint calendar.

### Cost Management
Labour, materials, subcontractors, activity based costing.

### Quality Management
Acceptance criteria, validation protocols.

### Resource Management
Roles, permissions, workload distribution.

### Communications Management
Stakeholder dashboards, automated weekly summaries.

### Risk Management
Risk register, scoring, mitigation tracking.

### Procurement Management
Vendor tracking, purchase logs, chemical/equipment procurement.

## 8. Agile Execution Layer
### Sprint Structure
Two week sprints with backlog, reviews, retrospectives.

### Backlog Types
- Technical uncertainties
- Experiments
- Development tasks
- Documentation tasks

### Experiment Workflow
- Hypothesis
- Procedure
- Expected result
- Actual result
- Data attachments
- Interpretation
- Advancement

## 7. System Architecture (PHP + ZIM)
### MVC Inspired Structure
- Models: Database interaction
- Views: HTML templates
- Controllers: Request handling
- Services: Business logic
- Helpers: Utility functions

### Key Modules
- SR&ED Engine
- Experiment Notebook
- PMBOK Planner
- Agile Sprint Manager
- Cost Attribution Engine
- Evidence Vault
- Report Generator
- Authentication & Roles

## 8. Recommended Folder Structure (Copy into VS Code)
```text
/project_root
|
|-- /public
|   |-- index.php
|   |-- /css
|   |-- /js
|   `-- /uploads
|
|-- /app
|   |-- /Controllers
|   |   |-- ProjectController.php
|   |   |-- ExperimentController.php
|   |   |-- SprintController.php
|   |   |-- CostController.php
|   |   |-- ReportController.php
|   |   `-- AuthController.php
|   |
|   |-- /Models
|   |   |-- Project.php
|   |   |-- Experiment.php
|   |   |-- Sprint.php
|   |   |-- Cost.php
|   |   |-- Evidence.php
|   |   `-- User.php
|   |
|   |-- /Views
|   |   |-- /layouts
|   |   |-- /project
|   |   |-- /experiment
|   |   |-- /sprint
|   |   |-- /cost
|   |   `-- /report
|   |
|   |-- /Services
|   |   |-- SredService.php
|   |   |-- ReportService.php
|   |   |-- CostService.php
|   |   `-- EvidenceService.php
|   |
|   `-- /Helpers
|       |-- Database.php
|       |-- Validator.php
|       `-- Auth.php
|
|-- /config
|   `-- config.php
|
|-- /zim
|   `-- schema.zim
|
`-- composer.json
```

## 9. Database Schema (MyZIM/MariaDB)
### Key Tables
- projects
- experiments
- experiment_versions
- sprints
- backlog_items
- cost_entries
- evidence_files
- risks
- procurements
- users
- reports

### Example: experiments table
```sql
id INT AUTO_INCREMENT PRIMARY KEY,
project_id INT,
hypothesis TEXT,
procedure TEXT,
expected_result TEXT,
actual_result TEXT,
analysis TEXT,
advancement TEXT,
created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
updated_at TIMESTAMP
```

## 10. API Style Routing (PHP)
### Experiments
- POST /experiment/create
- GET /experiment/view?id=
- POST /experiment/update
- GET /project/experiments?id=

### Sprints
- POST /sprint/create
- GET /sprint/view?id=
- POST /sprint/add-backlog

### Costs
- POST /cost/add
- GET /project/costs?id=

### Reports
- POST /report/sred
- GET /report/view?id=

## 11. Development Roadmap (Sprints)
### Sprint 1 - Foundation
- Routing
- Authentication
- Database schema
- Project creation

### Sprint 2 - Experiment Engine
- Experiment CRUD
- Evidence uploads
- Versioning

### Sprint 3 - PMBOK Planner
- Scope, schedule, cost, risk modules

### Sprint 4 - Agile Execution
- Backlog
- Sprint board
- Burndown chart

### Sprint 5 - Cost Attribution
- Labour/material tracking
- Activity based costing

### Sprint 6 - Reporting Engine
- SR&ED narrative generator
- Grant package generator

## 12. Success Criteria
- Produces CRA ready SR&ED documentation.
- Supports PMBOK planning and Agile execution.
- Maintains immutable, timestamped evidence.
- Generates grant ready reports.
- Provides a clean PHP/ZIM architecture for long term maintainability.
