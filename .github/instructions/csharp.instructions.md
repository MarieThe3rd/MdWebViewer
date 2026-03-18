---
description: "Use when writing, reviewing, or refactoring C# code in this project. Covers naming conventions, code structure, error handling, and .NET patterns for the Squirrel CLI app."
applyTo: "**/*.cs"
---

# C# Coding Standards — Squirrel

> Sourced from project conventions + [github/awesome-copilot](https://github.com/github/awesome-copilot) C# standards.

## Naming Conventions

- **Types and methods**: PascalCase — `ThoughtRepository`, `AddThought()`, `GetAllThoughts()`
- **Local variables and parameters**: camelCase — `thoughtId`, `newThought`, `filePath`
- **Private fields**: camelCase with underscore prefix — `_thoughts`, `_filePath`
- **Constants**: PascalCase — `DefaultStorageFileName`
- **Interfaces**: prefix with `I` — `IThoughtRepository`, `IStorageProvider`
- **Enums**: PascalCase for type and values — `ThoughtStatus.Promoted`
- **Async methods**: always suffix with `Async` — `SaveThoughtsAsync()`, `LoadThoughtsAsync()`

## Project Structure

- **Models** → `Models/` — pure data classes, no logic
- **Commands** → `Commands/` — one file per CLI command, one responsibility each
- **Storage** → `Storage/` — all file I/O and serialization logic
- **Interfaces** → `Interfaces/` — abstractions for testability

## Code Design Rules

- Follow project conventions first, then common C# conventions
- Don't add interfaces/abstractions unless needed for external dependencies or testing
- Don't wrap existing abstractions unnecessarily
- Least-exposure rule: `private` → `internal` → `protected` → `public`
- Comments explain **why**, not what
- Don't add unused methods or parameters
- Reuse existing methods as much as possible

## Code Style

- Use `var` when the type is obvious from the right-hand side
- Prefer expression-bodied members for simple one-liners
- Use `record` types for immutable models where appropriate
- Prefer `IEnumerable<T>` for return types over concrete collections
- Use pattern matching and switch expressions wherever possible
- Use `nameof` instead of string literals when referring to member names
- File-scoped namespace declarations and single-line using directives
- Always compile or check docs first if unfamiliar syntax — don't guess

## Nullable Reference Types

- Declare variables non-nullable; check for `null` at entry points
- Always use `is null` or `is not null` instead of `== null` or `!= null`
- Trust the C# null annotations — don't add null checks when the type system guarantees a value

## Async Programming

- Use `async/await` consistently — never `.Result` or `.Wait()`
- All async methods end with `Async`
- No fire-and-forget; always await
- Accept and pass `CancellationToken` through async call chains
- Use `ConfigureAwait(false)` in library/helper code
- No pointless wrappers — don't add `async/await` if you just return the task

## Command Handlers

- Each command handler does one thing only
- Delegate business logic to a service or repository — never inline in the handler
- Keep console output (`Console.WriteLine`) in the command handler, not in services
- Return `int` exit codes: `0` for success, non-zero for errors

## Error Handling

- Use `ArgumentNullException.ThrowIfNull()` for null checks — guard early
- Catch specific exceptions — `ArgumentException`, `InvalidOperationException`, etc. — never bare `Exception`
- No silent catches — don't swallow errors; log and rethrow or let them bubble
- Show friendly user-facing messages — this is a personal tool

## JSON Storage

- Use `System.Text.Json` — no third-party serializers
- Use `[JsonPropertyName]` attributes to control serialized names
- Pretty-print JSON when writing to file for human readability
- Stream large payloads; avoid loading entire file into memory unnecessarily

## Tests

- Use xUnit with `[Fact]` and `[Theory]` / `[InlineData]`
- Name tests: `MethodName_Scenario_ExpectedResult`
- Follow Arrange / Act / Assert with blank lines between sections
- One behavior per test; avoid branching/conditionals inside tests
- Cover happy path and at least two edge cases per method
- Test through public APIs — don't change visibility to enable testing
- Mock external dependencies only — never mock domain logic
