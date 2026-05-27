# Versioning

GitFlow Preview recommends Semantic Versioning for production releases and pre-release identifiers for approved preview validations and release-candidate builds.

## Semantic Versioning

Production releases SHOULD use SemVer:

```text
vMAJOR.MINOR.PATCH
```

Examples:

```text
v1.0.0
v1.0.1
v1.1.0
```

## Preview Versions

Approved preview validations MAY use the `preview` pre-release identifier.

Example:

```text
v1.1.0-preview.1
v1.1.0-preview.2
```

Preview versions SHOULD be created only after a validation state has been approved by QA, product, client review, or another team-defined approval gate.

Preview versions SHOULD NOT be created for every push to `preview`.

Preview versions MUST NOT imply that `preview` is a release source. They identify approved validation states only.

## Release Candidates

Release candidates MAY use the `rc` pre-release identifier.

Example:

```text
v1.1.0-rc.1
v1.1.0-rc.2
```

Release candidates SHOULD be based on `development` after the intended release scope has been accepted and is stable enough for release-candidate review.

## Production Tags

Production tags MUST be created from `master`.

Example:

```bash
git checkout master
git pull origin master
git tag v1.1.0
git push origin v1.1.0
```

## Recommended Mapping

| Version | Source | Meaning |
| --- | --- | --- |
| `v1.0.0` | `master` | Production release. |
| `v1.1.0-preview.1` | `preview` | Approved preview validation. |
| `v1.1.0-rc.1` | `development` | Release candidate. |

## Rules

- Tags SHOULD identify approved or formal states, not every push.
- Production tags MUST be created from `master`.
- Preview tags SHOULD be created from `preview` only after validation approval.
- Release candidates SHOULD be created from `development`.
- Preview tags MUST NOT be promoted directly to production unless the underlying commits have reached `development` and then `master` through the approved flow.
