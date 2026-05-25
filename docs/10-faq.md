# FAQ

## Is GitFlow Preview the same as GitFlow?

No. GitFlow Preview is inspired by GitFlow, but it adds a formal `preview` branch for QA, visual validation, and client review. It also makes clear that `preview` is not a source of truth.

## Is `preview` the same as `development`?

No. `development` is stable integration. `preview` is validation.

`development` contains accepted work. `preview` MAY contain temporary or unaccepted work.

## Can `preview` be merged into `development`?

No. `preview` MUST NOT be merged into `development`.

Accepted changes MUST be merged from their original `feature/*` branches into `development`.

## Why merge a feature twice?

The feature is not merged twice for the same purpose.

- `feature/* -> preview` validates the work in a deployed environment.
- `feature/* -> development` accepts the work into stable integration.

This keeps validation separate from canonical integration.

## What happens if a feature is rejected in preview?

Continue working on the `feature/*` branch or abandon it. Do not merge rejected work into `development`.

## Can multiple features be merged into `preview`?

Yes. `preview` MAY contain multiple features for combined validation. Teams SHOULD be aware that this can make it harder to identify which feature caused a defect.

## Can `preview` be reset?

Yes. Teams MAY refresh or recreate `preview` from `development` when old validation history is no longer useful. If the remote branch is rewritten, the team SHOULD be notified.

## Where do production releases come from?

Production releases come from `master`.

Normal releases flow from:

```text
development -> master
```

## Where do hotfixes go?

Hotfixes MUST be merged into:

```text
hotfix/* -> master
hotfix/* -> development
hotfix/* -> preview
```

## Can this workflow use pull requests?

Yes. Pull requests are recommended for `master` and `development`. Teams MAY also use pull requests for `preview`, especially when preview deployments should be auditable.

## Can feature branches deploy directly?

Yes. `feature/*` branches MAY deploy to ephemeral environments. This does not replace `preview`; it is an optional additional validation layer.

## Should the production branch be named `master` or `main`?

This specification uses `master` because GitFlow Preview defines `master` as the production branch. Teams MAY adapt the name to `main` if required by organizational policy, but the branch role and merge rules should remain unchanged.
