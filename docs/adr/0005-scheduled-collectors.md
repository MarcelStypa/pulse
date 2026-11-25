# 0005. Scheduling of collectors

- Status: Accepted
- Date: 2026-11-25

## Context
The backend runs scheduled collectors (URL probes, GitHub, Sonar). Container Apps may
run more than one replica; without coordination each replica would run every job,
causing duplicate data, extra load and rate-limit problems.

## Decision
- Use **ShedLock** with the existing `pulse` PostgreSQL database for `@Scheduled` jobs, so each job runs on one instance per interval
- Jobs are idempotent: results carry a deterministic key (tenant, target, scheduled time)
- Per-tenant work is split into small units so a slow tenant cannot block others
- Prefer GitHub webhooks (HMAC verified) over polling where possible (roadmap)
- Revisit **Container Apps Jobs** or a separate collector service if collector load needs independent scaling

## Consequences
- No new infrastructure; one extra table managed by Flyway
- Collectors and API still scale together for now
- Lock timeouts must be tuned to job duration; documented in the backend docs

## Alternatives considered
- Single replica for the backend: simple but a single point of failure and weak scale story
- Container Apps Jobs from the start: better separation, more moving parts for the MVP
- Message queue: overkill for the expected load
