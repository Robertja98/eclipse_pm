# Eclipse PM - Mixed Bed Resin R&D Management Application

Comprehensive project management and compliance tracking system for R&D activities, integrating SR&ED 2026 compliance, PMBOK planning, and Agile execution for grant/funding eligibility.

## Current Phase
✅ **Planning Complete** | **Sprint 1 Ready to Start (March 16, 2026)**

---

## 📋 Planning & Governance Documents

### Core Plans
- **[PROJECT_PLAN_2026.md](PROJECT_PLAN_2026.md)** – Master project plan, tech stack (PHP 8 + ZIM), core objectives, and 6-sprint roadmap
- **[REQUIREMENTS_BASELINE_2026.md](REQUIREMENTS_BASELINE_2026.md)** – Complete requirements matrix (27 functional + 10 compliance + 10 non-functional)
- **[PRECODING_EXECUTION_PLAN_2026.md](PRECODING_EXECUTION_PLAN_2026.md)** – WBS, milestones, risk register, and phase gates

### Architecture & Technical Decisions
- **[ARCHITECTURE_DECISIONS_2026.md](ARCHITECTURE_DECISIONS_2026.md)** – ADRs including ADR-004 on Repository Pattern + ZimXClient adapter
- **[DATA_DICTIONARY_2026.md](DATA_DICTIONARY_2026.md)** – Complete schema: 8 core tables (users, people, projects, experiments, costs, evidence, labour, versions)
- **[TEST_STRATEGY_2026.md](TEST_STRATEGY_2026.md)** – Unit, integration, acceptance, and compliance test approach

### Compliance & Governance
- **[FUNDING_DELIVERABLE_MATRIX_2026.md](FUNDING_DELIVERABLE_MATRIX_2026.md)** – Mapping of system outputs to IRAP, FedDev, NGen, OCE/OCI, BDC requirements
- **[PERSONNEL_GOVERNANCE_2026.md](PERSONNEL_GOVERNANCE_2026.md)** – Role, responsibility, and personnel data standards
- **[AI_REVIEW_POLICY_2026.md](AI_REVIEW_POLICY_2026.md)** – AI output validation and human-in-the-loop governance
- **[FACILITY_SETUP_STANDARDS_2026.md](FACILITY_SETUP_STANDARDS_2026.md)** – Physical space/equipment setup and zone definitions
- **[DATA_VALIDATION_RULES_2026.md](DATA_VALIDATION_RULES_2026.md)** – Field validation, constraints, and data quality rules

---

## 🛠️ Implementation Guides (Sprint 1 Ready)

### ZIM Database & Architecture
- **[ZIM_IMPLEMENTATION_GUIDE.md](ZIM_IMPLEMENTATION_GUIDE.md)** – Complete technical reference for ZIM integration
  - ZIM architecture overview (session-persistent data sets)
  - ZimXClient adapter pattern with examples
  - Repository interface + ZIM implementation patterns
  - Full SQL schema and table definitions
  - Service layer examples
  - Seed script for founder bootstrap
  - Unit & integration test patterns

- **[ZIM_QUICK_REFERENCE.md](ZIM_QUICK_REFERENCE.md)** – Cheat sheet for ZIM developers
  - FIND/INSERT/UPDATE/DELETE command patterns
  - JOIN, aggregate, and date function examples
  - Common repository patterns (pagination, search, existence checks)
  - Performance tips and debugging guides

### Sprint 1 Execution
- **[SPRINT_1_PLAN.md](SPRINT_1_PLAN.md)** – Detailed 2-week roadmap for authentication & identity
  - 6 stories with full acceptance criteria and tasks
  - Story point estimates and daily standup template
  - Risk mitigation, testing strategy, success metrics
  - Implementation checklist and next steps

---

## 🚀 Quick Start (Sprint 1)

### Prerequisites
- PHP 8+ with CLI
- ZIM engine (localhost:6002)
- Composer (for testing framework)

### Day 1-2: Infrastructure
```bash
# Set up ZIM engine on localhost:6002
# Create ZIM schema from /zim/schema.zim
# Create ZimXClient adapter
# Test ZIM connection from PHP
```

See [ZIM_IMPLEMENTATION_GUIDE.md](ZIM_IMPLEMENTATION_GUIDE.md#story-s1-00-set-up-zim-infrastructure--zimxclient) for setup checklist.

### Day 2-5: Authentication
Implement user registration, login, and password hashing via `AuthenticationService` and `UserRepository`.

### Day 5-8: Identity Management
Create people records, bootstrap founder account, and implement role-based access control.

### Day 8-10: Testing & Documentation
Integration tests, seed script, API docs, and deployment checklist.

---

## 📊 Documentation Status

| Document | Status | Purpose |
|----------|--------|---------|
| PROJECT_PLAN_2026.md | ✅ Approved | Vision, tech stack, objectives |
| REQUIREMENTS_BASELINE_2026.md | ✅ Approved | 27 FR + 10 CR + 10 NFR |
| ARCHITECTURE_DECISIONS_2026.md | ✅ Approved | 4 ADRs (MVC, ZIM/MariaDB, audit, ZimXClient) |
| PRECODING_EXECUTION_PLAN_2026.md | ✅ Approved | WBS, milestones, phase gates |
| ZIM_IMPLEMENTATION_GUIDE.md | ✅ Complete | Technical patterns for ZIM integration |
| ZIM_QUICK_REFERENCE.md | ✅ Complete | Command cheat sheet for developers |
| SPRINT_1_PLAN.md | ✅ Ready | 6 stories, 80 hours, 2 weeks |

---

## 🎯 Success Criteria (Sprint 1 Gate)
- [ ] Founder can register and login
- [ ] Founder person record created via seed script
- [ ] Team members can be added (admin interface)
- [ ] RBAC prevents unauthorized access
- [ ] 90%+ code coverage on services
- [ ] Zero SQL injection vulnerabilities
- [ ] All integration tests passing against real ZIM

---

## 📁 Repository Structure (In Development)
```
/workspaces/eclipse_pm/
├── app/
│   ├── Http/Controllers/
│   ├── Domain/Services/
│   ├── Domain/Entities/
│   └── Infrastructure/Persistence/
│       ├── Repositories/
│       └── ZimX/
├── views/
├── public/
├── zim/
│   └── schema.zim
├── scripts/
│   └── seed.php
├── tests/
│   ├── unit/
│   └── integration/
└── config/
    ├── zimx.php
    └── app.php
```

---

## 📞 Contact & Governance
- **Owner:** Founder / Principal Investigator
- **Technical Lead:** Founder
- **Approval Authority:** TBD
- **Next Review:** Sprint 1 End-of-Sprint Review (March 29, 2026)

---

## 📖 Next Steps Before Coding

1. ✅ Read [PROJECT_PLAN_2026.md](PROJECT_PLAN_2026.md) for full vision
2. ✅ Review [ZIM_IMPLEMENTATION_GUIDE.md](ZIM_IMPLEMENTATION_GUIDE.md) – Architecture and patterns
3. ⏭️ Install ZIM engine on localhost:6002
4. ⏭️ Deploy schema to ZIM (from `/zim/schema.zim`)
5. ⏭️ Create repository interfaces in `app/Infrastructure/Persistence/Repositories/`
6. ⏭️ Begin Sprint 1 Story sequence (S1-00 → S1-06)

---

**Last Updated:** 2026-03-09 | **Version:** 1.0 | **Status:** Planning Complete