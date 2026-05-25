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
