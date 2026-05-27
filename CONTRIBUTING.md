# Contributing

Thank you for contributing to GitFlow Preview.

This project documents a formal Git workflow. Contributions should preserve the branch model, normative rules, and professional documentation tone.

## Central Rule

Pull requests into `development` must come from `feature/*`, `hotfix/*`, `bugfix/*`, `docs/*`, or `chore/*`.

Pull requests from `preview` into `development` are not allowed.

The `preview` branch is only a validation environment and must not be used as a source of truth.

## Approved Branch Flow

Contributors MUST follow the documented workflow:

```text
feature/*   -> preview
feature/*   -> development
development -> master
hotfix/*    -> preview
hotfix/*    -> development
hotfix/*    -> master
```

Blocked flows:

```text
preview -> development
preview -> master
master  -> development
```

## Branch Naming

Use one of the following branch prefixes:

```text
feature/*
bugfix/*
hotfix/*
docs/*
chore/*
```

Examples:

```text
feature/add-preview-diagram
bugfix/fix-broken-link
hotfix/correct-release-rule
docs/update-faq
chore/update-templates
```

## Pull Requests

Pull requests SHOULD target the branch that matches their purpose:

| Target branch | Allowed source branches | Purpose |
| --- | --- | --- |
| `preview` | `feature/*`, `bugfix/*`, `hotfix/*`, `docs/*`, `chore/*` | Validation, QA, client review, and documentation preview. |
| `development` | `feature/*`, `bugfix/*`, `hotfix/*`, `docs/*`, `chore/*` | Accepted stable integration. |
| `master` | `development`, `hotfix/*` | Production release or urgent production fix. |

Hotfix pull requests SHOULD be opened in this order:

```text
hotfix/* -> preview
hotfix/* -> development
hotfix/* -> master
```

Maintainers MAY approve a direct `hotfix/* -> master` pull request first only for critical production incidents where immediate release is required.

Before opening a pull request:

```bash
git status
git diff
```

Review the documentation for contradictions, especially around this rule:

```text
preview MUST NOT merge into development
```

## Documentation Standards

Contributions SHOULD:

- Use clear, professional English.
- Keep terminology consistent across files.
- Use RFC-style terms only when the rule is intentional.
- Include command examples when a workflow step benefits from them.
- Update diagrams when branch flow behavior changes.
- Avoid introducing rules that make `preview` a source of truth.

## Required Validation

The repository includes a GitHub Actions workflow named `Validate Pull Request Flow`.

This workflow blocks:

- Pull requests from `preview` into `development`.
- Pull requests into `development` from unsupported branch names.
- Pull requests into `preview` from unsupported branch names.
- Pull requests into `master` from anything other than `development` or `hotfix/*`.

## Recommended Branch Protection

Repository maintainers SHOULD configure branch protection rules in GitHub:

```text
Settings -> Branches -> Branch protection rules
```

### `master`

- Require a pull request before merging.
- Require approvals.
- Require status checks to pass.
- Require conversation resolution.
- Restrict who can push.
- Do not allow force pushes.
- Do not allow deletions.

### `development`

- Require a pull request before merging.
- Require status checks to pass.
- Require approvals.
- Require `Validate Pull Request Flow` as a required check.
- Do not allow force pushes.
- Do not allow deletions.

### `preview`

- Require a pull request before merging.
- Require status checks to pass.
- Allow pull requests from `feature/*`, `bugfix/*`, `hotfix/*`, `docs/*`, and `chore/*`.
- Do not allow force pushes.
- Do not allow deletions.

## License

By contributing, you agree that your contributions will be licensed under the MIT License.
