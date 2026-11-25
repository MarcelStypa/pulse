# Pulse

Multi-tenant monitoring platform: URL uptime, CI/CD pipelines, code quality and
dependency vulnerabilities in one place. Built as a portfolio project to show an
end-to-end, production-style stack: Angular, Spring Boot, Keycloak, Grafana,
Terraform on Azure.

> Status: early design phase. See [ROADMAP.md](ROADMAP.md).

## Architecture

```
Angular pulse-frontend ──uses──► pulse-ui (npm library + Storybook)
        │ OIDC (Authorization Code + PKCE)
        ▼
Keycloak (auth.marcelstypa.com) ◄─ JWT validation ─ Spring Boot backend ─ Postgres (pulse DB)
                                                        │ collectors: GitHub, Sonar, URL probes
                                                        └─► Prometheus ─► Grafana (OIDC)
Azure Container Apps · Key Vault · Azure DNS · Terraform · GitHub Actions (OIDC)
```

## Hosts

| Host | Service |
|---|---|
| `pulse.marcelstypa.com` | Angular frontend |
| `api.pulse.marcelstypa.com` | Spring Boot backend |
| `grafana.pulse.marcelstypa.com` | Grafana |
| `auth.marcelstypa.com` | Keycloak (shared identity provider) |

## Repositories

| Repo | Purpose |
|---|---|
| `pulse` | This repo: overview, ADRs, local dev stack |
| `pulse-infra` | Terraform for all Azure resources |
| `pulse-auth` | Keycloak image, realm-as-code, organizations |
| `pulse-backend` | Collector and tenant-aware REST API |
| `pulse-ui` | Shared Angular component library + Storybook |
| `pulse-frontend` | The Pulse web app |
| `pulse-observability` | Grafana and Prometheus provisioning |
| `pulse-workflows` | Reusable CI workflows and shared Claude skills |

## Local development

Planned: `docker compose up` starts Keycloak (realm imported), Postgres, backend,
frontend and Grafana. See [docs/local-dev.md](docs/local-dev.md) once available.

## Documentation

- [Architecture decisions](docs/adr/)
- [Roadmap](ROADMAP.md)
- [Contributing and commit rules](CONTRIBUTING.md)

## AI-assisted development
This project is built with an AI coding assistant (Claude Code), openly and by design. The rules that govern its use (human review, verification before trust, attribution, no secrets) are documented in [docs/ai-workflow.md](docs/ai-workflow.md), and the assistant is bound by [CLAUDE.md](CLAUDE.md) and [docs/](docs/).

### In the maintainer's words
<!-- TODO(human): write 3-5 sentences in your own voice about how you use AI on this project -->

## License
[MIT](LICENSE)
