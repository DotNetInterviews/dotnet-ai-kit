# dotnet-ai-kit

Reusable AI skills for senior-level .NET development.

Install the skills into your project's .claude/skills directory and use them with Claude Code or GitHub Copilot.

# dotnet-ai-kit

> Production-grade AI skills for .NET developers.

`dotnet-ai-kit` is a collection of reusable AI skills designed to help .NET developers write, review, debug, optimize, and architect production-quality software.

The skills focus on **engineering judgment**, not code-style nitpicking.

Works with:

- GitHub Copilot
- Claude Code

---

## ✨ Why dotnet-ai-kit?

AI can generate C# code very quickly.

The harder problem is knowing whether that code is actually **good production code**.

`dotnet-ai-kit` gives AI a set of focused engineering skills for common .NET development tasks.

Instead of asking:

> "Is this code good?"

you can give your AI agent a specialized skill that knows what to look for.

For example:

> "Review this code using the .NET review skill."

The skill will look for real problems such as:

- Async/await mistakes
- Race conditions
- Incorrect dependency injection lifetimes
- EF Core performance issues
- N+1 queries
- Database transaction problems
- Resource leaks
- API design problems
- Security vulnerabilities
- Logging problems
- Scalability issues
- Maintainability problems

It intentionally avoids generating long lists of subjective style recommendations.

---

# 🚀 Installation

## GitHub Copilot

Install a skill into your current project:

```bash
gh skill install https://github.com/DotNetInterviews/dotnet-ai-kit dotnet-review \
  --agent copilot \
  --scope project
```

The skill will be installed into the project's Copilot skill directory.

Typically:

```text
.github/
└── skills/
    └── dotnet-review/
        └── SKILL.md
```

You can then ask Copilot:

```text
Review this code using the dotnet-review skill.
```

Or explicitly invoke the skill:

```text
/dotnet-review
```

> `gh skill` is currently a GitHub CLI feature in public preview. Make sure your GitHub CLI is up to date.

---

## Claude Code

Install the skill for Claude Code:

```bash
gh skill install YOUR_USERNAME/dotnet-ai-kit dotnet-review \
  --agent claude-code \
  --scope project
```

The skill will be installed into the Claude Code skill directory:

```text
.claude/
└── skills/
    └── dotnet-review/
        └── SKILL.md
```

You can then ask Claude:

```text
Review this code using the dotnet-review skill.
```

Or invoke it explicitly:

```text
/dotnet-review
```

---

# 🧰 Available Skills

| Skill | Description | Status |
|---|---|---|
| `dotnet-review` | Review C#/.NET code for correctness, performance, security and maintainability | ✅ Available |
| `ef-core-review` | Review EF Core queries, tracking, relationships and database performance | 🚧 Planned |
| `dotnet-debug` | Systematically diagnose .NET bugs and production failures | 🚧 Planned |
| `dotnet-performance` | Identify .NET application performance and scalability problems | 🚧 Planned |
| `api-review` | Review ASP.NET Core APIs for correctness, security and API design | 🚧 Planned |
| `dotnet-architecture` | Review .NET architecture, boundaries, dependencies and scalability | 🚧 Planned |

More skills will be added over time.

---

# 🔍 dotnet-review

The first skill in `dotnet-ai-kit` is `dotnet-review`.

It acts as a senior .NET engineer reviewing production code.

## What it reviews

### Correctness

Looks for:

- Incorrect `async`/`await` usage
- Missing `await`
- Fire-and-forget tasks
- Race conditions
- Incorrect null handling
- Incorrect exception handling
- Incorrect transaction boundaries
- Resource leaks

### Async

Checks:

- `CancellationToken` propagation
- `.Result` / `.Wait()` blocking
- Unnecessary `Task.Run`
- Synchronous I/O inside async methods
- Incorrect async abstractions
- `ConfigureAwait` where relevant

### Dependency Injection

Checks:

- Incorrect service lifetimes
- Singleton → Scoped dependency problems
- Service locator usage
- Manually constructed dependencies
- Excessive constructor dependencies
- Incorrect dependency boundaries

### EF Core

Looks for:

- N+1 queries
- Unnecessary tracking
- Loading large datasets into memory
- Premature `ToList()` / `ToArray()`
- Missing pagination
- Inefficient `Include`
- Queries inside loops
- Missing cancellation tokens
- Cartesian explosions
- Inefficient projections

### Performance

Looks for:

- Unnecessary allocations
- Repeated database calls
- Expensive operations inside loops
- Unnecessary LINQ materialization
- Excessive serialization/deserialization
- Inefficient string operations

### API Design

Checks:

- Incorrect HTTP status codes
- Internal exception leakage
- Missing validation
- Inconsistent response models
- Over-fetching
- Missing pagination
- Incorrect API error handling

### Logging

Checks:

- Sensitive information in logs
- Missing useful context
- Excessive logging
- Incorrect log levels
- String interpolation instead of structured logging

### Maintainability

Looks for:

- Excessive class responsibilities
- Duplicated logic
- Deeply nested conditionals
- Unnecessary abstractions
- Confusing naming
- Excessive coupling

### Production Failure Modes

Considers what happens under:

- High concurrency
- Database/network latency
- Partial failures
- Retries
- Process restarts
- Duplicate messages
- Timeouts
- Cancellation
- Large datasets
- Dependency outages

Particular attention is given to:

- Race conditions
- Duplicate processing
- Lost updates
- Non-idempotent operations
- Retry amplification
- Unbounded memory growth
- Unbounded queues
- Inconsistent state
- Operations that cannot safely resume after failure

---

# 📋 Review Output

The review intentionally reports **only meaningful findings**.

Each finding contains:

```text
Severity
File and line
Problem
Why it matters
Recommended fix
Corrected code (when useful)
```

Example:

```text
[HIGH] Race condition during order creation

Location:
OrderService.cs:42-47

Problem:
The code checks whether an order exists and then inserts it
as two independent database operations.

Why it matters:
Two concurrent requests can both observe that the order does
not exist and create duplicate records.

Recommended fix:
Enforce uniqueness at the database level and handle the
resulting conflict appropriately.
```

The goal is to identify **real engineering problems**, not produce a list of stylistic preferences.

---

# 🧠 Design Principles

`dotnet-ai-kit` follows a few principles.

### 1. Find real problems

Don't complain about code merely because another implementation is possible.

### 2. Prefer simple solutions

Prefer straightforward, idiomatic modern C# over unnecessary abstractions.

### 3. Think about production

Consider:

```text
Concurrency
Failure
Scale
Security
Performance
Observability
Maintainability
```

### 4. Explain the "why"

A useful review shouldn't just say:

> "Change this."

It should explain:

> "This can cause duplicate processing when two requests execute concurrently."

### 5. Don't nitpick

Formatting, personal preferences, naming preferences, and subjective style choices should not dominate the review.

---

# 📁 Repository Structure

```text
dotnet-ai-kit/
│
├── skills/
│   ├── dotnet-review/
│   │   └── SKILL.md
│   │
│   ├── ef-core-review/
│   │   └── SKILL.md
│   │
│   ├── dotnet-debug/
│   │   └── SKILL.md
│   │
│   └── ...
│
├── README.md
└── LICENSE
```

The skills themselves are kept independent from the AI agent.

The installation mechanism determines where they are placed:

```text
GitHub Copilot
    ↓
.github/skills/

Claude Code
    ↓
.claude/skills/
```

This allows the same skill to be used across different AI coding agents.

---

# 🛠️ Adding a New Skill

A skill is simply a directory containing a `SKILL.md` file.

Example:

```text
skills/
└── ef-core-review/
    └── SKILL.md
```

A skill should contain:

1. A clear name
2. A concise description
3. Instructions for the AI agent
4. Specific review/reasoning criteria
5. Expected output format
6. Rules to avoid unnecessary findings

Keep skills focused.

Prefer:

```text
ef-core-review
```

over:

```text
everything-about-dotnet
```

---

# 🤝 Contributing

Contributions are welcome.

Good contributions include:

- New .NET skills
- Improvements to existing skills
- Real-world production failure scenarios
- Better review heuristics
- Examples of problematic code
- Improvements to installation/documentation

When adding a skill, prioritize **engineering usefulness over completeness**.

A smaller skill that consistently identifies important problems is better than a huge skill that produces noisy reviews.

---

# 📜 License

MIT License

See [LICENSE](LICENSE) for details.

---

# ⭐ Support the Project

If you find `dotnet-ai-kit` useful, consider giving the repository a ⭐.

Have an idea for a .NET AI skill?

Open an issue or submit a pull request.

---

## Roadmap

The long-term goal is to build a practical AI engineering toolkit for .NET developers covering:

```text
Code Review
     ↓
Debugging
     ↓
Performance
     ↓
EF Core
     ↓
API Design
     ↓
Architecture
     ↓
Testing
     ↓
Security
     ↓
Distributed Systems
     ↓
Production Engineering
```

The goal isn't to make AI generate more code.

The goal is to make AI behave more like a **strong senior .NET engineer**.