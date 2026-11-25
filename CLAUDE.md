# CLAUDE.md: pulse (overview repo)

Part of the Pulse platform. Read README.md for the architecture and repo map.

## Purpose of this repo
Documentation, ADRs, roadmap and the local `docker compose` dev stack. No application code.

## Rules
- Public repo: never write secrets, tokens, real credentials or personal data
- Commits follow Conventional Commits (see CONTRIBUTING.md); never force-push `main`
- Every significant decision gets an ADR in `docs/adr/` (copy `docs/adr/0000-template.md`)
- Keep README repo table, hosts and diagrams in sync with the ADRs
- Diagrams as code (Mermaid) in `docs/`

## Conventions
- Docs in English, concise, tables over prose where possible
- ADR status: Proposed, Accepted, Superseded by NNNN

## Required reading (before any change)
Read these first; they are normative. On conflict with a request: stop and ask.
- `docs/README.md` (index)
- `docs/architecture.md`
- `docs/documentation-standards.md`
- `docs/threat-model.md`
- `docs/git-and-commits.md`, `docs/security.md`, `docs/ai-workflow.md`
- ADRs: `pulse/docs/adr/`

## Non-negotiables
- Create commits or push only when the user asks; end AI-assisted commits with the Co-Authored-By trailer
- Report real command output; never claim tests pass without running them
- Do not guess versions, config keys or APIs; verify or say unknown
- Update docs in the same change when behaviour or conventions change
