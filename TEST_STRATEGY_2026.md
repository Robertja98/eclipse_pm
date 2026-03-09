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
- Validate service and helper logic with mocked repositories.
- Focus on data validation, transformations, and business rules.
- Mock ZimXClient and repository methods (no ZIM database required)

### Integration Tests
- Validate controller-service-repository interactions against real ZIM database.
- Validate ZIM persistence (FIND/INSERT/UPDATE/DELETE operations)
- Validate session management and set lifecycle
- Validate routing and file evidence flow
- **Requirements:** ZIM engine running on localhost:6002

### ZIM-Specific Test Patterns
- **Consistency checks:** Verify that repositories create corresponding ZIM table entries
- **Set lifecycle:** Verify sets are created and dropped correctly
- **Session isolation:** Verify two concurrent HTTP sessions have independent ZIM sessions
- **Parameter binding:** Verify parameterized queries prevent SQL injection

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
- **For ZIM-dependent stories:** ZIM engine must be running and schema deployed

### Exit
- All critical acceptance criteria pass.
- No unresolved high-severity defects.
- Compliance checks pass for affected modules.
- Documentation updated.
- **For ZIM-dependent stories:** Both unit (mocked) and integration (real ZIM) tests pass

## 7. Sprint 1-Specific Test Cases (Authentication & Identity)
1. **Register user** – Verify email unique, password hashing, user record in ZIM `users` table
2. **Register validation** – Reject weak password, invalid email, duplicate email
3. **Login success** – Verify session created, user authorized, can access protected routes
4. **Login failure** – Verify failed attempt logged, user locked after N attempts (admin configurable)
5. **Logout** – Verify session cleared, ZIM context closed, cannot access protected routes
6. **Founder person record** – Verify seed script creates user + person, both retrievable
7. **RBAC enforcement** – Verify Team Member cannot access `/admin/*` routes, gets 403
8. **Concurrent sessions** – Verify User A login doesn't interfere with User B session

## 8. Future Sprint Test Cases (Post-Sprint 1)
1. Create experiment with all mandatory SR&ED fields.
2. Update experiment and verify version entry is created in ZIM.
3. Log failed experiment path and verify traceability to advancement notes.
4. Add labour/material cost and verify SR&ED category attribution.
5. Upload evidence file and verify hash + timestamp recorded.
6. Generate SR&ED report and validate required narrative sections.
