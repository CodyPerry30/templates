# Testing Standards

> **Scope**: Unit tests, integration tests, E2E tests

## Structure

```
tests/
├── unit/              # Fast, isolated tests
│   └── services/
├── integration/       # Tests with real dependencies
│   └── api/
└── e2e/               # Full user flow tests
```

- Mirror source structure in test folders
- Use `.test.ts` or `.spec.ts` suffix
- Group tests by behavior, not by method

## Naming

```typescript
// ✅ Describe behavior in plain English
it('creates order when cart has items')
it('returns error when email is invalid')
it('sends notification after payment succeeds')

// ❌ Avoid
it('test1')
it('testCreateOrder')
it('should work')
```

## Patterns

```typescript
// ✅ Arrange-Act-Assert
it('calculates total with discount', () => {
    // Arrange
    const cart = new Cart([{ price: 100, qty: 2 }])
    const discount = new Discount(10)
    
    // Act
    const total = cart.calculate(discount)
    
    // Assert
    expect(total).toBe(180)
})

// ✅ Factories for test data
const user = UserFactory.create({ role: 'admin' })

// ✅ Mock at boundaries, not internals
const mockApi = jest.fn().mockResolvedValue({ data: [] })
```

## Coverage

| Code Type | Target |
|-----------|--------|
| Business logic | 90% |
| API endpoints | 80% |
| Utilities | 80% |
| Config/DTOs | 60% |

Cover error paths and edge cases, not just happy paths.

## Boundaries

- ✅ Always: Test one behavior per test, use factories, isolate tests
- ⚠️ Ask first: Skipping tests, reducing coverage thresholds
- 🚫 Never: Share mutable state between tests, test implementation details

## Notes

- Mock sparingly—prefer real implementations when fast
- Mock at system boundaries: HTTP, database, filesystem, time
- Integration tests should use transactions with rollback
- Run tests before every commit
