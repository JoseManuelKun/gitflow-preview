# GitFlow Preview

**A Git workflow for preview-driven teams.**

GitFlow Preview is a formal Git workflow for teams that use feature branches, automated CI/CD, preview environments, QA validation, client review, and controlled production releases.

It is inspired by GitFlow, but it changes the role of the validation branch. In GitFlow Preview, `preview` is a deployment and review branch. It is not a source of truth, and it MUST NOT be merged back into `development`.

## Core Idea

GitFlow Preview separates three concerns:

| Concern | Branch | Purpose |
| --- | --- | --- |
| Production | `master` | Code currently released or ready to release to production. |
| Stable integration | `development` | Integrated, reviewed, and accepted work for the next release. |
| Visual validation | `preview` | Temporary validation surface for QA, stakeholder review, and client feedback. |
| Isolated work | `feature/*` | Independent implementation of a specific change. |
| Emergency fixes | `hotfix/*` | Urgent production corrections. |

## Required Flow

The approved merge paths are:

```text
feature/*   -> preview
feature/*   -> development
development -> master
hotfix/*    -> preview
hotfix/*    -> development
hotfix/*    -> master
```

The prohibited merge path is:

```text
preview -> development
```

`preview` MAY receive feature branches for validation, but it MUST NOT be treated as canonical history. If a feature is accepted, the same `feature/*` branch MUST be merged into `development`.

Hotfixes SHOULD be validated in `preview` before they are synchronized into `development` and released through `master`. For critical incidents, teams MAY release a hotfix to `master` first when production impact requires immediate action.

## Documentation

- [Introduction](docs/01-introduction.md)
- [Core Concepts](docs/02-core-concepts.md)
- [Branch Model](docs/03-branch-model.md)
- [Feature Workflow](docs/04-feature-workflow.md)
- [Preview Workflow](docs/05-preview-workflow.md)
- [Hotfix Workflow](docs/06-hotfix-workflow.md)
- [Versioning](docs/07-versioning.md)
- [CI/CD](docs/08-ci-cd.md)
- [Examples](docs/09-examples.md)
- [FAQ](docs/10-faq.md)

## Diagrams

GitFlow Preview provides Git graph and sequence diagram examples for feature delivery, validated hotfixes, and production releases.

Git graphs:

- [Feature Flow Git Graph](diagrams/gitgraph/01-gitgraph-feature-flow.mmd)
- [Hotfix Flow Git Graph](diagrams/gitgraph/02-gitgraph-hotfix-flow.mmd)
- [Production Release Git Graph](diagrams/gitgraph/03-gitgraph-production-release.mmd)

Sequence diagrams:

- [Feature Flow Sequence](diagrams/sequences/04-sequence-feature-flow.mmd)
- [Hotfix Flow Sequence](diagrams/sequences/05-sequence-hotfix-flow.mmd)
- [Production Release Sequence](diagrams/sequences/06-sequence-production-release.mmd)

## CI/CD Examples

- [Bitbucket Pipelines](examples/bitbucket-pipelines.yml)
- [GitHub Actions](examples/github-actions.yml)
- [AWS Elastic Beanstalk](examples/aws-elastic-beanstalk.md)
- [IONOS Webhook Deploy](examples/ionos-webhook-deploy.md)

## Contributing

Before contributing, read:

- [Contributing Guide](CONTRIBUTING.md)
- [Code of Conduct](CODE_OF_CONDUCT.md)
- [Security Policy](SECURITY.md)

Pull requests are validated by the `Validate Pull Request Flow` GitHub Actions workflow. In particular, pull requests from `preview` into `development` are blocked because `preview` is a validation branch, not a source of truth.

## Normative Language

This documentation uses RFC-style terms:

- **MUST** and **MUST NOT** define mandatory rules.
- **SHOULD** and **SHOULD NOT** define strong recommendations.
- **MAY** defines optional behavior.

## License

This project is released under the MIT License. See [LICENSE](LICENSE).
