# Branch Model

GitFlow Preview defines five branch categories.

## `master`

`master` is the production branch.

Rules:

- `master` MUST contain production-ready code.
- `master` SHOULD be protected.
- `master` SHOULD receive changes from `development` for normal releases.
- `master` MAY receive changes from `hotfix/*` for urgent production fixes after validation.
- Deployments from `master` SHOULD target production.

## `development`

`development` is the stable integration branch.

Rules:

- `development` MUST contain accepted work for the next production release.
- `development` SHOULD be protected by pull request review and CI checks.
- `development` MUST receive accepted features from `feature/*`.
- `development` MUST receive hotfixes from `hotfix/*`.
- `development` MUST NOT receive merges from `preview`.

## `preview`

`preview` is the validation branch.

Rules:

- `preview` MUST be used for validation, QA, visual review, and client approval.
- `preview` MAY receive work from `feature/*`.
- `preview` MUST receive urgent fixes from `hotfix/*`.
- `preview` MUST NOT be treated as canonical history.
- `preview` MUST NOT be merged into `development`.
- `preview` SHOULD be reset, rebuilt, or refreshed from known sources when needed.

## `feature/*`

`feature/*` branches isolate individual changes.

Rules:

- A feature branch SHOULD branch from `development`.
- A feature branch MAY be merged into `preview` for validation.
- An accepted feature branch MUST be merged into `development`.
- A rejected feature branch MUST NOT be merged into `development`.
- A feature branch SHOULD be deleted after it has been accepted, merged, and no longer needs validation.

Example:

```bash
git checkout development
git pull origin development
git checkout -b feature/customer-report
```

## `hotfix/*`

`hotfix/*` branches are used for urgent production corrections.

Rules:

- A hotfix branch SHOULD branch from `master`.
- A hotfix SHOULD be merged into `preview` first for validation.
- A hotfix SHOULD be merged into `development` after validation.
- A hotfix MUST be merged into `master` for production release.
- A hotfix MAY be merged into `master` before `preview` and `development` only for critical production incidents.
- A hotfix SHOULD be tagged after release.

Example:

```bash
git checkout master
git pull origin master
git checkout -b hotfix/login-timeout
```

## Branch Interaction Model

Diagrams and workflow descriptions SHOULD show only the branches that participate in the specific flow being explained.

The `diagrams/gitgraph/` directory contains branch-history examples. The `diagrams/sequences/` directory contains process examples. Diagram filenames are numbered in README order and include their diagram type, such as `01-gitgraph-feature-flow.mmd` and `04-sequence-feature-flow.mmd`.

Recommended interaction boundaries:

| Branch | Interacts With | Notes |
| --- | --- | --- |
| `master` | `development`, `hotfix/*` | Receives normal releases from `development` and validated urgent fixes from `hotfix/*`. |
| `development` | `feature/*`, `hotfix/*` | Receives accepted features and synchronized hotfixes. |
| `preview` | `feature/*`, `hotfix/*` | Receives validation merges only. It does not merge into `development` or `master`. |
| `feature/*` | `preview`, `development` | Validates in `preview`, then integrates into `development` when accepted. |
| `hotfix/*` | `preview`, `development`, `master` | Branches from `master`, validates in `preview`, synchronizes into `development`, then releases to `master`. |

## Branch Comparison

| Workflow | Production Branch | Integration Branch | Preview Branch | Merge Discipline | Best For |
| --- | --- | --- | --- | --- | --- |
| GitFlow Preview | `master` | `development` | `preview` | Feature branches validate in `preview`, then merge separately into `development` | Teams with QA/client preview environments |
| GitFlow | `master` | `develop` | None by default | Release branches coordinate production readiness | Scheduled release cycles |
| GitHub Flow | `main` | None | Optional via deployments | Short-lived branches merge directly to main | Continuous deployment and small teams |
| Trunk-Based Development | `main` | None | Optional via environment rules | Very short branches or direct trunk commits | High-automation teams with strong test suites |
