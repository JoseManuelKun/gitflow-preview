# Feature Workflow

The feature workflow defines how new work moves from isolated implementation to preview validation and stable integration.

## Overview

Feature branches are created from `development`.

```text
development -> feature/*
feature/*   -> preview
feature/*   -> development
```

`preview` is optional for very small internal changes, but it SHOULD be used for changes that require visual inspection, QA, product approval, or client review.

## Create a Feature Branch

```bash
git checkout development
git pull origin development
git checkout -b feature/add-invoice-export
```

## Work Locally

```bash
git add .
git commit -m "Add invoice export"
git push -u origin feature/add-invoice-export
```

## Keep a Feature Updated or Resume Paused Work

A feature branch SHOULD be synchronized with the latest `origin/development` when work resumes after a pause and before final integration. `development` contains accepted work for the next release; `master` contains production releases and may not include all accepted changes.

- Developers MUST commit or stash pending changes before synchronization.
- Developers SHOULD create a local backup branch before integrating a substantial backlog of changes.
- Features MUST NOT merge or rebase onto `preview`, because it may contain unaccepted work.
- Shared feature branches SHOULD use merge to preserve published history. Rebase MAY be used for unpublished work or when all affected collaborators explicitly coordinate the history rewrite.

Fetch current remote references, switch to the existing feature, and integrate stable development:

```bash
git fetch origin
git switch feature/add-invoice-export
git branch backup/add-invoice-export-before-sync
git merge origin/development
```

Use a unique backup branch name for each synchronization. If collaborators have updated the remote feature branch, reconcile those changes before merging `origin/development`; do not overwrite their commits.

If conflicts occur, edit the affected files, remove conflict markers, and complete the merge:

```bash
git status
git add path/to/resolved-file
git commit
```

To cancel an in-progress merge instead:

```bash
git merge --abort
```

Restore any stashed work and resolve resulting conflicts before validation. Developers MUST run the applicable tests and CI checks after synchronization. Changes requiring preview validation MUST be validated again before acceptance; an earlier approval does not automatically cover the updated feature. Preview tags MUST only identify approved validation snapshots, not every synchronization or push.

After local verification, push the feature branch when ready and follow the usual pull request process into `preview`, then `development`. Synchronization itself neither accepts the feature nor publishes a release.

```text
development -> feature/*   # Refresh the feature's stable base
feature/* -> preview       # Validate the updated work
feature/* -> development   # Accept the approved work
```

The synchronization direction `development -> feature/*` is a maintenance operation, not a new allowed pull request source into a protected principal branch.

If a production hotfix is missing from `development`, teams SHOULD complete its synchronization through the [hotfix workflow](06-hotfix-workflow.md) before refreshing the feature. Merging `master` only into the feature does not repair the missing integration in `development`.

## Send the Feature to Preview

Merge the feature branch into `preview` to trigger a preview deployment.

```bash
git checkout preview
git pull origin preview
git merge --no-ff feature/add-invoice-export
git push origin preview
```

Alternatively, teams MAY use a pull request from `feature/add-invoice-export` into `preview`.

## Validate

QA, product owners, designers, or clients review the deployed preview.

Outcomes:

- If accepted, merge the feature branch into `development`.
- If rejected, continue working on the same `feature/*` branch.
- If postponed, leave the feature out of `development`.

## Merge Accepted Feature to Development

```bash
git checkout development
git pull origin development
git merge --no-ff feature/add-invoice-export
git push origin development
```

Teams using pull requests SHOULD open a pull request from `feature/add-invoice-export` into `development`.

## Mandatory Rule

Do not merge `preview` into `development`.

Correct:

```text
feature/add-invoice-export -> development
```

Incorrect:

```text
preview -> development
```

## Cleanup

After the feature is accepted and merged into `development`, the team MAY delete the feature branch.

```bash
git branch -d feature/add-invoice-export
git push origin --delete feature/add-invoice-export
```
