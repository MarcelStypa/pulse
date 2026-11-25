# Roadmap

MVP principle: design for N tenants, build the UI for one.

## MVP scope
- One tenant visible in the UI (Pulse monitors Pulse), a second demo tenant for isolation proof
- URL uptime checker
- GitHub Actions pipeline view
- Open CVE count and Sonar quality gate badge
- Keycloak login for every API call and the Grafana UI

## Phases

1. **Foundation**: repos, conventions, CLAUDE.md, ADRs, shared workflows
2. **Infra**: Terraform state, resource group, Key Vault, Postgres, Container Apps env, DNS zones and delegation
3. **Auth**: Keycloak container, realm-as-code, secrets in Key Vault
4. **Backend**: secured skeleton, tenant context, first collector, tests, Sonar, Docker, deploy
5. **UI library + frontend**: pulse-ui with Storybook, login, dashboard, deploy
6. **Observability**: Prometheus and Grafana, dashboards as code, OIDC
7. **Polish**: diagrams, screenshots, demo login, write-up

## Later
- Tenant onboarding via admin API and Keycloak Admin API
- GitHub webhooks instead of polling
- Per-tenant theming from design tokens
- Audit log and per-tenant rate limiting
- SBOM and cosign signing displayed in Pulse
- Budget alerts and a `demo_mode` cost switch in Terraform
