# Contributing

This project is public. Keep history clean and never commit secrets.

## Commits
[Conventional Commits](https://www.conventionalcommits.org/): `type(scope): subject`

- Types: `feat`, `fix`, `docs`, `refactor`, `test`, `build`, `ci`, `chore`, `perf`
- Imperative, lower-case subject, max 72 characters, no trailing period
- One logical change per commit
- Body explains why, not what

## Branches and PRs
- `main` is protected: pull requests only, required checks must pass, squash merge
- Branch names: `feat/short-name`, `fix/short-name`, `docs/short-name`

## Secrets
- Never commit `.env`, `*.tfvars`, `*.tfstate`, keys or tokens
- Real values live in Azure Key Vault; repos only contain `*.example` files
- gitleaks runs locally (pre-commit) and in CI

## Quality gates
Tests, lint, dependency scan and the Sonar quality gate must pass before merge.
