# Examples

This page provides practical command examples for GitFlow Preview.

## Initialize Long-Lived Branches

```bash
git checkout -b master
git push -u origin master

git checkout -b development
git push -u origin development

git checkout -b preview
git push -u origin preview
```

If `master` already exists, create `development` and `preview` from the desired baseline:

```bash
git checkout master
git pull origin master
git checkout -b development
git push -u origin development
git checkout -b preview
git push -u origin preview
```

## Feature Validation and Acceptance

```bash
git checkout development
git pull origin development
git checkout -b feature/profile-settings

git add .
git commit -m "Add profile settings"
git push -u origin feature/profile-settings

git checkout preview
git pull origin preview
git merge --no-ff feature/profile-settings
git push origin preview

git checkout development
git pull origin development
git merge --no-ff feature/profile-settings
git push origin development
```

## Production Release

```bash
git checkout development
git pull origin development

git checkout master
git pull origin master
git merge --no-ff development
git tag v1.1.0
git push origin master
git push origin v1.1.0
```

## Preview Build Tag

```bash
git checkout preview
git pull origin preview
git tag v1.1.0-preview.1
git push origin v1.1.0-preview.1
```

## Release Candidate Tag

```bash
git checkout development
git pull origin development
git tag v1.1.0-rc.1
git push origin v1.1.0-rc.1
```

## Hotfix

```bash
git checkout master
git pull origin master
git checkout -b hotfix/fix-login-error

git add .
git commit -m "Fix login error"
git push -u origin hotfix/fix-login-error

git checkout preview
git pull origin preview
git merge --no-ff hotfix/fix-login-error
git push origin preview

git checkout development
git pull origin development
git merge --no-ff hotfix/fix-login-error
git push origin development

git checkout master
git pull origin master
git merge --no-ff hotfix/fix-login-error
git push origin master
```

## Prohibited Merge

This command MUST NOT be used as part of the workflow:

```bash
git checkout development
git merge preview
```

Merge accepted feature branches directly into `development` instead.
