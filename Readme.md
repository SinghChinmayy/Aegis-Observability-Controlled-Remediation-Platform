# Aegis

> Observability and controlled remediation for self-hosted services.

Aegis is a platform for detecting service failures, creating an auditable incident, proposing a safe remediation, requiring human approval, and verifying recovery.

# Tech stack

Aegis will use a **Go backend with a REST API**.

## Backend

- **Language:** Go
- **API style:** REST over HTTP with JSON request/response bodies
- **Router:** Chi or Gin, with Chi preferred for a small, explicit API surface
- **Database:** PostgreSQL
- **Database access:** `pgx` with SQL migrations
- **Async work:** background worker process for remediation runs and recovery checks
- **Observability:** structured logs, Prometheus metrics, OpenTelemetry traces
- **API contract:** OpenAPI generated from the implemented HTTP routes

## Frontend

- Start with a simple web UI after the first backend vertical slice works.
- Keep the UI API-facing only; remediation safety rules live in the backend.

## First backend slice

1. `GET /health`
2. `POST /v1/services`
3. `GET /v1/services`
4. `POST /v1/signals`
5. `POST /v1/incidents`
6. `GET /v1/incidents/{incident_id}`

The first slice is complete only when it has storage, tests, logs, metrics, and curl examples.

# Record Your Progress
Rule for every feature

A feature is not done until it has:

1. Working code
2. A test or reproducible failure case
3. Metrics/logs proving it ran
4. A screenshot or short demo clip
5. A short explanation of what you learned
