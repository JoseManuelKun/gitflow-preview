# IONOS Webhook Deployment Example

This example shows a simple webhook-based deployment model for IONOS hosting.

## Environment Mapping

| Branch | Target |
| --- | --- |
| `master` | Production web root |
| `development` | Staging directory or subdomain |
| `preview` | Preview directory or client-review subdomain |

`preview` MUST NOT deploy to the production web root.

## Recommended Flow

```text
feature/*   -> preview       -> preview.example.com
feature/*   -> development   -> staging.example.com
development -> master        -> www.example.com
hotfix/*    -> master        -> www.example.com
hotfix/*    -> development   -> staging.example.com
hotfix/*    -> preview       -> preview.example.com
```

## Example Webhook Payload

```json
{
  "branch": "preview",
  "commit": "abc123",
  "environment": "preview"
}
```

## Example Deployment Script

```bash
#!/usr/bin/env bash
set -euo pipefail

BRANCH="${1:-preview}"

case "$BRANCH" in
  master)
    TARGET="/home/customer/www/example.com/public_html"
    ;;
  development)
    TARGET="/home/customer/www/staging.example.com/public_html"
    ;;
  preview)
    TARGET="/home/customer/www/preview.example.com/public_html"
    ;;
  *)
    echo "Unsupported branch: $BRANCH"
    exit 1
    ;;
esac

git fetch origin "$BRANCH"
git checkout "$BRANCH"
git reset --hard "origin/$BRANCH"
npm ci
npm run build
rsync -av --delete dist/ "$TARGET/"
```

## Security Notes

- Webhook endpoints MUST validate a shared secret or signature.
- Deployment users SHOULD have the minimum required filesystem permissions.
- Production deployment SHOULD require additional protection.
- Preview deployments MAY be automatic on every push to `preview`.

## Acceptance Reminder

After a feature is approved on the IONOS preview environment, merge the original `feature/*` branch into `development`.

Do not merge `preview` into `development`.
