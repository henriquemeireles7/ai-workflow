# AGENTS.md Template

> Copy this file to your project root as `AGENTS.md` and customize it.
> This is the operational brief that Claude reads at the start of every autonomous loop.
> Keep it under 80 lines - this is prime context window real estate.

---

# [YOUR PROJECT NAME]

## What We're Building
[1-2 sentences: what this project does and who it's for]

## Tech Stack
- **Language**: [e.g., TypeScript, Python]
- **Framework**: [e.g., Next.js, FastAPI, Hono]
- **Database**: [e.g., PostgreSQL, SQLite]
- **Testing**: [e.g., Vitest, pytest, Jest]
- **Package Manager**: [e.g., pnpm, npm, uv]

## Project Structure
```
src/
├── [folder]/     # [what it contains]
├── [folder]/     # [what it contains]
└── [folder]/     # [what it contains]
tests/
└── [structure]   # [test organization]
```

## Commands
```bash
[pkg] install          # Install dependencies
[pkg] run dev          # Start dev server
[pkg] run test         # Run tests
[pkg] run lint         # Lint
[pkg] run build        # Build
[pkg] run validate     # Full check: lint + test + build
```

## Code Style
- [Key convention 1, e.g., "TypeScript strict mode, no `any`"]
- [Key convention 2, e.g., "Functions under 50 lines"]
- [Key convention 3, e.g., "Conventional commits: feat:, fix:, etc."]

## Architecture Rules
- [Rule 1, e.g., "Handlers call services, services call repositories"]
- [Rule 2, e.g., "No cross-feature imports"]
- [Rule 3, e.g., "All user input validated with Zod"]

## Testing Rules
- Tests first (TDD): write failing test, then implement
- Fix the implementation, never the test (unless test is wrong)
- Minimum [X]% coverage

## Current Status
- [What's done]
- [What's in progress]
- [What's next]

## Important Notes
- [Constraint 1, e.g., "Don't add dependencies without discussing"]
- [Constraint 2, e.g., "All API responses must follow the standard envelope"]
- [Gotcha, e.g., "The auth middleware must be applied before route handlers"]
