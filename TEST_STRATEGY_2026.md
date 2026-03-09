# TEST STRATEGY 2026

---
**Document Version:** v1.0  
**Status:** Draft  
**Owner:** Founder  
**Approver:** TBD  
**Approval Date:** TBD  
**Last Updated:** 2026-03-09  

---

## 1. Objective
Define how quality and compliance will be verified before coding starts.

## 2. Test Levels
### Unit Tests
- Validate model, service, and helper logic.
- Focus on data validation, transformations, and business rules.

### Integration Tests
- Validate controller-service-model interactions.
- Validate routing, persistence, and file evidence flow.

### Acceptance Tests
- Validate end-to-end workflows:
  - Experiment creation and updates
  - Failure logging
  - Cost attribution
  - SR&ED/grant report generation

### Compliance Tests
- Validate required SR&ED fields are mandatory.
- Validate timestamp and immutability behavior.
- Validate version history integrity.
- Validate export output completeness (JSON/CSV/PDF narrative).

## 3. Quality Gates Per Sprint
- Sprint planning includes testable acceptance criteria.
- Pull requests require passing unit/integration checks.
- Sprint review requires evidence of compliance test execution for relevant stories.

## 4. Non-Functional Tests
- Security baseline: auth/session controls and role checks.
- Reliability baseline: upload integrity and retry behavior.
- Performance baseline: acceptable response time for core CRUD and report routes.

## 5. Test Data Policy
- Use synthetic data for dev/test.
- Tag test evidence clearly to avoid audit confusion.
- Keep fixtures versioned with schema changes.

## 6. Entry and Exit Criteria
### Entry
- Requirements and acceptance criteria approved.
- Data dictionary aligned with feature scope.
- Test cases drafted for critical paths.

### Exit
- All critical acceptance criteria pass.
- No unresolved high-severity defects.
- Compliance checks pass for affected modules.
- Documentation updated.

## 7. Initial Critical Test Cases
1. Create experiment with all mandatory SR&ED fields.
2. Update experiment and verify version entry is created.
3. Log failed experiment path and verify traceability to advancement notes.
4. Add labour/material cost and verify SR&ED category attribution.
5. Upload evidence file and verify hash + timestamp recorded.
6. Generate SR&ED report and validate required narrative sections.
