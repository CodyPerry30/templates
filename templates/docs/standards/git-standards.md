# Git Standards

> **Scope**: Branching, commits, pull requests, code review

## Branches

| Prefix | Purpose | Example |
|--------|---------|---------|
| `feature/` | New functionality | `feature/AUTH-123-user-login` |
| `bugfix/` | Non-urgent fixes | `bugfix/BUG-456-null-error` |
| `hotfix/` | Urgent production fixes | `hotfix/SEC-789-xss-patch` |
| `release/` | Release preparation | `release/v2.1.0` |

Format: `<type>/<ticket>-<short-description>`

## Commits

```bash
# Format: <type>(scope): <description>

# ✅ Good commits
feat(auth): add JWT refresh endpoint
fix(api): handle null payment response
docs(readme): update installation steps
refactor(user): extract validation to service

# Breaking changes
feat(api)!: change response format
```

**Types**: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`

## Pull Requests

- Keep PRs small: < 400 lines changed
- Title follows commit message format
- Link to ticket in description
- Required: 1 approval (2 for security/database changes)
- All CI checks must pass

```markdown
## Summary
Brief description of changes

## Related Issue
Closes #123

## Testing
- [ ] Unit tests added
- [ ] Manual testing completed
```

## Code Review

| Prefix | Meaning | Action |
|--------|---------|--------|
| `nit:` | Minor suggestion | Optional |
| `suggestion:` | Consider alternative | Optional |
| `question:` | Need clarification | Response needed |
| `blocking:` | Must fix | Required |

Review within 24 hours; respond to feedback promptly.

## Boundaries

- ✅ Always: Use conventional commits, squash merge features, delete merged branches
- ⚠️ Ask first: Force pushing, changing protected branches
- 🚫 Never: Commit directly to main, merge without approval

## Notes

- Rebase feature branches onto main before merge
- Tag releases with semantic versioning: `v1.2.3`
- Run tests locally before pushing
