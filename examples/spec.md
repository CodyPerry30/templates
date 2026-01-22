# Smart Task Search Specification

> **Purpose:** Implements the advanced search functionality defined in the Smart Task Search PRD, enabling natural language queries, advanced filters, and saved searches.
> **PRD Reference:** [Smart Task Search PRD](./PRD-EXAMPLE.md)

## Tech Stack

- **Language:** TypeScript 5.4
- **Framework:** Next.js 14 (App Router), React 18
- **Database:** PostgreSQL 16 (primary), Elasticsearch 8.11 (search index)
- **Key dependencies:** zod (validation), @tanstack/react-query (data fetching), cmdk (command palette UI), date-fns (date parsing)

## Commands

```bash
# Development
pnpm dev             # Start dev server at localhost:3000
pnpm build           # Production build

# Testing
pnpm test            # Run tests (must pass before commits)
pnpm test:cov        # With coverage report (target: >80%)

# Code quality
pnpm lint            # ESLint check
pnpm typecheck       # TypeScript validation
pnpm search:reindex  # Rebuild Elasticsearch index (dev only)
```

## Project Structure

```
src/
├── app/                    # Next.js app router pages
│   └── api/search/         # Search API routes
├── components/             # React components
│   └── search/             # Search-specific components
│       ├── SearchModal.tsx
│       ├── FilterBuilder.tsx
│       └── SearchResults.tsx
├── lib/                    # Core business logic
│   ├── search/             # Search service layer
│   │   ├── parser.ts       # Natural language query parser
│   │   ├── indexer.ts      # Elasticsearch indexing
│   │   └── ranking.ts      # Result ranking algorithms
│   └── db/                 # Database utilities
├── types/                  # TypeScript type definitions
└── utils/                  # Shared utilities

tests/
├── unit/                   # Unit tests (parser, ranking logic)
└── integration/            # API and database integration tests
```

## Functional Requirements

### FR-1: Natural Language Query Parsing

**Description:** Parse user queries written in natural language into structured search parameters. Handle typos, synonyms, and implicit filters (e.g., "my tasks" → assignee:current_user).
**Input:** Raw query string from user (e.g., "bugs assigned to Sarah last week")
**Output:** Structured `ParsedQuery` object with extracted filters, keywords, and confidence scores
**Validation:** Parser correctly extracts filters from 95%+ of test query corpus; falls back to literal search on low confidence

### FR-2: Elasticsearch Query Execution

**Description:** Execute parsed queries against Elasticsearch index with relevance ranking, respecting user's project access permissions.
**Input:** `ParsedQuery` object + user context (ID, accessible project IDs)
**Output:** Ranked array of `SearchResult` objects with highlights, max 50 results per page
**Validation:** p95 response time <500ms; results respect permission boundaries (verified via integration tests)

### FR-3: Saved Search Management

**Description:** Allow users to save, name, update, and delete search queries. Support sharing saved searches with team members.
**Input:** Search query + metadata (name, visibility: personal|team)
**Output:** Persisted `SavedSearch` record; shareable URL for team-visible searches
**Validation:** CRUD operations complete successfully; shared searches only accessible to users with project access

## Data Model

```typescript
// Core search entities

SearchQuery {
  raw: string                    // Original user input
  keywords: string[]             // Extracted search terms
  filters: SearchFilter[]        // Structured filters
  confidence: number             // Parser confidence 0-1
  parsedAt: DateTime
}

SearchFilter {
  field: FilterField             // 'assignee' | 'status' | 'priority' | 'project' | 'label' | 'date'
  operator: FilterOperator       // 'eq' | 'neq' | 'gt' | 'lt' | 'range' | 'in'
  value: string | string[] | DateRange
}

SavedSearch {
  id: UUID                       // Primary key
  userId: UUID                   // Owner
  name: string                   // User-provided name (max 100 chars)
  query: SearchQuery             // The saved query
  visibility: 'personal' | 'team'
  projectId: UUID | null         // Required if visibility = 'team'
  usageCount: number             // Analytics: times executed
  createdAt: DateTime
  updatedAt: DateTime
}

SearchResult {
  taskId: UUID
  title: string
  titleHighlight: string         // With  tags for matches
  projectName: string
  assigneeName: string | null
  status: TaskStatus
  score: number                  // Relevance score
  updatedAt: DateTime
}
```

## API Contracts

```typescript
// POST /api/search
// Execute a search query

Request: {
  query: string                  // Raw search query (max 500 chars)
  page?: number                  // Default: 1
  pageSize?: number              // Default: 20, max: 50
}

Response: {
  results: SearchResult[]
  total: number
  page: number
  pageSize: number
  queryParsed: SearchQuery       // For debugging/transparency
  durationMs: number
}

Errors:
  - 400: Invalid query (empty or exceeds max length)
  - 429: Rate limit exceeded (max 60 requests/minute)
  - 500: Search service unavailable
```

```typescript
// POST /api/search/saved
// Create a saved search

Request: {
  name: string                   // 1-100 characters
  query: string                  // The query to save
  visibility: 'personal' | 'team'
  projectId?: string             // Required if visibility = 'team'
}

Response: {
  id: string
  name: string
  shareUrl: string | null        // Present if visibility = 'team'
  createdAt: string
}

Errors:
  - 400: Validation error (name too long, missing projectId for team)
  - 403: User lacks access to specified project
  - 409: Saved search with this name already exists
```

```typescript
// GET /api/search/saved
// List user's saved searches

Request: {
  includeTeam?: boolean          // Include team-shared searches (default: true)
}

Response: {
  searches: Array
}

Errors:
  - 401: Not authenticated
```

## Code Style

```typescript
// ✅ Preferred patterns

// Use explicit return types on exported functions
export async function executeSearch(
  query: string,
  userId: string,
  options?: SearchOptions
): Promise {
  // Validate input at boundaries
  const validated = searchQuerySchema.parse({ query, ...options });
  
  // Early returns for edge cases
  if (!validated.query.trim()) {
    return { results: [], total: 0, page: 1, pageSize: 20, durationMs: 0 };
  }
  
  // Descriptive variable names
  const parsedQuery = await parseNaturalLanguageQuery(validated.query);
  const accessibleProjectIds = await getUserProjectIds(userId);
  
  // Single responsibility: this function orchestrates, doesn't implement
  const results = await searchIndex.execute(parsedQuery, accessibleProjectIds);
  
  return formatSearchResponse(results, validated);
}

// Colocate types with their primary usage
interface SearchOptions {
  page?: number;
  pageSize?: number;
}
```

**Conventions:**

- Variables/functions: camelCase (`parseQuery`, `searchResults`)
- Types/interfaces: PascalCase (`SearchResult`, `ParsedQuery`)
- Files: kebab-case (`search-parser.ts`, `query-builder.ts`)
- Imports: group by external → internal → relative, alphabetized within groups
- Functions: max 40 lines; extract helpers for complex logic

## Boundaries

### ✅ Always Do

- Run `pnpm test && pnpm typecheck` before committing
- Validate all user input with zod schemas at API boundaries
- Log errors to Sentry with relevant context (user ID, query, duration)
- Include `data-testid` attributes on interactive search UI elements
- Use parameterized queries for all database operations

### ⚠️ Ask First

- Elasticsearch index mapping changes (requires reindex)
- Adding new npm dependencies (check bundle size impact)
- Modifying the query parser grammar (affects all users)
- Changes to search ranking algorithm weights
- Any changes to permission/access control logic

### 🚫 Never Do

- Commit `.env` files or Elasticsearch credentials
- Log raw user queries to application logs (PII concern)
- Bypass permission checks for "performance reasons"
- Use `any` type without explicit justification comment
- Execute raw SQL or Elasticsearch queries without parameterization
- Disable TypeScript strict mode or ESLint rules

## Success Criteria

- [ ] All tests pass with >85% line coverage on `lib/search/*`
- [ ] TypeScript compiles with zero errors, strict mode enabled
- [ ] ESLint passes with zero warnings
- [ ] Search response p95 latency <500ms (measured in staging with production data volume)
- [ ] Natural language parser achieves >90% accuracy on test corpus of 500 real user queries
- [ ] Saved searches correctly enforce project-level access permissions (security review passed)
- [ ] Lighthouse accessibility score >95 for search modal

## Testing Requirements

**Unit tests:** Query parser logic, filter extraction, ranking algorithm, input validation schemas. Mock Elasticsearch client.

**Integration tests:** Full search flow from API to Elasticsearch and back, saved search CRUD operations, permission boundary enforcement.

**Test data:** Fixtures in `tests/fixtures/`, factory functions in `tests/factories/`. Use `createMockTask()`, `createMockUser()`, `createMockProject()`.

```typescript
// Test naming convention

describe('parseNaturalLanguageQuery', () => {
  it('extracts assignee filter from "assigned to {name}" pattern', async () => {
    // Arrange
    const query = 'bugs assigned to Sarah';
    
    // Act
    const result = await parseNaturalLanguageQuery(query);
    
    // Assert
    expect(result.filters).toContainEqual({
      field: 'assignee',
      operator: 'eq',
      value: 'sarah', // normalized
    });
    expect(result.keywords).toEqual(['bugs']);
  });

  it('falls back to keyword search when confidence is low', async () => {
    // Arrange
    const ambiguousQuery = 'the thing from last meeting';
    
    // Act
    const result = await parseNaturalLanguageQuery(ambiguousQuery);
    
    // Assert
    expect(result.confidence).toBeLessThan(0.5);
    expect(result.keywords).toEqual(['thing', 'last', 'meeting']);
    expect(result.filters).toHaveLength(0);
  });
});
```

## Implementation Notes

- **Elasticsearch index refresh:** Default refresh interval is 1s, which is acceptable for task updates. For bulk imports, temporarily set to 30s to reduce load.

- **Query parser architecture:** Using a rule-based parser (not ML) for V1 to ensure predictability and debuggability. Rules are defined in `lib/search/parser-rules.ts` and can be extended without retraining.

- **Permission caching:** User's accessible project IDs are cached in Redis for 5 minutes. Cache key: `user:{userId}:projects`. Invalidate on project membership changes.

- **Rate limiting:** Implemented at API gateway level (60 req/min per user). Search-specific rate limit headers included in response: `X-RateLimit-Remaining`, `X-RateLimit-Reset`.

- **Keyboard shortcuts:** Using cmdk library for the command palette. Ensure shortcuts don't conflict with browser defaults. Cmd+K opens search; Escape closes; Arrow keys navigate; Enter selects.

- **Known limitation:** Elasticsearch highlight fragments may split words awkwardly. Post-process to extend fragments to word boundaries in `formatSearchResults()`.

## Related Documents

- [Smart Task Search PRD](./PRD-EXAMPLE.md)
- [Elasticsearch Cluster Architecture](../infrastructure/elasticsearch-setup.md)
- [Task Data Model Reference](../data-models/tasks.md)
- [API Authentication Guide](../api/authentication.md)
- [Accessibility Standards Checklist](../design/a11y-checklist.md)

---

*Last updated: January 6, 2025 | Author: Alex Rivera*