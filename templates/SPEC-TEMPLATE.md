# [Feature Name] Specification

> **Purpose:** [What this implements from the PRD, in one sentence]
> **PRD Reference:** [Link to parent PRD if applicable]

## Tech Stack

- **Language:** [e.g., TypeScript 5.4, Python 3.12, C# 12]
- **Framework:** [e.g., Next.js 14, FastAPI, ASP.NET Core]
- **Database:** [e.g., PostgreSQL 16, MongoDB 7, SQL Server]
- **Key dependencies:** [e.g., zod, pydantic, FluentValidation]

## Commands

```bash
# Development
[dev command]        # Start dev server
[build command]      # Production build

# Testing
[test command]       # Run tests (must pass before commits)
[test:cov command]   # With coverage report (target: >80%)

# Code quality
[lint command]       # Lint check
[typecheck command]  # Type validation
```

## Project Structure

```
src/
├── [folder]/        # [Description]
├── [folder]/        # [Description]
│   ├── [subfolder]/ # [Description]
│   └── [subfolder]/ # [Description]
├── [folder]/        # [Description]
└── [folder]/        # [Description]

tests/
├── unit/            # Unit tests
└── integration/     # Integration tests
```

## Functional Requirements

### FR-1: [Requirement Name]

**Description:** [What the system must do]
**Input:** [Expected input format/source]
**Output:** [Expected output format/destination]
**Validation:** [How to verify this works]

### FR-2: [Requirement Name]

**Description:** [What the system must do]
**Input:** [Expected input format/source]
**Output:** [Expected output format/destination]
**Validation:** [How to verify this works]

### FR-3: [Requirement Name]

**Description:** [What the system must do]
**Input:** [Expected input format/source]
**Output:** [Expected output format/destination]
**Validation:** [How to verify this works]

## Data Model

```
// Define entities relevant to this feature
// Use your language's type syntax or pseudocode

[EntityName] {
  id: [type]              // [Description]
  [field]: [type]         // [Description]
  [field]: [type]         // [Description]
  createdAt: [datetime]
  updatedAt: [datetime]
}
```

## API Contracts

```
// [METHOD] [endpoint]
Request: { [field]: [type] }
Response: { [field]: [type] }
Errors: 
  - [status]: [description]
  - [status]: [description]
```

```
// [METHOD] [endpoint]
Request: { [field]: [type] }
Response: { [field]: [type] }
Errors: 
  - [status]: [description]
```

## Code Style

```
// ✅ Preferred patterns - show a real example from your codebase

[Example code showing naming conventions, formatting, and patterns]
```

**Conventions:**

- [Naming convention for variables/functions]
- [Naming convention for types/classes]
- [Import/module organization preference]
- [Maximum function/method length]

## Boundaries

### ✅ Always Do

- Run tests before committing
- [Follow type safety rules for your language]
- Log errors to monitoring service
- Follow naming conventions shown above

### ⚠️ Ask First

- Database schema migrations
- Adding new dependencies
- Changes to CI/CD configuration
- Modifying authentication/authorization flow
- [Project-specific sensitive area]

### 🚫 Never Do

- Commit environment files or secrets
- Edit vendor/generated files
- Remove or skip failing tests
- Use debug logging in production code
- [Project-specific prohibition]
- [Project-specific prohibition]

## Success Criteria

- [ ] All tests pass with >[X]% coverage
- [ ] Code compiles/builds with zero errors
- [ ] Lint passes with zero warnings
- [ ] [Feature-specific criterion]: [Measurable outcome]
- [ ] [Feature-specific criterion]: [Measurable outcome]
- [ ] [Feature-specific criterion]: [Measurable outcome]

## Testing Requirements

**Unit tests:** [What to cover—e.g., service layer functions, business logic]
**Integration tests:** [What to cover—e.g., API endpoints, database operations]
**Test data:** [Location of fixtures/factories]

```
// Test naming convention example

[Test suite/class name] {
  [test case naming pattern] {
    // Arrange, Act, Assert pattern
  }
}
```

## Implementation Notes

[Any context, gotchas, or decisions the implementing agent/developer should know]

- [Known issue or workaround]
- [Architecture decision and rationale]
- [Performance consideration]
- [Reference to related specs or docs]

## Related Documents

- [Link to PRD]
- [Link to architecture docs]
- [Link to API documentation]
- [Link to related feature specs]

---

*Last updated: [Date] | Author: [Name]*
