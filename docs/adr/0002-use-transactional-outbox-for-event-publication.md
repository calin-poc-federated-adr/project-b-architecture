# 0002 - Use transactional outbox for event publication

- Status: Accepted
- Date: 2026-04-21
- Deciders: Project B architecture team

## Context

Project B publishes domain events to other systems.

A simple approach would be:
1. write business data to the database
2. publish an event to the message broker

That approach creates a consistency risk:
- the database write may succeed while event publication fails
- or event publication may succeed while the business transaction later fails

Project B needs a more reliable way to publish events without introducing distributed transactions.

## Decision

Project B will use the transactional outbox pattern for event publication.

## Rationale

With a transactional outbox:
- business data and outbox records are written in the same database transaction
- a separate publisher process reads pending outbox records
- events are then delivered to the broker asynchronously

This reduces the risk of inconsistency between persisted state and published events.

## Consequences

### Positive

- Improved reliability of event publication
- No need for distributed transactions
- Better separation between transactional write path and asynchronous delivery

### Negative

- Additional component or process to publish outbox entries
- Extra table management and cleanup
- Consumers may still need idempotency because duplicate delivery remains possible

## Follow-up

The implementation should define:
- outbox table structure
- retry behavior
- cleanup policy
- idempotency expectations for consumers