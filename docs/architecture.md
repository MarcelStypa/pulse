# System architecture

Claude MUST read this before changing anything that crosses repo boundaries.

## Context
Pulse is a multi-tenant monitoring platform: URL uptime, CI/CD pipelines, code quality (Sonar) and dependency vulnerabilities. Two views on the same data: the Angular app (product view) and Grafana (ops view).

## Components and ownership
| Component | Repo | Owns |
|---|---|---|
| Web app | `pulse-frontend` | UI composition, OIDC login, routing |
| UI library | `pulse-ui` | Presentational components, design tokens |
| API and collectors | `pulse-backend` | Business logic, `pulse` database, tenant scoping |
| Identity | `pulse-auth` | Keycloak realm, clients, roles, organizations, `keycloak` database |
| Dashboards | `pulse-observability` | Grafana and Prometheus provisioning |
| Cloud | `pulse-infra` | All Azure resources, DNS, Key Vault, databases |
| CI | `pulse-workflows` | Reusable workflows, shared Claude skills |

## Hosts
`pulse.marcelstypa.com` (frontend), `api.pulse.marcelstypa.com` (backend), `grafana.pulse.marcelstypa.com` (Grafana), `auth.marcelstypa.com` (Keycloak). See ADR 0001.

## Cross-repo contracts
- Frontend and Grafana authenticate through Keycloak; backend validates JWTs
- Backend publishes an OpenAPI spec; the TypeScript client is generated from it, never hand-written
- Tenant = Keycloak organization claim; only the backend maps it to `tenant_id` (ADR 0003)
- Grafana reads the `pulse` database with the read-only role only (ADR 0002)
- Secrets come from Key Vault; infra grants access through managed identities

## Rules for changes
- A change that alters a contract above needs an ADR and updates in every affected repo
- Do not let one service read another service's database or internal storage
