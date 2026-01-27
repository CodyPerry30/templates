# C# Standards

> **Scope**: All `.cs` files

## Naming

| Element | Convention | Example |
|---------|------------|---------|
| Classes/Records | PascalCase | `UserService`, `OrderDto` |
| Interfaces | IPascalCase | `IUserRepository` |
| Methods | PascalCase | `GetUserById`, `ValidateAsync` |
| Properties | PascalCase | `FirstName`, `IsActive` |
| Private fields | _camelCase | `_userRepository`, `_logger` |
| Parameters/Locals | camelCase | `userId`, `isValid` |
| Async methods | Suffix with Async | `SaveChangesAsync` |

## Formatting

- Allman brace style (braces on new line)
- 4 spaces indentation, no tabs
- One public type per file, filename matches type
- Max line length: 120 characters

## Patterns

```csharp
// ✅ Constructor injection
public class UserService
{
    private readonly IUserRepository _userRepository;
    
    public UserService(IUserRepository userRepository)
    {
        _userRepository = userRepository ?? throw new ArgumentNullException(nameof(userRepository));
    }
}

// ✅ Async with cancellation
public async Task<User?> GetUserAsync(int id, CancellationToken ct = default)
{
    return await _context.Users.FindAsync(id, ct);
}

// ✅ Records for DTOs
public record UserDto(int Id, string Name, string Email);

// ✅ Null handling
ArgumentNullException.ThrowIfNull(user);
var name = user?.Name ?? "Unknown";
```

## Boundaries

- ✅ Always: Enable nullable reference types, use async/await for I/O
- ⚠️ Ask first: Adding NuGet packages, changing dependency injection setup
- 🚫 Never: Use `dynamic`, catch bare `Exception` without rethrowing

## Notes

- Order usings: System → Microsoft → Third-party → Project
- Order members: constants → fields → constructors → properties → methods
- Use `var` when type is obvious, explicit type when not
