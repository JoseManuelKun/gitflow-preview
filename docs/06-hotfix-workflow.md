# Hotfix Workflow

The hotfix workflow handles urgent production corrections while keeping all long-lived branches aligned.

## Overview

Hotfix branches SHOULD be created from `master`.

```text
master -> hotfix/*
hotfix/* -> master
hotfix/* -> development
hotfix/* -> preview
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

## Merge to Production

```bash
git checkout master
git pull origin master
git merge --no-ff hotfix/fix-payment-timeout
git push origin master
```

The production deployment SHOULD be triggered from `master`.

## Merge Back to Development

```bash
git checkout development
git pull origin development
git merge --no-ff hotfix/fix-payment-timeout
git push origin development
```

This prevents the bug from returning in the next release.

## Merge to Preview

```bash
git checkout preview
git pull origin preview
git merge --no-ff hotfix/fix-payment-timeout
git push origin preview
```

This keeps the preview environment aligned with the urgent correction.

## Tag the Hotfix

```bash
git checkout master
git tag v1.0.1
git push origin v1.0.1
```

## Rules

- A hotfix branch MUST be created for urgent production corrections.
- A hotfix SHOULD branch from `master`.
- A hotfix MUST be merged into `master`.
- A hotfix MUST be merged into `development`.
- A hotfix MUST be merged into `preview`.
- A hotfix SHOULD be tagged after deployment.
