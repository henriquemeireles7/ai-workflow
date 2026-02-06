# Project: [YOUR PROJECT NAME]

> **CUSTOMIZE**: Replace this file with your project's details.
> This file is read by Claude Code at the start of every session.
> For autonomous loop mode, also create AGENTS.md at root (see templates/AGENTS.md).

## Overview

[Brief description of what this project does]

## Tech Stack

- **Frontend**: [e.g., Next.js 15, React 19, Tailwind CSS]
- **Backend**: [e.g., Node.js, Express, FastAPI]
- **Database**: [e.g., PostgreSQL via Supabase, Prisma ORM]
- **Auth**: [e.g., Auth.js, Clerk, Supabase Auth]
- **Deployment**: [e.g., Vercel, Railway, AWS]
- **Testing**: [e.g., Vitest, Playwright, Jest]

## Project Structure

```
src/
├── app/           # [Describe]
├── components/    # [Describe]
├── features/      # [Describe]
├── lib/           # [Describe]
└── types/         # [Describe]
```

## Key Commands

```bash
npm run dev          # Start development server
npm run build        # Production build
npm run test         # Run tests
npm run lint         # Run linter
npm run validate     # Full validation: lint + test + build
```

## Development Modes

### Interactive Mode (Cursor / Claude Code session)
Standard agentic development with human in the loop:

1. **Plan** - Use `planner` agent for features >1 file
2. **TDD** - Write tests first, then implement (use `tdd-guide` agent)
3. **Review** - Use `code-reviewer` agent after changes
4. **Security** - Use `security-reviewer` for auth/API/input code
5. **Commit** - Follow conventional commits (`feat:`, `fix:`, etc.)

### Autonomous Mode (loop.sh)
Headless Claude Code running in a bash loop:

```bash
./loop.sh plan        # Generate IMPLEMENTATION_PLAN.md from specs/
./loop.sh build       # Execute tasks from IMPLEMENTATION_PLAN.md
./loop.sh plan-work   # Hybrid: plan one task, implement, repeat
```

Key files for autonomous mode:
- `AGENTS.md` - Operational brief (always read first)
- `specs/*.md` - Requirements and feature specs
- `prompts/PROMPT_plan.md` - Planning prompt template
- `prompts/PROMPT_build.md` - Build prompt template
- `IMPLEMENTATION_PLAN.md` - Generated task list (source of truth)

## Agents & Rules

See `agents/` for subagent definitions and `rules/` for coding guidelines:

- `rules/coding-style.md` - Code conventions
- `rules/testing.md` - TDD workflow
- `rules/security.md` - Security checks
- `rules/git-workflow.md` - Git process
- `rules/performance.md` - Model selection
- `rules/agents.md` - When to use which agent

## Important Notes

- [Add project-specific notes, constraints, or conventions]
- [Add known issues or workarounds]
- [Add deployment-specific instructions]
