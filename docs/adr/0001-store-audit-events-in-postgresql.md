# 0001 - Store audit events in PostgreSQL

- Status: Accepted
- Date: 2026-04-21
- Deciders: Project B architecture team

## Context

Project B must retain audit events for operational traceability and compliance reporting.

The team considered whether audit events should be kept only in application logs, stored only in a messaging system, or persisted in a relational database.

The main requirements are:
- queryability
- predictable retention
- structured reporting
- simple operational ownership

The platform already uses PostgreSQL and the team is familiar with operating it.

## Decision

Project B will persist audit events in PostgreSQL.

## Rationale

PostgreSQL provides:
- structured storage
- easy querying for investigations and reports
- clear retention management
- mature backup and restore procedures

This is sufficient for the expected scale of audit traffic in Project B.

## Consequences

### Positive

- Easy querying by support and engineering teams
- Straightforward reporting
- Strong operational familiarity
- Clear data ownership

### Negative

- The database becomes part of the audit pipeline
- Schema evolution must be managed carefully
- High event volumes may require partitioning or archiving later

## Notes

This decision does not prevent later forwarding of audit events to other analytical or monitoring platforms.