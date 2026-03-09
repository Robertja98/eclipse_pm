# PERSONNEL GOVERNANCE 2026

---
**Document Version:** v1.0  
**Status:** Draft  
**Owner:** Founder  
**Approver:** TBD  
**Approval Date:** TBD  
**Last Updated:** 2026-03-09  

---

## 1. Purpose
Define personnel classification, role transitions, and eligibility rules for SR&ED, IRAP, and other funding claims to ensure audit-defensible labour attribution.

## 2. Current Baseline

### Founding Team (2026-03-09)
- **Team Size:** 1 person (founder/operator)
- **Operating Mode:** Single-person bootstrap with multi-user system readiness
- **Initial Roles:** Founder performs all technical, administrative, and reporting work

### System User Model
- **User Account:** Single `founder` account initially
- **Person Record:** Linked to `people` table with full eligibility metadata
- **Scalability:** System supports future addition of employees, contractors, advisors

## 3. Person Types and Definitions

### Founder
**Definition:** Company founder performing R&D and operational work.  
**SR&ED Eligibility:** Eligible if directly engaged in R&D activities (not purely administrative).  
**IRAP Eligibility:** Eligible for technical work; administrative work may require review.  
**Tracking Requirements:**
- Labour entries must distinguish R&D vs. administrative activity.
- Hourly rate or salary-equivalent must be documented.
- Work must be attributable to specific experiments or projects.

### Employee
**Definition:** Hired staff on payroll with T4 reporting.  
**SR&ED Eligibility:** Eligible if >90% time on R&D; partially eligible if 50-90%; not eligible if <50%.  
**IRAP Eligibility:** Eligible for full direct R&D labour; review required for partial.  
**Tracking Requirements:**
- Timesheets or effort logs required.
- Role title and responsibility scope documented.
- Salary/hourly rate recorded.

### Contractor
**Definition:** External individual or firm engaged under contract (T4A reporting).  
**SR&ED Eligibility:** Generally not eligible unless contract explicitly assigns IP to claimant and contractor is arm's length.  
**IRAP Eligibility:** Review required; eligible if direct technical contribution and proper contract structure.  
**Tracking Requirements:**
- Contract terms and deliverables documented.
- Effort attribution method (e.g., hourly logs, deliverable-based).
- Invoice and payment records linked to cost entries.

### Advisor
**Definition:** Part-time or occasional guidance provider (not performing direct R&D work).  
**SR&ED Eligibility:** Not eligible (advisory work does not qualify as direct R&D).  
**IRAP Eligibility:** Not eligible for labour claims; may be eligible under specific advisory program terms.  
**Tracking Requirements:**
- Engagement scope and deliverables documented.
- Time/effort logged for transparency but not claimed.

## 4. Role Title Standards

| Role Title | Typical Activities | SR&ED Eligibility Default | IRAP Eligibility Default |
|---|---|---|---|
| Founder / Principal Investigator | R&D design, experimentation, analysis | Eligible | Eligible |
| Research Scientist | Hypothesis development, experiment execution, data analysis | Eligible | Eligible |
| Lab Technician | Sample preparation, instrument operation, data collection | Eligible | Eligible |
| Engineering Lead | Process design, prototype development, technical troubleshooting | Eligible | Eligible |
| Data Analyst | Experimental data processing, statistical analysis | Eligible | Eligible |
| Software Developer (R&D) | Algorithm development, data pipeline automation for R&D | Eligible | Eligible |
| Project Manager | Planning, coordination, admin (non-technical) | Not Eligible | Review Required |
| Finance / Admin | Accounting, HR, compliance reporting | Not Eligible | Not Eligible |
| Business Development | Commercialization, sales, marketing | Not Eligible | Not Eligible |

## 5. Eligibility Classification Rules

### SR&ED Eligibility Flags
- **Eligible:** Person spends >90% time on qualifying R&D activities.
- **Partially Eligible:** Person spends 50-90% time on R&D; pro-rata claim calculation required.
- **Not Eligible:** Person spends <50% time on R&D or performs purely administrative/commercial work.

### IRAP Eligibility Flags
- **Eligible:** Direct technical contributor to IRAP-funded project activities.
- **Review Required:** Mixed technical/admin role; case-by-case with NRC ITA.
- **Not Eligible:** No direct technical contribution or purely support/admin role.

### Eligibility Review Triggers
- Role change (e.g., founder transitions from R&D to CEO-only role)
- New hire onboarding
- Contractor engagement
- Annual eligibility refresh for multi-year projects

## 6. Labour Entry Activity Categories

### R&D Activity Categories (Claimable)
- **Experimentation:** Hands-on experiment execution, sample prep, instrument operation
- **Analysis:** Data interpretation, statistical analysis, hypothesis refinement
- **Design:** Process/system design, method development, prototype engineering
- **Documentation (Technical):** Experiment logging, technical report writing, data dictionary updates
- **Troubleshooting:** Technical issue diagnosis and resolution related to R&D

### Non-Claimable Activity Categories
- **Administration:** Scheduling, email, general coordination
- **Finance/Accounting:** Invoicing, bookkeeping, payroll
- **Commercialization:** Sales, marketing, customer engagement
- **General Maintenance:** Facility cleaning, equipment moves unrelated to experimental setup

### Mixed Activity Handling
- If single labour entry includes both claimable and non-claimable work, split into separate entries.
- Each entry must have clear `activity_category` and `sred_category` mapping.

## 7. Bootstrap Workflow (Single Founder)

### Initial Setup
1. Create founder person record in `people` table:
   - `person_type = 'Founder'`
   - `role_title = 'Founder / Principal Investigator'`
   - `sred_eligibility = 'Eligible'`
   - `irap_eligibility = 'Eligible'`
   - `hourly_rate` = [founder's notional rate or salary equivalent]
2. Create founder user account in `users` table:
   - `role = 'Founder'` (system role with full permissions)
   - Link to `people` record via `people.user_id`

### Labour Entry Recording
- Founder logs own time using standard labour entry form.
- Each entry specifies: project, experiment (if applicable), activity category, hours, description.
- System auto-fills `person_id`, `rate_used`, `amount` from founder profile.

### Transition Planning (Future Multi-User)
- When first employee/contractor joins, create new person + user records.
- Assign appropriate roles and eligibility flags based on job description.
- Update founder's role/eligibility if duties shift (e.g., less hands-on R&D).

## 8. Role Transition Rules

### Scenario: Founder Reduces R&D Time
- **Trigger:** Founder shifts to >50% admin/business development work.
- **Action:** Update `sred_eligibility` to `'Partially Eligible'` or `'Not Eligible'`.
- **Retroactive Impact:** Review prior claims; eligibility changes apply prospectively unless misclassified.
- **Documentation:** Record role change in person notes with effective date.

### Scenario: Contractor Becomes Employee
- **Trigger:** Contractor hired as full-time employee.
- **Action:** 
  - Update `person_type` from `'Contractor'` to `'Employee'`.
  - Update eligibility flags based on new role scope.
  - Ensure payroll transition (T4A → T4).
- **Historical Records:** Preserve contractor-period labour entries for audit continuity.

## 9. Evidence and Audit Requirements

### Person Profile Evidence
- Resume or role description on file.
- Employment contract or offer letter (for employees).
- Contractor agreement (for contractors).
- Hourly rate or salary documentation.

### Labour Entry Evidence
- Timesheet or effort log (weekly or per-experiment basis).
- Activity description sufficient to demonstrate R&D nature of work.
- Link to experiment or project context.
- Approver sign-off (if multi-user; founder self-approves initially).

### Annual Eligibility Refresh
- Review each person's actual time allocation vs. claimed eligibility.
- Update flags if role has changed.
- Document review outcome and date in person notes or separate log.

## 10. Data Dictionary Mapping

### `people` Table Fields (Governance-Related)
- `person_type`: Founder, Employee, Contractor, Advisor
- `role_title`: Functional role (affects eligibility interpretation)
- `sred_eligibility`: Eligible, Partially Eligible, Not Eligible
- `irap_eligibility`: Eligible, Review Required, Not Eligible
- `active_from`, `active_to`: Engagement period
- `notes`: Classification rationale, role changes, review outcomes

### `labour_entries` Table Fields (Governance-Related)
- `person_id`: Links to person with eligibility metadata
- `activity_category`: Must map to claimable vs. non-claimable list
- `sred_category`: SR&ED classification (if claimable)
- `approved_by_user_id`: Approver (validates claim appropriateness)

## 11. Compliance Checklist

- [ ] Founder person record created with correct eligibility flags.
- [ ] Founder user account linked to person record.
- [ ] Hourly rate or salary-equivalent documented.
- [ ] Labour entry activity categories defined and mapped to claimability.
- [ ] Eligibility review process scheduled (annual minimum).
- [ ] Role transition rules documented and communicated.
- [ ] Evidence retention policy for person profiles and labour logs.
- [ ] Export filter logic ensures only eligible labour is included in SR&ED/IRAP claims.

## 12. Open Decisions
- Confirm founder's notional hourly rate for labour cost calculation.
- Confirm if BDO or tax advisor requires specific timesheet format.
- Confirm IRAP ITA expectations for labour documentation detail.
- Confirm if partial eligibility requires formal CRA pre-approval or is calculated at claim time.

## 13. Next Steps
1. Populate founder record in `people` table as part of Sprint 1 setup.
2. Create labour entry form with activity category dropdown (claimable activities only by default).
3. Map personnel governance rules to acceptance test cases in `TEST_STRATEGY_2026.md`.
4. Schedule first eligibility review at end of Q1 2026 or first funding claim preparation.
