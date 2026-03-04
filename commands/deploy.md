---
description: |
  Deploy, configure Docker, CI/CD, or manage infrastructure. Includes server hardening.
  Examples: /deploy "docker setup", /deploy "github actions", /deploy "harden server"
argument-hint: [deploy, docker, ci/cd, or infrastructure task]
allowed-tools: Read, Edit, Bash, Grep, Glob, Write, mcp_codex-bridge, mcp_gemini-bridge, mcp_context7, mcp_playwright, mcp_zread, mcp_web-search-prime, mcp_web-reader, mcp_zai-mcp-server, mcp_open-bridge
---
# Deploy Mode

**Routes to:** `devops-engineer` agent
**Skill:** `devops-infrastructure`

> Always create rollback plan before making changes. Test configs before applying.

## Task
$ARGUMENTS

## Instructions

1. Check current infrastructure configuration
2. Understand the deployment goal
3. Propose changes with rollback plan
4. Implement with safety checks
5. Verify deployment

## Quick Actions

| Task | Command |
|------|---------|
| Docker build | `docker compose build` |
| Docker up | `docker compose up -d` |
| Laravel deploy | See deploy script below |

### Laravel Deployment Script
```bash
php artisan down --retry=60
git pull origin main
composer install --no-dev --optimize-autoloader
php artisan migrate --force
php artisan config:cache
php artisan route:cache
php artisan view:cache
php artisan up
```

## Output Format

```markdown
## 🚀 Deployment

### Changes
| File | Action |
|------|--------|
| ... | ... |

### Verification
$ [command]
[output]

### Rollback Plan
[How to revert if needed]
```

## Examples
- `/deploy docker setup`
- `/deploy github actions`
- `/deploy production`
