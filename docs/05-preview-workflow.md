# Preview Workflow

The preview workflow defines how teams use the `preview` branch for validation without making it a source of truth.

## Purpose of Preview

`preview` exists to answer one question:

> Is this work acceptable when deployed and inspected?

It supports:

- QA validation.
- Visual review.
- Client approval.
- Product acceptance.
- Design verification.
- Integration smoke testing in a non-production environment.

## Preview Is Not Canonical

`preview` MAY contain a changing combination of branches. It can be ahead of, behind, or different from `development`.

Because of that:

- `preview` MUST NOT be merged into `development`.
- `preview` MUST NOT be used as the release source.
- `preview` MUST NOT be treated as stable integration history.

Accepted changes MUST be merged from their original `feature/*` branches into `development`.

## Common Preview Patterns

### Single Feature Preview

```bash
git checkout preview
git pull origin preview
git merge --no-ff feature/new-dashboard
git push origin preview
```

### Multi-Feature Preview

```bash
git checkout preview
git pull origin preview
git merge --no-ff feature/new-dashboard
git merge --no-ff feature/export-csv
git push origin preview
```

### Refresh Preview from Development

Teams MAY refresh `preview` from `development` when old validation history is no longer useful.

One safe approach is to recreate the branch from `development`:

```bash
git checkout development
git pull origin development
git checkout -B preview
git push --force-with-lease origin preview
```

This is a maintenance reset, not a normal merge path. It does not make `development -> preview` part of the feature acceptance flow.

This operation SHOULD be communicated to the team because it rewrites the remote `preview` branch.

## Preview Deployment

CI/CD SHOULD deploy `preview` automatically to a non-production environment.

Recommended environment names:

- `preview`
- `qa`
- `staging-preview`
- `client-review`

The environment SHOULD be isolated from production data unless explicit data governance rules allow sanitized replicas.

## Acceptance Rule

When work is approved in preview, merge the original feature branch into `development`.

```bash
git checkout development
git pull origin development
git merge --no-ff feature/new-dashboard
git push origin development
```

The `preview` branch itself MUST NOT be merged into `development`.
