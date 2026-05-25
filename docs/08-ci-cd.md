# CI/CD

GitFlow Preview works best when branch updates trigger predictable deployments.

## Branch Deployment Mapping

| Branch | Environment | Deployment Type |
| --- | --- | --- |
| `master` | Production | Automatic with approval or fully automatic after checks. |
| `development` | Integration or staging | Automatic integration deployment. |
| `preview` | Preview, QA, or client review | Automatic validation deployment. |
| `feature/*` | Optional ephemeral environment | Optional per-feature deployment. |
| `hotfix/*` | Optional test environment | Optional validation before production. |

## Required CI Checks

All protected branches SHOULD require:

- Dependency installation.
- Linting.
- Automated tests.
- Build verification.
- Security checks when applicable.

## Deployment Rules

- `master` SHOULD deploy to production.
- `development` SHOULD deploy to integration or staging.
- `preview` SHOULD deploy to a preview environment.
- `preview` MUST NOT deploy to production.
- `feature/*` MAY deploy to ephemeral environments.
- `hotfix/*` MAY deploy to temporary validation environments.

## Merge Gates

Recommended branch protection:

| Target Branch | Required Review | Required CI | Notes |
| --- | --- | --- | --- |
| `master` | Yes | Yes | Production branch. |
| `development` | Yes | Yes | Stable integration branch. |
| `preview` | Optional | Yes | Validation branch; faster review may be acceptable. |

## CI/CD Examples

See:

- [Bitbucket Pipelines](../examples/bitbucket-pipelines.yml)
- [GitHub Actions](../examples/github-actions.yml)
- [AWS Elastic Beanstalk](../examples/aws-elastic-beanstalk.md)
- [IONOS Webhook Deploy](../examples/ionos-webhook-deploy.md)

## Pipeline Principle

CI/CD MUST reinforce the branch model.

Automation SHOULD make the correct path easy:

```text
feature/*   -> preview       -> preview deployment
feature/*   -> development   -> integration deployment
development -> master        -> production deployment
hotfix/*    -> master        -> production deployment
hotfix/*    -> development   -> integration deployment
hotfix/*    -> preview       -> preview deployment
```

Automation MUST NOT promote `preview` directly into `development` or production.
