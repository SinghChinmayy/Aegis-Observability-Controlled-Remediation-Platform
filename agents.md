# Agent Reference

## Project summary

Aegis is an observability and controlled-remediation platform for self-hosted services. It detects service failures, creates auditable incidents, proposes safe remediation plans, requires human approval, executes constrained actions, and verifies recovery.

The core product promise is not automatic repair at any cost. Aegis should make operational action safe, visible, reviewable, and reversible where possible.

## Current tech direction

- Backend language: Go
- API style: REST over HTTP with JSON
- Router: Chi preferred for the first implementation
- Database: PostgreSQL
- Database access: `pgx` with SQL migrations
- Async work: background worker process for remediation runs and recovery checks
- Observability: structured logs, Prometheus metrics, OpenTelemetry traces
- API contract: OpenAPI generated from or maintained beside implemented routes

## Core domain model

- Service: a target system Aegis can observe
- Signal: raw evidence such as an alert, health-check failure, log anomaly, or metric threshold breach
- Incident: operator-facing event created from one or more signals
- Remediation plan: proposed safe action, not execution itself
- Approval: human decision on whether a plan may proceed
- Remediation run: execution record for an approved plan
- Audit event: immutable record of safety-relevant actions and outcomes

## First backend slice

Build the first vertical slice in this order:

1. `GET /health`
2. `POST /v1/services`
3. `GET /v1/services`
4. `POST /v1/signals`
5. `POST /v1/incidents`
6. `GET /v1/incidents/{incident_id}`

This slice is only done when it has persistence, tests, structured logs, metrics, and curl examples.

## API principles

- Use `/v1` route prefixes from the start.
- Prefer resource-oriented REST paths.
- Use JSON request and response bodies with `snake_case` fields.
- Use string IDs rather than exposing database integer IDs.
- Validate input at the HTTP boundary with explicit Go request structs.
- Reject malformed or unknown fields rather than silently accepting them.
- Use consistent structured error responses with a request ID.
- Return meaningful status codes, especially `201`, `202`, `400`, `401`, `403`, `404`, `409`, and `422`.
- Long-running work should return `202 Accepted` and continue in a worker.

## Safety rules

- Never add an arbitrary command-execution endpoint.
- Remediation actions must be allow-listed.
- Approval and execution must remain separate concepts.
- The identity that detects an incident must not automatically be authorized to execute remediation.
- Every approval, run, failure, retry, and recovery verification should be audit-relevant.
- Retried state-changing operations need idempotency protection.
- Destructive or risky worker actions need bounded retries and clear failure states.

## Documentation habits

Record progress as features are built. A feature is not done until it has:

1. Working code
2. A test or reproducible failure case
3. Metrics/logs proving it ran
4. A screenshot or short demo clip
5. A short explanation of what was learned

Primary docs currently live in:

- `Readme.md`
- `Docs/learnings/api-guide.md`
- `Docs/logs/week1.md`

Keep future docs aligned with the Go + REST stack unless the user explicitly changes direction.
