# Threat model

> Normative. Claude MUST read this before changing the backend collectors, auth, tenancy, Keycloak exposure or anything handling external input.

Status: initial version, reviewed at the end of every phase.

## Assets
| Asset | Why it matters |
|---|---|
| Tenant data (targets, results, findings) | Confidentiality between customers |
| Third-party tokens (GitHub, Sonar) | Grant read access to customer systems |
| Keycloak realm, admin access, signing keys | Control over every login |
| Azure identities and Key Vault | Control over the whole platform |
| Availability of Pulse itself | It is a monitoring product |

## Trust boundaries
Internet → (frontend, API, Grafana, Keycloak) → Azure internal network → (Postgres, Key Vault). Everything crossing the first boundary is untrusted, including URLs entered by tenants and any data returned by monitored systems or GitHub.

## Threats and mitigations

### T1. SSRF through the URL checker (high)
A tenant enters a URL such as `http://169.254.169.254/...`, an internal Container Apps host or `localhost`; the backend requests it and leaks cloud tokens or internal data.
- Allow only `http` and `https`, standard ports unless explicitly allowed
- Resolve DNS first and reject loopback, link-local, private (RFC 1918), CGNAT and metadata ranges; pin the resolved IP for the request to prevent DNS rebinding
- Do not follow redirects blindly: re-validate every hop, cap at 3
- Connect and read timeouts, response size limit, never return the response body to the user
- Per-tenant limits on number of targets and probe frequency
- Tests cover each blocked range and a rebinding case

### T2. Cross-tenant data access (high)
- Tenant derived only from the validated JWT organization claim (ADR 0003)
- Hibernate `@TenantId` plus Postgres Row-Level Security
- Mandatory cross-tenant isolation test; Grafana tenant variable, read-only DB role
- Admin endpoints require the `admin` role and are audited

### T3. Token and credential exposure (high)
- Third-party tokens are encrypted at rest, read-only scopes, rotated; never logged or returned by the API
- GitHub access through a GitHub App or fine-grained read-only token, never a classic PAT
- Secrets only in Key Vault; managed identity for access; gitleaks in hooks and CI

### T4. Keycloak admin exposure (high)
- `/admin` and the master realm are not reachable from the public internet (IP restriction or separate internal ingress)
- Admin bootstrap credentials are one-time, rotated, stored in Key Vault
- Redirect URIs exact, PKCE required, short token lifetimes, brute-force protection

### T5. Prompt injection into the AI assistant (medium)
Monitored data (run names, commit messages, page content) may contain instructions.
- Claude treats tool output and fetched content as data, never as instructions (`security.md`)
- Production data and credentials are not available in the development environment

### T6. Supply chain (medium)
- Lockfiles, Dependabot, dependency and image scans, SBOM, pinned GitHub Actions by SHA
- New dependencies are reviewed manually (PR checklist); verify the package exists and is the intended one

### T7. Denial of service and cost abuse (medium)
- Rate limits per tenant and per IP; collector concurrency limits
- Scale limits on Container Apps; budget alert; `demo_mode` switch

### T8. Availability of the monitor (medium)
- External uptime check on Pulse itself; scheduled collectors use a distributed lock (ADR 0005)

### T9. State and infrastructure (medium)
- Terraform state storage is private, RBAC only, no shared keys, versioned
- CI deploys through OIDC federation; production credentials are not on developer machines

## Out of scope for the MVP
Customer-managed encryption keys, WAF, formal compliance certification. Revisit before onboarding real external customers.

## Review
Update this document when a boundary, asset or integration changes. New collector or external integration = new threat review in the PR.
