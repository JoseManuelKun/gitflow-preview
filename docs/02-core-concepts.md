# Core Concepts

GitFlow Preview is based on a small set of branch roles and merge rules. The workflow is intentionally strict about where truth lives and where validation happens.

## Source of Truth

`master` and `development` are the source-of-truth branches.

- `master` represents production.
- `development` represents accepted integration for the next release.

`preview` is not a source of truth. It is a review surface.

## Validation Surface

The `preview` branch exists to deploy a version of the application that QA, clients, designers, and stakeholders can inspect.

`preview` MAY contain:

- One feature under review.
- Several features under review.
- A hotfix that must also be visible before or after release.
- Temporary validation commits created by CI/CD automation, if the team allows them.

`preview` MUST NOT be used as the basis for stable integration.

## Independent Features

Each `feature/*` branch SHOULD represent one coherent change. A feature branch MAY be deployed to `preview` before it is accepted into `development`.

If a feature is accepted, the same branch MUST be merged into `development`.

## Controlled Release

Production releases flow from `development` to `master`.

The `master` branch MUST represent production-ready code. Teams MAY choose manual approvals, release tags, or automated deployment gates before deploying `master`.

## Emergency Fixes

Urgent production defects are handled with `hotfix/*` branches.

A hotfix MUST be merged into:

- `master`
- `development`
- `preview`

This keeps production fixed, future integration current, and preview aligned with the emergency correction.

## Normative Rules

- Teams MUST maintain `master` as the production branch.
- Teams MUST maintain `development` as the stable integration branch.
- Teams MUST treat `preview` as a validation branch.
- Teams MUST NOT merge `preview` into `development`.
- Teams SHOULD merge accepted features from `feature/*` into `development`.
- Teams MAY deploy `preview` automatically on every update.
- Teams SHOULD protect `master` and `development` with review and CI checks.
- Teams MAY protect `preview` while still allowing fast validation merges.
