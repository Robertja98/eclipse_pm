# FACILITY SETUP STANDARDS 2026

---
**Document Version:** v1.0  
**Status:** Draft  
**Owner:** Founder  
**Approver:** TBD  
**Approval Date:** TBD  
**Last Updated:** 2026-03-09  

---

## 1. Purpose
Define space zoning, readiness checks, and location tagging standards for proper shop/lab setup to support operational control, safety, and IRAP/CRA governance evidence.

## 2. Current Facility Baseline (Single Founder Setup)

### Physical Context
- **Location:** Shop/lab workspace (specify address or reference in separate secure doc)
- **Size:** [To be documented]
- **Primary Use:** R&D experimentation for mixed bed resin treatment process optimization
- **Occupancy:** Single operator initially; designed for future expansion

### Initial Zoning (Recommended)
- **Bench Area:** Primary hands-on work surface for sample prep and small-scale experiments
- **Test Area:** Dedicated zone for active experimental runs and instrument operation
- **Storage Area:** Shelving and cabinets for chemicals, materials, equipment
- **Safety Zone:** Designated area for PPE, eyewash, fire extinguisher, first aid
- **Office/Documentation Area:** Desk for computer work, documentation, logging

## 3. Space Type Definitions

### Bench
**Purpose:** Manual work surface for preparation, assembly, and small-scale tasks.  
**Typical Equipment:** Lab bench, fume hood (if applicable), hand tools, glassware.  
**Safety Requirements:** Clean surface, proper lighting, PPE accessible.  
**Readiness Checks:** Surface clean, no clutter, utilities functional (water, power if applicable).

### Test Area
**Purpose:** Active experimental runs involving instruments, reactors, or process equipment.  
**Typical Equipment:** Reactors, pumps, sensors, data acquisition systems.  
**Safety Requirements:** Proper ventilation, spill containment, emergency shut-off accessible.  
**Readiness Checks:** Equipment calibrated, safety systems verified, documentation in place.

### Storage
**Purpose:** Organized storage of chemicals, materials, consumables, spare equipment.  
**Typical Equipment:** Shelving, cabinets, chemical storage (flammable cabinet if needed).  
**Safety Requirements:** Proper chemical segregation, labeling, MSDS/SDS accessible.  
**Readiness Checks:** Inventory current, expired materials removed, labels legible.

### Safety Zone
**Purpose:** Safety equipment and emergency response resources.  
**Typical Equipment:** Eyewash station, safety shower (if applicable), fire extinguisher, first aid kit, spill kits, PPE (gloves, goggles, lab coat).  
**Safety Requirements:** Unobstructed access, regular inspection, equipment in-date.  
**Readiness Checks:** Equipment functional, inspection stickers current, access clear.

### Office
**Purpose:** Computer work, documentation, planning, reporting.  
**Typical Equipment:** Desk, computer, printer, filing cabinets (physical or digital).  
**Safety Requirements:** Ergonomic setup, proper electrical wiring, secure data storage.  
**Readiness Checks:** Workspace organized, backups current, no trip hazards.

## 4. Facility Space Registration

### Creating Space Records
Each physical zone should be registered in `facility_spaces` table:
- `space_code`: Short internal code (e.g., "BENCH-01", "TEST-A", "STORAGE-MAIN")
- `space_name`: Human-readable name (e.g., "Primary Lab Bench", "Resin Test Stand")
- `space_type`: Bench, TestArea, Storage, SafetyZone, Office
- `status`: Active, Restricted (if access controlled), OutOfService (if under maintenance)
- `notes`: Setup details, constraints, special requirements

### Example Initial Setup (Single Founder Shop)
```
space_code: BENCH-01
space_name: Primary Lab Bench
space_type: Bench
status: Active
notes: Main work surface, fume hood available, 6ft x 3ft bench

space_code: TEST-A
space_name: Resin Test Stand
space_type: TestArea
status: Active
notes: Reactor and pump setup, flow meters installed

space_code: STORAGE-MAIN
space_name: Main Chemical Storage
space_type: Storage
status: Active
notes: Flammable cabinet, acid/base segregation enforced

space_code: SAFETY-01
space_name: Safety Station
space_type: SafetyZone
status: Active
notes: Eyewash, fire extinguisher, PPE rack, first aid kit

space_code: OFFICE-01
space_name: Documentation Desk
space_type: Office
status: Active
notes: Computer workstation, filing cabinet, printer
```

## 5. Readiness Check Standards

### Check Frequency
- **Pre-Use:** Before each experiment or work session in that space.
- **Weekly:** General space readiness and safety equipment verification.
- **Monthly:** Detailed inspection with documentation (recommended for IRAP/CRA evidence).
- **Annual:** Comprehensive review, equipment calibration verification, safety audit.

### Checklist Template (Bench / Test Area)
| Check Item | Pass/Fail/Conditional | Notes |
|---|---|---|
| Work surface clean and clear | | |
| Equipment functional and calibrated | | |
| Utilities available (power, water, air) | | |
| PPE accessible | | |
| Spill kit available | | |
| Emergency contacts posted | | |
| Lighting adequate | | |
| Ventilation operational | | |
| No safety hazards (trip, electrical, chemical) | | |

### Checklist Template (Storage)
| Check Item | Pass/Fail/Conditional | Notes |
|---|---|---|
| Chemicals properly labeled | | |
| Inventory matches records | | |
| Incompatible materials segregated | | |
| Expired materials removed | | |
| MSDS/SDS accessible | | |
| Spill containment in place | | |
| Shelving stable and secure | | |

### Checklist Template (Safety Zone)
| Check Item | Pass/Fail/Conditional | Notes |
|---|---|---|
| Eyewash operational (if installed) | | |
| Fire extinguisher charged and in-date | | |
| First aid kit stocked and current | | |
| PPE available and in good condition | | |
| Emergency exit clear | | |
| Safety signage posted | | |

## 6. Readiness Check Recording

### Creating Check Records
Readiness checks recorded in `facility_readiness_checks` table:
- `facility_space_id`: Links to space being checked
- `checked_by_person_id`: Person performing check (founder initially)
- `check_date`: Date of check
- `setup_status`: Ready, Conditional (minor issues), NotReady (blocking issues)
- `safety_status`: Pass, Warning, Fail
- `findings`: Summary of check outcomes
- `corrective_action`: Required actions if Conditional or NotReady
- `evidence_file_id`: Optional photo or checklist scan upload
- `valid_until`: Expiry of readiness state (e.g., monthly check valid for 30 days)

### Status Interpretation
- **Ready + Pass:** Space fully operational, no issues.
- **Conditional + Warning:** Space usable but minor issues noted (e.g., one PPE item low stock); corrective action recommended but not blocking.
- **NotReady + Fail:** Space not safe or functional; corrective action required before use.

### Evidence for IRAP/CRA
- Readiness checks demonstrate operational control and safety governance.
- Monthly or pre-experiment checks provide audit-defensible record of proper facility management.
- Photos or scanned checklists strengthen evidence (optional but recommended).

## 7. Location Tagging for Experiments and Data

### Linking to Facility Spaces
- Experiments may record which space(s) were used (optional field in `experiments` or via separate link table).
- Data collection records can link to `facility_space_id` to show where measurement occurred.
- Equipment can be assigned a "home" location in `equipment_assets.location` or linked to space.

### Traceability Benefits
- Reproducibility: Knowing where experiment was run helps replicate conditions.
- Audit: CRA/IRAP reviewers can verify work was performed in proper controlled environment.
- Safety: If incident occurs, space context helps investigation and corrective action.

## 8. Space Restrictions and Access Control

### Restricted Status
- If space requires special training, clearance, or safety approval, set `status = 'Restricted'`.
- System can enforce workflow: user must complete readiness check or training record before work in restricted space.

### Out-of-Service Status
- If space under maintenance or renovation, set `status = 'OutOfService'`.
- System should warn or block new work assignments to that space until cleared.

## 9. Single Founder Bootstrap Workflow

### Initial Setup (Day 1)
1. Walk through physical shop/lab.
2. Identify and name each functional zone.
3. Create `facility_spaces` records for each zone.
4. Perform initial readiness check for each space.
5. Document corrective actions if any space is Conditional or NotReady.

### Ongoing Operations
- Before each experiment, quick visual check of work area.
- Weekly: Log formal readiness check for active spaces.
- Monthly: Comprehensive documented check with evidence upload.
- Link experiments and data collection to spaces as applicable.

### Future Expansion
- When additional staff join, assign space access permissions if needed.
- Update space records if layout changes (e.g., new equipment installed, zone repurposed).
- Maintain readiness check discipline to preserve audit trail.

## 10. Integration with Other Systems

### Experiment Workflow
- Experiment planning form can include "Primary Space" dropdown.
- Links experiment to `facility_space_id` (optional but recommended).

### Data Collection Workflow
- Data entry form can include "Location" dropdown.
- Links data record to `facility_space_id`.

### Equipment Management
- Equipment asset `location` field can reference space code or name.
- Equipment calibration records can note where calibration was performed.

## 11. Compliance Checklist

- [ ] Facility spaces defined and registered in system.
- [ ] Initial readiness checks completed for all spaces.
- [ ] Readiness check schedule established (weekly/monthly).
- [ ] Checklist templates created and accessible.
- [ ] Evidence upload process tested.
- [ ] Space status logic enforced (e.g., warn if space NotReady).
- [ ] Location tagging enabled in experiment and data collection forms.
- [ ] Safety zone equipment inspection dates logged.

## 12. Open Decisions
- Confirm physical shop address and size documentation method (secure note vs. system record).
- Confirm readiness check frequency (weekly or bi-weekly for active spaces; monthly for storage/office).
- Confirm if photos required for all checks or only monthly/annual.
- Confirm who performs checks if/when additional staff join (founder + safety officer role?).

## 13. Next Steps
1. Populate `facility_spaces` table with initial shop zones.
2. Perform and record first readiness check for each space.
3. Create readiness check form/template (paper or digital).
4. Map facility setup standards to acceptance test cases in `TEST_STRATEGY_2026.md`.
5. Schedule first monthly comprehensive check within 30 days.
