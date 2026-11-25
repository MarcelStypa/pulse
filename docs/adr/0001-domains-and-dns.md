# 0001. Domains and DNS delegation

- Status: Accepted
- Date: 2026-11-25

## Context
The domain `marcelstypa.com` is registered at an external registrar that Terraform
cannot manage. Container Apps managed certificates need DNS validation records.
Keycloak should be a shared identity provider for future projects.

## Decision
| Host | Service |
|---|---|
| `pulse.marcelstypa.com` | Angular frontend |
| `api.pulse.marcelstypa.com` | Spring Boot backend |
| `grafana.pulse.marcelstypa.com` | Grafana |
| `auth.marcelstypa.com` | Keycloak |

Delegate two zones to Azure DNS: `pulse.marcelstypa.com` and `auth.marcelstypa.com`.
Four `NS` records per zone are created once at the registrar. Terraform manages
everything below the delegated zones.

## Consequences
- Fully IaC DNS and certificate validation, no manual record changes after delegation
- Two Azure DNS zones (about EUR 1 per month)
- One-time manual step at the registrar, documented in `pulse-infra`
- Keycloak redirect URIs, CORS origins and Grafana `root_url` must match these hosts exactly

## Alternatives considered
- Manual CNAME/TXT records per host: free but not reproducible
- `auth.pulse.marcelstypa.com`: simpler (one zone) but ties the IdP to one project
