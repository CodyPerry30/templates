# Documentation Standards

> **Scope**: Code comments, READMEs, ADRs, API docs

## Code Comments

```typescript
// ✅ Explain WHY, not what
// Rate limited per SEC-2024-03 compliance
const limiter = new RateLimiter({ max: 100 })

// ✅ TODO with ticket and owner
// TODO(AUTH-456): Replace after OAuth migration - @jsmith

// ❌ Don't state the obvious
// Increment counter
counter++
```

## README Structure

```markdown
# Project Name
Brief description.

## Installation
npm install

## Usage
Quick example

## Configuration
| Variable | Description | Required |
|----------|-------------|----------|

## Development
npm run dev

## License
MIT
```

## ADRs (Architecture Decision Records)

```markdown
# ADR-0001: Use PostgreSQL

## Status
Accepted

## Context
We need a relational database with JSON support.

## Decision
Use PostgreSQL 17.

## Consequences
- ✅ Strong JSON support, good tooling
- ⚠️ Requires ops expertise for HA
```

Store in `docs/adr/` with sequential numbering.

## API Documentation

- Maintain OpenAPI 3.0+ spec
- Document all endpoints with examples
- Include error responses
- Keep spec in sync with implementation

```yaml
/users/{id}:
  get:
    summary: Get user by ID
    responses:
      '200':
        description: User found
      '404':
        description: Not found
```

## Boundaries

- ✅ Always: Update docs with code changes, include examples
- ⚠️ Ask first: Major doc restructuring, changing doc tooling
- 🚫 Never: Leave outdated docs, document implementation details that change

## Notes

- Write ADRs for architectural decisions
- Keep changelog in CHANGELOG.md (Keep a Changelog format)
- Deprecate with `@deprecated` and removal version
