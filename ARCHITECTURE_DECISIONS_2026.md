# ARCHITECTURE DECISIONS 2026

---
**Document Version:** v1.0  
**Status:** Draft  
**Owner:** Founder  
**Approver:** TBD  
**Approval Date:** TBD  
**Last Updated:** 2026-03-09  

---

## Purpose
Track major technical decisions before and during implementation with rationale, alternatives, and impact.

## ADR-001: Application Pattern
- Status: Accepted
- Date: 2026-03-09
- Decision: Use modular MVC-style PHP architecture.
- Context: Need maintainable separation for SR&ED, PMBOK, Agile, cost, and reporting modules.
- Alternatives considered:
  - Monolithic procedural PHP
  - Heavy full-stack framework
- Rationale: MVC provides clear boundaries while keeping local-hosted deployment simple.
- Consequences:
  - Positive: Better maintainability and testability.
  - Negative: Requires disciplined folder/module ownership.

## ADR-002: Data Store Strategy
- Status: Accepted
- Date: 2026-03-09
- Accepted: 2026-03-09
- Decision: Keep model/repository abstraction to support MyZIM and MariaDB.
- Context: Project needs flexibility in deployment and evolving storage requirements.
- Alternatives considered:
  - MariaDB only
  - MyZIM only
- Rationale: Abstraction reduces lock-in and allows phased migration.
- Consequences:
  - Positive: Storage flexibility.
  - Negative: Slightly more design work upfront.

## ADR-003: Audit and Immutability Strategy
- Status: Accepted
- Date: 2026-03-09
- Accepted: 2026-03-09
- Decision: Append-only audit events for experiment and evidence changes.
- Context: SR&ED 2026 requires robust timestamped and traceable records.
- Alternatives considered:
  - Overwrite in place with updated timestamps only
  - Full event sourcing for all entities
- Rationale: Append-only audit logs are simpler than full event sourcing and still audit-grade.
- Consequences:
  - Positive: Strong traceability and defensibility.
  - Negative: More storage and reporting joins.

## ADR-004: Repository Abstraction with ZimXClient
- Status: Accepted
- Date: 2026-03-09
- Decision: Use Repository Pattern with ZimXClient adapter for all persistence operations.
- Context: ZIM database requires explicit session management and command execution. Repository abstraction isolates ZIM complexity from business logic while supporting future migration.
- Alternatives considered:
  - Direct ZIM queries in controllers (tight coupling)
  - ORM library (ZIM not supported by standard ORMs)
  - Raw SQL with prepared statements (less maintainable)
- Rationale: ZimXClient adapter encapsulates ZIM-specific behavior (session sets, FIND/INSERT/UPDATE commands, parameter binding), allowing services to remain database-agnostic.
- Consequences:
  - Positive: Clear boundaries between persistence and logic; testable via mock repositories; supports future storage migration.
  - Negative: Requires custom adapter maintenance; FIND/INSERT/UPDATE style differs from typical SQL ORMs.
- Implementation: See `ZIM_IMPLEMENTATION_GUIDE.md` for detailed patterns and examples.

## ADR Template
Use this template for all future decisions:

```md
## ADR-XXX: <Title>
- Status: Proposed | Accepted | Superseded
- Date: YYYY-MM-DD
- Decision:
- Context:
- Alternatives considered:
  - A
  - B
- Rationale:
- Consequences:
  - Positive:
  - Negative:
```
