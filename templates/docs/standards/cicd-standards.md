# CI/CD Standards

> **Scope**: Pipelines, deployments, quality gates

## Pipeline Stages

```
Build → Test → Security → Deploy
```

| Stage | Purpose | Max Duration |
|-------|---------|--------------|
| Build | Compile, install deps | < 5 min |
| Test | Unit + integration | < 10 min |
| Security | Dependency scan, SAST | < 5 min |
| Deploy | Release to environment | < 10 min |

- Fail fast: stop pipeline if any stage fails
- Cache dependencies for faster builds

## Quality Gates

**Pre-merge (required):**
- [ ] Build passes
- [ ] Tests pass (≥80% coverage)
- [ ] Lint passes
- [ ] Security scan passes
- [ ] Code review approved

**Pre-production:**
- [ ] Integration tests pass in staging
- [ ] Manual approval for production

## Deployment

```
Development → Staging → Production
```

- Never skip environments (except emergency hotfixes)
- Use blue-green or canary for production
- Always have rollback procedure ready

```yaml
# Health check after deploy
- name: Verify deployment
  run: |
    curl -f $APP_URL/health || exit 1
```

## Environment Config

```yaml
# ✅ Use secrets storage
env:
  DATABASE_URL: ${{ secrets.DATABASE_URL }}
  API_KEY: ${{ secrets.API_KEY }}

# ❌ Never hardcode
env:
  DATABASE_URL: "postgresql://prod/db"  # NEVER
```

Same artifact deploys to all environments; only config differs.

## Boundaries

- ✅ Always: Run full test suite, scan dependencies, notify on deploy
- ⚠️ Ask first: Pipeline changes, adding stages, deployment strategy changes
- 🚫 Never: Deploy without tests, skip staging for production, hardcode secrets

## Notes

- Embed build info (version, commit, timestamp) in artifacts
- Track pipeline metrics: duration, success rate
- Smoke test after every deployment
