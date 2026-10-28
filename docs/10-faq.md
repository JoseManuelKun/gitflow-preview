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

## How do I resume a feature after a long pause?

Save pending work, fetch remote updates, and merge `origin/development` into the existing `feature/*` branch. Use `development`, not `preview`, as the stable integration base. `master` represents production and may lack accepted work intended for the next release.

Shared branches SHOULD use merge rather than rewrite published history. Resolve conflicts, run the applicable tests, and repeat required preview validation before acceptance. An earlier approval does not automatically apply to the updated feature.

This maintenance merge does not approve or release the feature. See [Keep a Feature Updated or Resume Paused Work](04-feature-workflow.md#keep-a-feature-updated-or-resume-paused-work) for commands, backup guidance, and conflict recovery.

## What if a production hotfix is missing from `development`?

Teams SHOULD complete the hotfix's synchronization into `development` through the [hotfix workflow](06-hotfix-workflow.md), then refresh the feature from `origin/development`. Updating only the feature from `master` leaves the stable integration branch incomplete.

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

Hotfixes SHOULD be validated before production release:

```text
hotfix/* -> preview
hotfix/* -> development
hotfix/* -> master
```

For critical production incidents, teams MAY release a hotfix to `master` first when immediate production impact requires it.

## Can this workflow use pull requests?

Yes. Pull requests are recommended for `master` and `development`. Teams MAY also use pull requests for `preview`, especially when preview deployments should be auditable.

## Can feature branches deploy directly?

Yes. `feature/*` branches MAY deploy to ephemeral environments. This does not replace `preview`; it is an optional additional validation layer.

## Should the production branch be named `master` or `main`?

This specification uses `master` because GitFlow Preview defines `master` as the production branch. Teams MAY adapt the name to `main` if required by organizational policy, but the branch role and merge rules should remain unchanged.
