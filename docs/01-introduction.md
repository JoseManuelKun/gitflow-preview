# Introduction

GitFlow Preview is a Git workflow designed for teams that need a reliable separation between implementation, visual validation, stable integration, and production release.

Traditional GitFlow works well for release-oriented teams, but many modern teams deploy continuously to non-production environments. They need a branch that can feed a preview environment for QA, client review, design approval, and stakeholder validation without becoming the authoritative integration branch.

GitFlow Preview introduces that role explicitly through the `preview` branch.

## Purpose

GitFlow Preview provides a formal model for:

- Isolated feature development.
- Automated deployment to preview environments.
- QA and client validation before stable integration.
- Clean promotion from `development` to `master`.
- Emergency hotfix handling across production, integration, and preview.

## Key Principle

`preview` is a validation branch, not a source of truth.

The canonical long-lived branches are:

- `master` for production.
- `development` for stable integration.

The `preview` branch MAY contain a temporary combination of features under review. Because of that, it MUST NOT be merged into `development`.

## Why Not Merge Preview Back?

The `preview` branch can contain work that is incomplete, rejected, reordered, or validated only for visual review. Merging `preview` into `development` would make temporary validation history part of the stable integration history.

Accepted work MUST move from its original `feature/*` branch into `development`.

## Official Flow

```text
feature/*   -> preview       # validation
feature/*   -> development   # accepted integration
development -> master        # production release
hotfix/*    -> preview       # validate urgent fix
hotfix/*    -> development   # keep integration current
hotfix/*    -> master        # production fix
```

## Audience

GitFlow Preview is intended for:

- Product teams with QA and client validation stages.
- Agencies building client-facing applications.
- SaaS teams with automated preview deployments.
- Teams that use Bitbucket Pipelines, GitHub Actions, AWS Elastic Beanstalk, IONOS, or similar deployment platforms.

## Status

This project documents the GitFlow Preview methodology as a publishable, implementation-ready workflow.
