# Documentation standards

Claude MUST follow these when writing or changing docs.

- Every repo has `README.md` (what, how to run), `CLAUDE.md` (rules for the assistant) and `docs/` (decisions and standards)
- Decisions go into ADRs in `pulse/docs/adr/` using `0000-template.md`; accepted ADRs are not rewritten, they are superseded
- Standards in `docs/` are normative: use MUST, SHOULD, MAY deliberately
- Shared files (`ai-workflow.md`, `git-and-commits.md`, `security.md`) have a canonical copy in `pulse`; change it there first and sync the copies
- Docs change in the same commit as the behaviour they describe
- Prefer tables and short lists; diagrams as Mermaid or ASCII, no binary images for architecture
- No secrets, internal URLs with credentials or personal data in docs
