# FUNDING DELIVERABLE MATRIX 2026

---
**Document Version:** v1.0  
**Status:** Draft  
**Owner:** Founder  
**Approver:** TBD  
**Approval Date:** TBD  
**Last Updated:** 2026-03-09  

---

## 1. Purpose
Define program-specific deliverable requirements for Canadian R&D funding applications to ensure the system produces compliant exports for each funding body.

## 2. Program Overview

### SR&ED (Scientific Research & Experimental Development - CRA)
- **Administering Body:** Canada Revenue Agency (CRA)
- **Claim Type:** Tax credit
- **Claim Cycle:** Annual (fiscal year)
- **Submission Format:** T661 form + narrative + financials

### IRAP (Industrial Research Assistance Program - NRC)
- **Administering Body:** National Research Council Canada (NRC)
- **Claim Type:** Grant/contribution
- **Claim Cycle:** Project milestones + quarterly reporting
- **Submission Format:** Progress reports + budget burn + technical deliverables

### FedDev Ontario
- **Administering Body:** Federal Economic Development Agency for Southern Ontario
- **Claim Type:** Grant/contribution
- **Claim Cycle:** Milestone-based
- **Submission Format:** Milestone reports + commercialization plan + budget tracking

### NGen (Next Generation Manufacturing Canada)
- **Administering Body:** NGen Canada (Innovation Supercluster)
- **Claim Type:** Co-investment
- **Claim Cycle:** Project milestones
- **Submission Format:** Technical progress + commercialization roadmap + matching fund evidence

### OCE/OCI (Ontario Centres of Excellence / Ontario Capital Investment)
- **Administering Body:** Province of Ontario
- **Claim Type:** Grant/voucher
- **Claim Cycle:** Project-based
- **Submission Format:** Technical reports + collaboration evidence + budget summary

### BDC (Business Development Bank of Canada - Tech Loans)
- **Administering Body:** BDC
- **Claim Type:** Loan/financing
- **Claim Cycle:** Quarterly financial reporting
- **Submission Format:** Financial statements + technical progress + revenue forecast

## 3. Deliverable Requirements Matrix

| Deliverable Element | SR&ED | IRAP | FedDev | NGen | OCE/OCI | BDC |
|---|---|---|---|---|---|---|
| **Technological Uncertainty Statement** | ✓ Mandatory | ✓ Mandatory | ✓ Required | ✓ Required | ✓ Required | Optional |
| **Hypothesis-Driven Experiments** | ✓ Mandatory | ✓ Mandatory | ✓ Required | ✓ Required | ✓ Required | Optional |
| **Failure Documentation** | ✓ Mandatory (2026) | ✓ Required | Optional | Optional | Optional | N/A |
| **Labour Breakdown (by person/role)** | ✓ Mandatory | ✓ Mandatory | ✓ Required | ✓ Required | ✓ Required | Summary only |
| **Material Costs Detail** | ✓ Mandatory | ✓ Required | ✓ Required | ✓ Required | ✓ Required | Summary only |
| **Subcontractor Costs** | ✓ Mandatory | ✓ Required | ✓ Required | ✓ Required | ✓ Required | Summary only |
| **Equipment/Capital Expenditure** | Separate claim | ✓ Required | ✓ Required | ✓ Required | ✓ Required | ✓ Required |
| **Timeline / Milestones** | N/A | ✓ Mandatory | ✓ Mandatory | ✓ Mandatory | ✓ Required | ✓ Required |
| **Commercialization Plan** | N/A | ✓ Required | ✓ Mandatory | ✓ Mandatory | ✓ Required | ✓ Mandatory |
| **Technical Merit Narrative** | ✓ Mandatory | ✓ Mandatory | ✓ Required | ✓ Required | ✓ Required | Optional |
| **Budget Burn-Down** | N/A | ✓ Mandatory | ✓ Mandatory | ✓ Mandatory | ✓ Required | ✓ Mandatory |
| **Risk Register** | N/A | ✓ Required | ✓ Required | ✓ Required | Optional | ✓ Required |
| **Evidence Vault / Attachments** | ✓ Mandatory | ✓ Required | Optional | Optional | Optional | N/A |
| **Version History / Audit Trail** | ✓ Mandatory | ✓ Required | Optional | Optional | Optional | N/A |
| **Data Collection Records** | ✓ Required | ✓ Required | Optional | Optional | Optional | N/A |
| **Facility/Setup Context** | ✓ Recommended | ✓ Required | Optional | Optional | Optional | N/A |
| **AI Model Traceability** | ✓ Required (if used) | ✓ Required (if used) | Optional | Optional | Optional | N/A |

## 4. Export Format Requirements

### SR&ED Export Package
**Required Outputs:**
- PDF narrative (CRA T661 format-aligned)
- Experiment log (CSV/JSON/PDF)
- Labour attribution detail (CSV with person/role/hours/activity)
- Cost summary by category (Labour, Materials, Subcontractor, Overhead if proxy method)
- Evidence file index (file list + hash + timestamp)
- Failure log with advancement claims
- Version history for key experiments

**System Requirements Mapping:**
- `FR-010`: Generate SR&ED narrative
- `CR-001` to `CR-009`: Full compliance traceability
- Export endpoints: `/report/sred`, `/export/experiments`, `/export/costs`, `/export/evidence-index`

### IRAP Export Package
**Required Outputs:**
- Milestone progress report (PDF/Word template)
- Technical achievement summary
- Budget vs. actual spend (Excel/CSV)
- Labour breakdown by milestone
- Risk and mitigation update
- Next-phase forecast

**System Requirements Mapping:**
- `RF-001`: Milestone reporting
- `FR-008`: Risk register
- Export endpoints: `/report/irap-milestone`, `/export/budget-burn`, `/export/risks`

### FedDev Export Package
**Required Outputs:**
- Milestone technical report
- Commercialization roadmap update
- Job creation / retention evidence
- Regional economic impact statement
- Budget tracking and variance report

**System Requirements Mapping:**
- `RF-001`, `RF-002`: Milestone and commercialization
- Export endpoints: `/report/feddev-milestone`, `/report/commercialization`

### NGen Export Package
**Required Outputs:**
- Technical progress summary
- Commercialization roadmap
- Matching fund evidence (co-investment proof)
- IP and collaboration plan
- Supply chain / manufacturing readiness

**System Requirements Mapping:**
- `RF-002`, `RF-003`: Commercialization and technical merit
- Export endpoints: `/report/ngen-progress`, `/report/commercialization`

### OCE/OCI Export Package
**Required Outputs:**
- Project summary report
- Technical deliverable evidence
- Collaboration partner records (if applicable)
- Budget summary

**System Requirements Mapping:**
- `RF-003`: Technical merit
- Export endpoints: `/report/oce-summary`

### BDC Export Package
**Required Outputs:**
- Quarterly financial report
- Technical progress update
- Revenue forecast and burn rate
- Risk and mitigation summary

**System Requirements Mapping:**
- `RF-004`: Budget burn-down
- Export endpoints: `/report/bdc-quarterly`

## 5. Data Lineage and Compliance Mapping

### Labour Claims (All Programs)
**Source Tables:**
- `people` (eligibility flags: `sred_eligibility`, `irap_eligibility`)
- `labour_entries` (activity linkage to projects/experiments)
- `cost_entries` (derived labour costs)

**Export Rules:**
- SR&ED: Only `sred_eligibility = 'Eligible'` or `'Partially Eligible'`
- IRAP: Only `irap_eligibility = 'Eligible'`
- All: Must link to project and timeframe

### Experiment Evidence (SR&ED, IRAP)
**Source Tables:**
- `experiments`
- `experiment_versions`
- `data_collection_records`
- `evidence_files`
- `ai_analysis_runs` (if AI-assisted)

**Export Rules:**
- Include uncertainty statement, hypothesis, results, analysis
- Preserve full version history for audit
- Link evidence files by hash and timestamp

### Milestone Progress (IRAP, FedDev, NGen, OCE, BDC)
**Source Tables:**
- `sprints` (linked to deliverables)
- `backlog_items` (requirement traceability)
- `reports` (generated packages)

**Export Rules:**
- Report against approved milestones
- Include completion %
- Flag blockers and risks

## 6. Approval and Sign-Off Requirements

| Program | Internal Approver Role | External Review | Submission Timing |
|---|---|---|---|
| SR&ED | Founder + Accountant | BDO / Tax Consultant | Annual (within 18 months of fiscal year-end) |
| IRAP | Founder + Project Manager | NRC Industrial Technology Advisor (ITA) | Quarterly or milestone-based |
| FedDev | Founder + Finance | FedDev Program Officer | Milestone submission dates per agreement |
| NGen | Founder + Technical Lead | NGen Project Manager | Milestone submission dates per agreement |
| OCE/OCI | Founder | OCE Program Coordinator | Project completion or interim as required |
| BDC | Founder + Finance | BDC Relationship Manager | Quarterly |

## 7. System Implementation Checklist

- [ ] Report generation endpoints for each program type
- [ ] Export templates aligned to program formats (PDF/CSV/Excel)
- [ ] Eligibility filtering logic for labour claims
- [ ] Evidence vault export with integrity verification
- [ ] Automated compliance checks before export (missing fields, invalid data)
- [ ] Role-based report approval workflow
- [ ] Export audit log (who exported what, when)
- [ ] Program-specific validation rules mapped to acceptance tests

## 8. Open Questions and Decisions Needed
- Confirm BDO engagement scope for SR&ED narrative review.
- Confirm IRAP ITA contact and milestone schedule.
- Confirm FedDev agreement milestones and submission format preferences.
- Confirm NGen co-investment tracking methodology and matching fund evidence format.
- Confirm if OCE/OCI requires interim reporting or only final submission.
- Confirm BDC loan reporting cycle and technical progress detail level.

## 9. Next Steps
1. Map each export endpoint to controller/service implementation plan.
2. Create report templates (PDF/Excel shells) for each program.
3. Define export validation test cases in `TEST_STRATEGY_2026.md`.
4. Schedule dry-run exports before first real submission deadline.
