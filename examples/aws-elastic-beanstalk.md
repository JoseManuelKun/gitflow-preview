# AWS Elastic Beanstalk Deployment Example

This example shows how GitFlow Preview can map branches to AWS Elastic Beanstalk environments.

## Environment Mapping

| Branch | Elastic Beanstalk Environment |
| --- | --- |
| `master` | `app-production` |
| `development` | `app-staging` |
| `preview` | `app-preview` |

`preview` MUST deploy only to a non-production Elastic Beanstalk environment.

## Recommended Flow

```text
feature/*   -> preview       -> app-preview
feature/*   -> development   -> app-staging
development -> master        -> app-production
hotfix/*    -> master        -> app-production
hotfix/*    -> development   -> app-staging
hotfix/*    -> preview       -> app-preview
```

## Example GitHub Actions Deployment Step

```yaml
- name: Deploy to Elastic Beanstalk preview
  if: github.ref == 'refs/heads/preview'
  uses: einaregilsson/beanstalk-deploy@v22
  with:
    aws_access_key: ${{ secrets.AWS_ACCESS_KEY_ID }}
    aws_secret_key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
    application_name: my-application
    environment_name: app-preview
    version_label: preview-${{ github.sha }}
    region: us-east-1
    deployment_package: app.zip
```

## Example Branch Commands

```bash
git checkout preview
git pull origin preview
git merge --no-ff feature/client-dashboard
git push origin preview
```

After QA or client approval:

```bash
git checkout development
git pull origin development
git merge --no-ff feature/client-dashboard
git push origin development
```

Do not merge `preview` into `development`.

## Operational Notes

- Store AWS credentials in CI/CD secrets.
- Use separate Elastic Beanstalk environments for production, staging, and preview.
- Use separate databases or sanitized data for preview.
- Require manual approval before production deployment if the organization requires release control.
