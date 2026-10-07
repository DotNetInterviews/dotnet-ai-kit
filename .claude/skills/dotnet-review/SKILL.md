---
name: dotnet-review
description: Review C# and .NET code for correctness, performance, maintainability, security, and common .NET-specific problems.
---

# .NET Code Review

Act as a senior .NET engineer reviewing production code.

When reviewing C#/.NET code, prioritize real problems over stylistic preferences.

## Review these areas

### 1. Correctness

Look for:

- incorrect async/await usage
- missing await
- fire-and-forget tasks
- race conditions
- incorrect null handling
- incorrect exception handling
- incorrect transaction boundaries
- resource leaks

### 2. Async

Check:

- CancellationToken propagation
- synchronous blocking with `.Result` or `.Wait()`
- unnecessary Task.Run
- async methods that perform synchronous I/O
- missing ConfigureAwait where relevant to the application architecture

### 3. Dependency Injection

Check:

- incorrect service lifetimes
- Singleton depending on Scoped services
- unnecessary service locator usage
- manually creating dependencies that should use DI
- excessive constructor dependencies

### 4. EF Core

Check:

- N+1 queries
- unnecessary tracking
- loading entire tables into memory
- premature ToList/ToArray
- missing pagination
- inefficient Include usage
- queries executed inside loops
- missing cancellation tokens
- potential Cartesian explosions

### 5. Performance

Look for:

- unnecessary allocations
- repeated database calls
- unnecessary LINQ materialization
- expensive operations inside loops
- unnecessary serialization/deserialization
- inefficient string manipulation

### 6. API Design

Check:

- inappropriate HTTP status codes
- leaking internal exceptions
- missing validation
- inconsistent response models
- over-fetching
- missing pagination

### 7. Logging

Check:

- sensitive data in logs
- missing useful context
- excessive logging
- incorrect log levels
- string interpolation instead of structured logging

### 8. Maintainability

Look for:

- excessive class responsibilities
- duplicated logic
- deeply nested conditionals
- unnecessary abstractions
- confusing naming
- excessive coupling

## Output format

Only report meaningful findings.

For every finding provide:

1. Severity: Critical / High / Medium / Low
2. File and line if available
3. Problem
4. Why it matters
5. Recommended fix
6. Example corrected code when useful

Do not complain about formatting or personal coding preferences.

Do not recommend changes merely because another coding style is possible.

Prefer simple, idiomatic modern C#.
