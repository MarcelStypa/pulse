# 0003. Multi-tenancy model

- Status: Accepted
- Date: 2026-11-25

## Context
Pulse should be usable as a platform where customers are added later. Retrofitting
tenancy is expensive, so it is a standard from day one.

## Decision
- **Identity:** one Keycloak realm with the Organizations feature; each customer is an organization and tokens carry the organization claim
- **Backend:** every business table has `tenant_id`; a request filter resolves the tenant from the JWT into a `TenantContext`; Hibernate `@TenantId` scopes queries; Postgres Row-Level Security is a second line of defence
- **Grafana:** shared dashboards filtered by a tenant variable first, org-per-tenant later if needed
- **Dogfooding:** Pulse is tenant #1, a demo customer is tenant #2
- **Proof:** an automated cross-tenant isolation test is mandatory

## Consequences
- Small upfront cost in every table, query and test
- Onboarding needs an admin API that calls the Keycloak Admin API
- UI is built for one tenant first; onboarding UI is on the roadmap

## Alternatives considered
- Realm per tenant: strong isolation but poor scaling and slow admin operations
- Schema or database per tenant: heavy for this scale
