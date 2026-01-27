# TypeScript Standards

> **Scope**: All `.ts`, `.tsx` files

## Naming

| Element | Convention | Example |
|---------|------------|---------|
| Variables/Functions | camelCase | `userName`, `getUserById` |
| Types/Interfaces | PascalCase | `User`, `ApiResponse` |
| Components | PascalCase | `UserProfile`, `OrderList` |
| Constants | UPPER_SNAKE_CASE | `MAX_RETRIES`, `API_URL` |
| Files | kebab-case | `user-service.ts`, `use-auth.ts` |
| Hooks | use prefix | `useAuth`, `useLocalStorage` |

## Formatting

- Single quotes for strings, backticks for interpolation
- No semicolons (or always semicolons—be consistent)
- Trailing commas in multiline structures
- 2 spaces indentation

## Patterns

```typescript
// ✅ Explicit return types on exports
export function getUser(id: number): Promise<User | null> { }

// ✅ Props interface with destructuring
interface UserCardProps {
    user: User;
    onSelect?: (user: User) => void;
}

export function UserCard({ user, onSelect }: UserCardProps) {
    return <div onClick={() => onSelect?.(user)}>{user.name}</div>
}

// ✅ Custom hooks
export function useAuth() {
    const [user, setUser] = useState<User | null>(null)
    // ...
    return { user, login, logout }
}

// ✅ Async/await over .then()
const data = await fetchData()
```

## Boundaries

- ✅ Always: Use strict mode, handle null/undefined explicitly
- ⚠️ Ask first: Adding npm packages, changing tsconfig
- 🚫 Never: Use `any`, disable TypeScript rules, use class components

## Notes

- Import order: external → internal (@/) → relative → styles
- Prefer named exports; default exports only for pages
- Use `unknown` instead of `any` for truly unknown types
- Memoize with `useMemo`/`useCallback` when needed for performance
