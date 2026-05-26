# Hotfix Workflow

The hotfix workflow handles urgent production corrections while keeping all long-lived branches aligned.

GitFlow Preview recommends a validated hotfix order: create the hotfix from `master`, validate it in `preview`, synchronize it into `development`, and release it to `master` only after validation passes.

## Overview

Hotfix branches SHOULD be created from `master`.

```text
master -> hotfix/*
hotfix/* -> preview
hotfix/* -> development
hotfix/* -> master
```

## Create a Hotfix Branch

```bash
git checkout master
git pull origin master
git checkout -b hotfix/fix-payment-timeout
```

## Implement the Fix

```bash
git add .
git commit -m "Fix payment timeout"
git push -u origin hotfix/fix-payment-timeout
```

## Validate in Preview

```bash
git checkout preview
git pull origin preview
git merge --no-ff hotfix/fix-payment-timeout
git push origin preview
```

This deploys the hotfix to the preview environment for QA, smoke testing, or stakeholder validation.

## Merge Back to Development

```bash
git checkout development
git pull origin development
git merge --no-ff hotfix/fix-payment-timeout
git push origin development
```

This prevents the bug from returning in the next release.

## Merge to Production

```bash
git checkout master
git pull origin master
git merge --no-ff hotfix/fix-payment-timeout
git push origin master
```

The production deployment SHOULD be triggered from `master` after the fix has passed preview validation and has been synchronized into `development`.

## Tag the Hotfix

```bash
git checkout master
git tag v1.0.1
git push origin v1.0.1
```

## Rules

- A hotfix branch MUST be created for urgent production corrections.
- A hotfix SHOULD branch from `master`.
- A hotfix SHOULD be merged into `preview` first for validation.
- A hotfix SHOULD be merged into `development` after validation.
- A hotfix MUST be merged into `master` for production release.
- A hotfix MAY be merged into `master` before `preview` and `development` only for critical production incidents.
- A hotfix SHOULD be tagged after deployment.
