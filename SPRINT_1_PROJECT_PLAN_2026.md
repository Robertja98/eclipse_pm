# SPRINT 1 PROJECT PLAN 2026

---
**Document Version:** v1.0  
**Status:** Draft  
**Owner:** Founder  
**Approver:** TBD  
**Approval Date:** TBD  
**Last Updated:** 2026-03-09  

---

## 1. Sprint Overview

### Sprint Duration
- **Start Date:** 2026-03-16 (Monday)
- **End Date:** 2026-03-29 (Sunday)
- **Duration:** 2 weeks
- **Sprint Days:** 10 working days

### Sprint Goal
Establish authentication system and core identity management using ZIM database. Enable login workflow, role-based access, and foundational person/team records.

### Technical Foundation
Sprint 1 focuses on **ZIM infrastructure and authentication**:
- Set up ZimXClient adapter and session management
- Implement repository pattern for user/people persistence
- Create auth service with bcrypt hashing and session management
- Bootstrap founder account via seed script
- Establish RBAC framework for future feature protection

For detailed technical implementation patterns, see `ZIM_IMPLEMENTATION_GUIDE.md` and `SPRINT_1_PLAN.md`.

### Success Criteria
- All 8 stories below completed and tested
- No critical severity defects remaining
- Local `localhost` deployment functional
- Manual smoke test pass (basic workflows)
- Foundation ready for Sprint 2 (PMBOK planner additions)

## 2. Sprint Backlog (8 Stories)

### Story S1-001: User Authentication and Session Management
**Requirement Map:** FR-012 (Enforce authentication and role-based access)  
**Acceptance Criteria:**
- [ ] Login form accessible at `/` or `/login`
- [ ] Founder account created with single-person bootstrap
- [ ] Session persists across page load (cookies/session store working)
- [ ] Logout clears session
- [ ] Invalid credentials rejected with user-friendly message
- [ ] Password hashing implemented (bcrypt or similar)

**Tasks:**
1. Set up routing structure (`/login`, `/logout`, `/dashboard`)
2. Create users table and seed founder account
3. Implement login controller with credential validation
4. Implement session/auth middleware
5. Add logout endpoint
6. Create login view (HTML form)

**Estimate:** 8 story points  
**Owner:** Founder  
**Acceptance Test:** Manual login/logout workflow test

---

### Story S1-002: Project CRUD (Create, Read, Update)
**Requirement Map:** FR-001 (Manage projects)  
**Acceptance Criteria:**
- [ ] Create project form accepts: name, objective, scope summary
- [ ] Project created with status = "Draft"
- [ ] Project list view shows all projects
- [ ] Project detail view shows all fields
- [ ] Edit project updates fields
- [ ] Delete project (soft delete with `deleted_at`)
- [ ] All timestamps recorded (`created_at`, `updated_at`)

**Tasks:**
1. Create Project model and database table (if not exists)
2. Create ProjectController with CRUD endpoints
3. Create project service for business logic
4. Create project views (list, detail, form)
5. Add form validation
6. Add flash messages (create/update/delete success)

**Estimate:** 5 story points  
**Owner:** Founder  
**Acceptance Test:** Create → list → read → update → delete workflow

---

### Story S1-003: Experiment Creation and Structured Workflow
**Requirement Map:** FR-002 (Manage experiments with structured workflow), CR-001 (Capture uncertainty explicitly)  
**Acceptance Criteria:**
- [ ] Create experiment form accepts: project, hypothesis, procedure, expected result, uncertainty statement
- [ ] uncertainty_statement is mandatory field
- [ ] Experiment created with status = "Planned"
- [ ] Experiment links to parent project
- [ ] Experiment list filters by project
- [ ] Experiment detail shows all fields
- [ ] Edit updates fields (but tracks versions via S1-004)

**Tasks:**
1. Create Experiment model and database table
2. Create ExperimentController with CRUD endpoints
3. Create experiment service
4. Create experiment views (list, detail, form)
5. Add uncertainty_statement validation (non-empty)
6. Add project dropdown in form

**Estimate:** 5 story points  
**Owner:** Founder  
**Acceptance Test:** Create experiment with uncertainty → list → read

---

### Story S1-004: Experiment Versioning (Change History)
**Requirement Map:** FR-003 (Track versions), CR-003 (Maintain version history)  
**Acceptance Criteria:**
- [ ] On experiment update, prior version saved to `experiment_versions` table
- [ ] Version record includes: version_number, changed_by_user_id, change_summary, payload_snapshot
- [ ] Version history view shows all versions in chronological order
- [ ] Users can view prior version snapshot (read-only)
- [ ] Payload snapshot preserves full experiment state at time of change

**Tasks:**
1. Create experiment_versions table
2. Add versioning trigger/service logic on experiment update
3. Create change_summary field and collection logic
4. Create version history view
5. Test version snapshot restore (read-only display)

**Estimate:** 5 story points  
**Owner:** Founder  
**Acceptance Test:** Update experiment 3x → view version history → verify snapshots

---

### Story S1-005: Evidence File Upload and Storage
**Requirement Map:** FR-005 (Upload evidence files), CR-002 (Immutable timestamped records), NFR-003 (Data integrity)  
**Acceptance Criteria:**
- [ ] Evidence upload form linked to experiments
- [ ] File upload to server storage (public/uploads/ or similar)
- [ ] File hash (SHA-256) computed and stored
- [ ] File metadata captured: name, MIME type, size, upload timestamp, uploader
- [ ] Evidence file list view shows all uploads for an experiment
- [ ] Download file preserves original name
- [ ] No file overwrites (unique naming or ID-based store)

**Tasks:**
1. Create evidence_files table
2. Add file upload endpoint (/experiment/{id}/upload)
3. Implement file hashing logic (SHA-256)
4. Create file storage strategy (local filesystem)
5. Create evidence list view
6. Add download endpoint with integrity check

**Estimate:** 5 story points  
**Owner:** Founder  
**Acceptance Test:** Upload 3 files → verify hash → download → compare integrity

---

### Story S1-006: Labour Entry and Time Tracking (Founder)
**Requirement Map:** FR-020 (Track labour effort), FR-019 (Manage people), CR-008 (Personnel attribution)  
**Acceptance Criteria:**
- [ ] Labour entry form accepts: date, hours, activity category, description, experiment (optional)
- [ ] Activity category dropdown restricted to claimable types (Experimentation, Analysis, etc.)
- [ ] Founder person record auto-populated as `person_id`
- [ ] Labour amount auto-calculated (hours × founder rate)
- [ ] Labour entry linked to project and/or experiment
- [ ] Labour list shows all entries for a project
- [ ] Entries sortable by date
- [ ] No labour entry deletion (append-only); soft delete if necessary

**Tasks:**
1. Create people table with founder seed record
2. Create labour_entries table
3. Create LabourController with entry form and list
4. Add activity_category dropdown (enum or controlled vocab)
5. Implement labour amount calculation service
6. Add form validation (hours > 0, valid date, category selected)

**Estimate:** 5 story points  
**Owner:** Founder  
**Acceptance Test:** Create 5 labour entries (various activities) → list → verify amounts

---

### Story S1-007: Cost Entry Tracking (Materials/Subcontractor)
**Requirement Map:** FR-006 (Record activity-based costs)  
**Acceptance Criteria:**
- [ ] Cost entry form accepts: category (Labour, Materials, Subcontractor), amount, currency, date, project, experiment (optional), description
- [ ] Category dropdown enforces allowed values
- [ ] Currency defaults to CAD
- [ ] Cost linked to project (mandatory) and experiment (optional)
- [ ] Cost list shows all entries for a project
- [ ] Cost total summary by category
- [ ] Entries support soft delete (not hard delete)

**Tasks:**
1. Create cost_entries table
2. Create CostController with CRUD endpoints
3. Create cost service with category validation
4. Create cost views (form, list, summary chart/table)
5. Add date picker to form
6. Add category enum/dropdown

**Estimate:** 4 story points  
**Owner:** Founder  
**Acceptance Test:** Create labour + materials + subcontractor entries → view list → verify total

---

### Story S1-008: Dashboard and Navigation
**Requirement Map:** System usability  
**Acceptance Criteria:**
- [ ] Dashboard shows: recent projects, recent experiments, labour summary (YTD hours), cost summary (YTD amount)
- [ ] Navigation menu links to: Projects, Experiments, Labour, Costs, Settings
- [ ] All pages have consistent header/footer
- [ ] User name and logout link visible in header
- [ ] Mobile-friendly layout (basic responsive CSS)
- [ ] No JavaScript errors in browser console

**Tasks:**
1. Create DashboardController
2. Create dashboard view with panels
3. Create navigation layout template
4. Add summary logic (recent projects, labour YTD, cost YTD)
5. Add CSS for basic responsive layout
6. Test mobile viewport

**Estimate:** 4 story points  
**Owner:** Founder  
**Acceptance Test:** Manual walkthrough of dashboard and nav on desktop + mobile

---

## 3. Definition of Done (All Stories)
- [ ] Code peer-reviewed (self-review by founder is acceptable for Sprint 1)
- [ ] Unit tests written and passing (minimum 70% coverage for models/services)
- [ ] Integration tests for workflows (login → create project → create experiment → upload file)
- [ ] Manual acceptance test passed
- [ ] Acceptance criteria all checked
- [ ] No console errors or warnings
- [ ] Database migrations applied and tested
- [ ] Changes committed to git with clear messages

## 4. Daily Work Schedule

### Week 1 (2026-03-16 to 2026-03-20)
| Day | Focus | Stories | Punchline |
|---|---|---|---|
| Mon 3/16 | Setup & Auth | S1-001 | Dev env ready, routing established, login form working |
| Tue 3/17 | Project CRUD | S1-002 | Projects create/read/update/delete, list view |
| Wed 3/18 | Experiment Core | S1-003 | Experiment CRUD, uncertainty_statement required |
| Thu 3/19 | Versioning | S1-004 | Version history tracking and snapshot view |
| Fri 3/20 | Evidence Upload | S1-005 | File upload, hash, list with download |

### Week 2 (2026-03-23 to 2026-03-29)
| Day | Focus | Stories | Punchline |
|---|---|---|---|
| Mon 3/23 | Labour Tracking | S1-006 | Labour entry form, founder time logging, list |
| Tue 3/24 | Cost Tracking | S1-007 | Cost entry, category breakdown, totals |
| Wed 3/25 | Dashboard | S1-008 | Dashboard view, navigation, responsive layout |
| Thu 3/26 | Integration Testing | All | Full workflow test: login → project → experiment → evidence → labour → costs |
| Fri 3/27 | Bug Fixes & Polish | All | Console errors, edge cases, demo readiness |
| Sat 3/28 | Contingency | Blockers | If any story slips, buffer day |
| Sun 3/29 | **Sprint Review** | - | Manual smoke test, prepare Sprint 2 planning |

## 5. Technical Implementation Notes

### Database Setup
- Use MyZIM (ZIM document/database format) as primary data store
- Initialize ZIM schema file with tables: users, people, projects, experiments, experiment_versions, evidence_files, labour_entries, cost_entries
- Seed with founder user and founder person record
- Use ZIM PHP SDK/wrapper for data access layer (abstraction per ADR-002)

### Code Organization (per PROJECT_PLAN_2026.md)
```
/public
  /index.php          (router entry point)
  /css/app.css        (basic styling)
  /js/app.js          (if needed for UI)
  /uploads/           (evidence files storage)

/app
  /Controllers
    AuthController.php
    ProjectController.php
    ExperimentController.php
    LabourController.php
    CostController.php
  
  /Models
    User.php
    Project.php
    Experiment.php
    ExperimentVersion.php
    EvidenceFile.php
    Labour.php
    Cost.php
  
  /Services
    AuthService.php
    ProjectService.php
    ExperimentService.php
    LabourService.php
    CostService.php
    ZimDataProvider.php  (ZIM abstraction layer)
  
  /Views
    /layouts/app.html
    /auth/login.html
    /projects/list.html
    /projects/detail.html
    /projects/form.html
    /experiments/list.html
    /experiments/detail.html
    /experiments/form.html
    /experiments/versions.html
    /evidence/list.html
    /labour/form.html
    /labour/list.html
    /costs/form.html
    /costs/list.html
    /dashboard/index.html

/zim
  /schema.zim        (ZIM database file)

/tests
  /unit/
    ExperimentTest.php
    LabourTest.php
    CostTest.php
  /integration/
    LabourWorkflowTest.php
```

### No Sprint 1 Requirements
- PMBOK planner (sprints, risks, procurements) → Sprint 4
- SR&ED narrative generator → Sprint 6
- Grant report exports → Sprint 6
- AI analysis integration → Sprint 3 (optional early if desired)
- Multi-user support, permissions → Sprint 2+
- Facility setup UI → Sprint 2

### Testing Strategy (Sprint 1)
- Unit tests for models (validation, calculations)
- Integration tests for workflows (create → read tests)
- Manual smoke tests for UI/UX
- No automated UI tests (Selenium) in Sprint 1

## 6. Risk Mitigation

| Risk | Impact | Mitigation |
|---|---|---|
| PHP framework choice delays dev | High | Use simple procedural PHP with helper functions; avoid heavy frameworks |
| Database schema mismatch | Medium | Run migrations before story starts; validate schema against data dictionary |
| File upload security | High | Restrict file types, use unique storage names, scan for malware (ClamAV optional) |
| Session timeout issues | Medium | Set reasonable timeout (2 hours), add warning at 90 min |
| Versioning performance | Low | Payload snapshot as JSON; if too large, archive old versions separately |

## 7. Definition of Sprint Done
- [ ] All 8 stories completed and accepted
- [ ] Integration test pass (full workflow)
- [ ] Zero critical defects
- [ ] Code committed to git
- [ ] Documentation updated (e.g., any schema changes noted)
- [ ] Sprint retrospective conducted
- [ ] Sprint 2 planning scheduled

## 8. Acceptance Test Checklist (Full Workflow)

**Test Scenario: Single Founder R&D Session**
1. [ ] Login as founder → Dashboard shows (recent projects, recent experiments, labour, costs totals)
2. [ ] Create project "Resin Treatment 2026" → Listed on dashboard and in Projects view
3. [ ] Create experiment "Mixed bed regeneration efficiency" under project → Shows uncertainty statement mandatory
4. [ ] Upload 3 evidence files (PDF chart, CSV data, photo) → Files listed with hashes
5. [ ] Log labour: 4 hours experimentation, 2 hours analysis → Total shown as 6 hours
6. [ ] Log costs: $150 materials, $100 chemical → Total shown as $250
7. [ ] Edit experiment hypothesis → New version created and visible in version history
8. [ ] Logout and login again → Session restored correctly
9. [ ] No console errors, no broken links, responsive on mobile

## 9. Sprint Success Metrics
- **Velocity:** 41 story points (8 stories × avg 5 points)
- **Burn-down:** Stories should be completed by Day 7 (Fri 3/27), with 1 day buffer
- **Quality:** Zero critical defects at sprint review
- **Completeness:** 100% of stories accepted

## 10. Next Steps After Sprint 1
1. Sprint 1 Review (2026-03-29)
2. Prepare Sprint 2 backlog (PMBOK planning, multi-user support)
3. Begin Sprint 2 development (2026-04-06)

---

**Created:** 2026-03-09  
**Ready for Review:** Yes  
**Ready for Development Start:** Yes (pending approval)
