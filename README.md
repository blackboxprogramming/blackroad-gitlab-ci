[![CI Status](https://img.shields.io/github/actions/workflow/status/blackboxprogramming/blackroad-gitlab-ci/auto-deploy.yml?label=deploy)](https://github.com/blackboxprogramming/blackroad-gitlab-ci/actions)
[![Security](https://img.shields.io/github/actions/workflow/status/blackboxprogramming/blackroad-gitlab-ci/security-scan.yml?label=security)](https://github.com/blackboxprogramming/blackroad-gitlab-ci/actions)
[![License](https://img.shields.io/badge/License-Proprietary-red.svg)](LICENSE)

# BlackRoad CI/CD Pipeline

Production-ready CI/CD template for multi-platform deployments across BlackRoad OS, Inc. infrastructure.

---

## Workflows

| Workflow | Trigger | Description |
|----------|---------|-------------|
| **Auto Deploy** | Push to `main`/`master`, manual | Detects service type and deploys to Cloudflare Pages, Cloudflare Workers, or Railway |
| **Cloudflare Worker** | Push to `workers/**`, manual | Dedicated Cloudflare Worker deployment for long-running tasks and cron jobs |
| **Security Scan** | Push, PR, weekly schedule | CodeQL analysis, dependency audit, pip audit, secret detection |
| **Self-Healing** | Every 30 min, post-deploy | Health monitoring, auto-rollback, auto-issue creation |
| **Automerge** | PR events | Auto-approves and merges Dependabot patch/minor updates |

## Supported Platforms

| Platform | Service Types | Detection |
|----------|--------------|-----------|
| **Cloudflare Pages** | Next.js, static sites | `next.config.{mjs,js,ts}` or no framework detected |
| **Cloudflare Workers** | Workers, cron tasks | `wrangler.toml` or `wrangler.jsonc` |
| **Railway** | Docker, Node.js, Python | `Dockerfile`, `package.json`, `requirements.txt` |
| **BlackRoad Platform** | All (via GitLab CI) | GitLab pipeline on `main`/`develop` |

## Required Secrets

Configure these in your repository settings:

| Secret | Platform | Required |
|--------|----------|----------|
| `CLOUDFLARE_API_TOKEN` | Cloudflare | Yes (for CF deploys) |
| `CLOUDFLARE_ACCOUNT_ID` | Cloudflare | Yes (for CF deploys) |
| `RAILWAY_TOKEN` | Railway | Yes (for Railway deploys) |
| `DEPLOY_URL` | All | Yes (for health checks) |
| `NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY` | Clerk | If using Clerk auth |
| `BLACKROAD_API_KEY` | BlackRoad | Yes (for GitLab CI) |

## Security

All GitHub Actions are pinned to specific commit hashes for supply-chain security:

```yaml
# Example — pinned to exact commit, version noted in comment
- uses: actions/checkout@34e114876b0b11c390a56381ad16ebd13914f8d5 # v4.3.1
```

Security scanning includes:
- **CodeQL** — static analysis for JavaScript, TypeScript, and Python
- **Dependency audit** — npm and pip vulnerability scanning
- **Secret detection** — scans for hardcoded API keys, tokens, and credentials
- **Dependency review** — blocks PRs introducing known vulnerabilities

## Automerge

Dependabot PRs for patch and minor updates are automatically approved and merged. Major version updates require manual review. Add the `automerge` label to any PR to enable auto-merge after checks pass.

## Self-Healing

The self-healing pipeline runs every 30 minutes and after each deployment:
1. Checks the `/api/health` endpoint
2. Triggers rollback to the previous commit on failure
3. Creates or updates a GitHub issue for tracking

## Quick Start

1. Fork or copy this repository's `.github/` directory into your project
2. Configure the required secrets in your repository settings
3. Push to `main` to trigger the first deployment
4. Verify the health check endpoint at `/api/health`

## Stripe & Product Integration

Stripe products, pricing, and related payment assets are managed through the BlackRoad platform. Configure `STRIPE_SECRET_KEY` and `STRIPE_WEBHOOK_SECRET` in your deployment environment for payment processing.

## Vercel, Railway & Database

- **Vercel**: Supported via separate `vercel.json` in target repositories
- **Railway**: Auto-detected via `Dockerfile` or `package.json`
- **Database**: Configure `DATABASE_URL` in your deployment platform's environment variables

---

## License & Copyright

**Copyright (c) 2024-2026 BlackRoad OS, Inc. All Rights Reserved.**

**CEO:** Alexa Amundson

**PROPRIETARY AND CONFIDENTIAL** — This software is not open source.

This is the proprietary property of BlackRoad OS, Inc. and is **not licensed for commercial resale, redistribution, or modification** without written permission.

- **Permitted:** Testing, evaluation, and educational purposes
- **Prohibited:** Commercial use, resale, redistribution, AI model training

### Enterprise Scale

Designed to support:
- 30,000 AI Agents
- 30,000 Human Employees
- Single Operator: Alexa Amundson (CEO)

### Contact

For commercial licensing inquiries:
- **Email:** blackroad.systems@gmail.com
- **Organization:** BlackRoad OS, Inc.

See [LICENSE](LICENSE) for complete terms.
