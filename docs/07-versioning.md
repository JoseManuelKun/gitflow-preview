# Versioning

GitFlow Preview recommends Semantic Versioning for production releases and pre-release identifiers for preview and release-candidate builds.

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

Preview builds MAY use the `preview` pre-release identifier.

Example:

```text
v1.1.0-preview.1
v1.1.0-preview.2
```

Preview versions SHOULD be generated from the `preview` branch or from CI metadata associated with preview deployments.

Preview versions MUST NOT imply that `preview` is a release source. They identify validation builds only.

## Release Candidates

Release candidates MAY use the `rc` pre-release identifier.

Example:

```text
v1.1.0-rc.1
v1.1.0-rc.2
```

Release candidates SHOULD be based on `development` after the intended release scope has been accepted.

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
| `v1.1.0-preview.1` | `preview` | Preview validation build. |
| `v1.1.0-rc.1` | `development` | Release candidate. |

## Rules

- Production tags MUST be created from `master`.
- Preview identifiers MAY be created from `preview`.
- Release candidates SHOULD be created from `development`.
- Preview tags MUST NOT be promoted directly to production unless the underlying commits have reached `development` and then `master` through the approved flow.
