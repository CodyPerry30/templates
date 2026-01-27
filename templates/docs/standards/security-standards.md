# Security Standards

> **Scope**: Auth, secrets, input validation, secure coding

## Secrets

```bash
# ✅ Use environment variables
DATABASE_URL=postgresql://...
API_KEY=${{ secrets.API_KEY }}

# ❌ Never hardcode
const apiKey = 'sk-1234567890'  # NEVER
```

- Store in environment variables or secrets manager
- Never commit `.env` files (use `.env.example`)
- Support rotation without redeployment

## Authentication

| Requirement | Standard |
|-------------|----------|
| Password hashing | bcrypt (cost ≥12) or Argon2id |
| Password length | Minimum 12 characters |
| Session cookies | HttpOnly, Secure, SameSite=Strict |
| Tokens | Bearer in Authorization header |
| Rate limiting | 5 attempts, then 15min lockout |

```typescript
// ✅ Regenerate session after login
await session.regenerate()
session.userId = user.id
```

## Input Validation

```typescript
// ✅ Allowlist approach
const ALLOWED_FIELDS = ['name', 'email', 'status']
if (!ALLOWED_FIELDS.includes(field)) throw new Error('Invalid field')

// ✅ Parameterized queries (prevent SQL injection)
db.query('SELECT * FROM users WHERE id = $1', [userId])

// ❌ Never concatenate user input into queries
db.query(`SELECT * FROM users WHERE id = ${userId}`)  // SQL INJECTION
```

## Output

- Encode output based on context (HTML, URL, JS)
- Implement Content Security Policy headers
- Never expose stack traces in production errors

## Checklist

Before code review:
- [ ] No hardcoded secrets
- [ ] All input validated server-side  
- [ ] Parameterized queries used
- [ ] Authorization checked on endpoints
- [ ] Sensitive data not logged

## Boundaries

- ✅ Always: Validate server-side, use parameterized queries, hash passwords
- ⚠️ Ask first: Auth flow changes, new external integrations
- 🚫 Never: Store plain text passwords, log tokens/passwords, trust client input

## Notes

- Client-side validation is UX only—never trust it
- Log auth failures with IP and timestamp
- Scan dependencies for vulnerabilities in CI
