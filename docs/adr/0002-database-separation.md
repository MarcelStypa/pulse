# 0002. Database separation

- Status: Accepted
- Date: 2026-11-25

## Context
Keycloak and the Pulse backend both need PostgreSQL. A service must own its data;
Keycloak's schema changes between versions and must never be read by other services.

## Decision
Database-per-service with logical separation on one Azure Database for PostgreSQL
Flexible Server:

- DB `keycloak` with role `keycloak` (only Keycloak connects)
- DB `pulse` with roles `pulse_app` (backend) and `grafana_ro` (read-only for Grafana)
- Separate credentials per role, stored in Key Vault
- Terraform variable `separate_db_servers` (default `false`) allows a second server later

## Consequences
- Low cost (one burstable server) with credential-level isolation
- Shared server means shared maintenance window and resource limits
- Backend talks to Keycloak only via tokens and the Admin API

## Alternatives considered
- Two servers from the start: stronger isolation, roughly double the cost
- Shared database with separate schemas: weaker isolation, rejected
