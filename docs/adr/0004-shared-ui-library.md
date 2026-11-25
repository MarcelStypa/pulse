# 0004. Shared Angular UI library

- Status: Accepted
- Date: 2026-11-25

## Context
Future projects and customer-facing apps should reuse the same components. The
project is split into multiple repositories, so an Nx monorepo is not used.

## Decision
- Separate repo `pulse-ui`: Angular library built with `ng-packagr`
- Published as `@marcelstypa/pulse-ui` to GitHub Packages, versioned by `release-please`
- Storybook deployed as a static site as the component showcase
- Design tokens as CSS variables, enabling per-tenant theming
- `pulse-frontend` consumes it as a normal npm dependency

## Consequences
- Reuse is visible in `package.json`
- Releasing a library change and consuming it takes two steps
- Library must stay free of tenant data and API calls

## Alternatives considered
- Nx monorepo: simpler local dev, but contradicts the multi-repo goal
- Copying components between apps: no reuse
