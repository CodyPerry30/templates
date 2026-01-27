# Project: [Name]

[One-sentence description of what this project does]

## Tech Stack

- **Backend**: [e.g., .NET 10.0, ASP.NET Core, EF Core, PostgreSQL 17]
- **Frontend**: [e.g., React 19, TypeScript 5.9, TanStack, Tailwind, Vite]
- **Infrastructure**: [e.g., Docker, Kubernetes, AWS/Azure/GCP]

## Commands

```bash
[build]        # Build the project
[test]         # Run all tests
[lint]         # Lint and format
[dev]          # Start development server
[db:migrate]   # Run database migrations
```

Run all commands from the project root unless specified.

## Architecture

[Claude maps out file structure and identifies architectural patterns]

## Code Style

| File Type | Standards |
|-----------|-----------|
| C# (`.cs`) | See `docs/csharp-standards.md` |
| TypeScript (`.ts`, `.tsx`) | See `docs/typescript-standards.md` |
| SQL, Migrations | See `docs/database-standards.md` |
| API Endpoints | See `docs/api-standards.md` |

## Boundaries

- ✅ Always: Run tests before commits, follow existing patterns, update docs
- ⚠️ Ask first: Dependencies, schema changes, architectural decisions, CI changes
- 🚫 Never: Commit secrets, edit generated files, skip tests, use `any` in TS

## Workflows

### Adding a Feature

1. Create branch: `feature/TICKET-123-description`
2. Read relevant standards docs
3. Implement with tests (≥80% coverage)
4. Create PR with ticket link

### Database Changes

1. Read `docs/database-standards.md`
2. Create versioned migration
3. Test up and down migrations locally

## Notes

- [Critical project-specific gotcha or warning]
- [Important context about the codebase]
- See `docs/adr/` for architectural decisions
