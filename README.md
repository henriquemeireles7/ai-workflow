# AI Workflow Template

A battle-tested template for **agentic AI development** -- the center of truth for coding with AI agents (Claude Code, Cursor, Copilot, etc.).

This template provides a complete set of **agents**, **rules**, **skills**, and **templates** that you copy into your project to get structured, high-quality AI-assisted development from day one.

---

## What's Inside

```
ai-workflow/
├── agents/                    # Subagent definitions
│   ├── planner.md             # Feature planning specialist
│   ├── architect.md           # System design specialist
│   ├── tdd-guide.md           # Test-driven development enforcer
│   ├── code-reviewer.md       # Code quality & security reviewer
│   ├── security-reviewer.md   # OWASP Top 10 & vulnerability scanner
│   ├── build-error-resolver.md # Build/type error fixer
│   ├── e2e-runner.md          # Playwright E2E testing specialist
│   ├── database-reviewer.md   # PostgreSQL optimization specialist
│   ├── refactor-cleaner.md    # Dead code & dependency cleaner
│   └── doc-updater.md         # Documentation & codemap maintainer
│
├── rules/                     # AI behavior guidelines
│   ├── agents.md              # When to delegate to which agent
│   ├── coding-style.md        # Code conventions & anti-patterns
│   ├── git-workflow.md        # Commit format, branching, PR process
│   ├── patterns.md            # Common code patterns & templates
│   ├── performance.md         # Model selection & context management
│   ├── security.md            # Mandatory security checks
│   └── testing.md             # TDD workflow & coverage requirements
│
├── skills/                    # Reusable knowledge modules
│   ├── continuous-learning/   # Self-improving pattern detection
│   └── prisma/                # Prisma ORM best practices
│
├── templates/                 # Document templates
│   ├── HANDOFF.md             # Session handoff document
│   ├── ADR.md                 # Architecture Decision Record
│   └── PROJECT-TASKS.md       # Task tracking template
│
├── CLAUDE.md                  # Template for Claude Code users
├── .cursorrules               # Template for Cursor IDE users
├── SETUP.md                   # Step-by-step customization guide
│
└── everything-claude-code/    # Reference: advanced agentic patterns
    ├── the-shortform-guide.md # Setup guide for Claude Code
    └── the-longform-guide.md  # Advanced techniques & optimization
```

## Quick Start

### 1. Clone this template

```bash
git clone https://github.com/YOUR_USERNAME/ai-workflow.git
cd ai-workflow
```

### 2. Copy into your project

```bash
# Copy the workflow files into your project
cp -r agents/ /path/to/your-project/.ai/agents/
cp -r rules/ /path/to/your-project/.ai/rules/
cp -r skills/ /path/to/your-project/.ai/skills/
cp -r templates/ /path/to/your-project/.ai/templates/

# For Claude Code users
cp CLAUDE.md /path/to/your-project/CLAUDE.md

# For Cursor users
cp .cursorrules /path/to/your-project/.cursorrules
```

### 3. Customize

See [SETUP.md](./SETUP.md) for detailed customization instructions.

---

## Core Concepts

### Agents

Agents are specialized AI assistants with focused responsibilities. Instead of asking one AI to do everything, you delegate specific tasks to purpose-built agents:

| Agent | Purpose | When to Use |
|-------|---------|-------------|
| **planner** | Break down features into steps | Before starting any multi-file feature |
| **architect** | System design decisions | New services, major refactors |
| **tdd-guide** | Enforce test-first development | Every new feature or bug fix |
| **code-reviewer** | Quality + security review | After writing any code |
| **security-reviewer** | Vulnerability detection | Auth, API, user input code |
| **build-error-resolver** | Fix build/type errors | When builds fail |
| **database-reviewer** | SQL/schema optimization | Schema changes, slow queries |
| **e2e-runner** | E2E test management | Critical user flow testing |
| **refactor-cleaner** | Dead code removal | Regular maintenance |
| **doc-updater** | Documentation sync | After features land |

### Rules

Rules define the AI's behavior and coding standards. They're loaded automatically by the AI tool and enforce consistent practices across sessions:

- **coding-style.md** - Naming conventions, file organization, anti-patterns
- **testing.md** - TDD workflow, 80% coverage requirement
- **security.md** - Mandatory security checks before every commit
- **git-workflow.md** - Conventional commits, PR process
- **performance.md** - Model selection, context window management

### Skills

Skills are reusable knowledge modules that encode patterns and best practices:

- **continuous-learning** - Detects patterns from corrections and saves them
- **prisma** - ORM best practices, schema design, query optimization
- Add your own! (backend-patterns, frontend-patterns, etc.)

### Templates

Pre-built document templates for common development workflows:

- **HANDOFF.md** - Session handoff when hitting context limits
- **ADR.md** - Architecture Decision Records for important choices
- **PROJECT-TASKS.md** - Task tracking with TDD workflow

---

## Recommended Workflow

### For New Features

```
1. planner     -> Create implementation plan
2. tdd-guide   -> Write tests first (RED)
3. [implement] -> Make tests pass (GREEN)
4. [refactor]  -> Clean up (IMPROVE)
5. code-reviewer -> Review code quality
6. security-reviewer -> Check security (if applicable)
7. doc-updater -> Update documentation
```

### For Bug Fixes

```
1. tdd-guide   -> Write failing test reproducing bug
2. [fix]       -> Make test pass
3. code-reviewer -> Review the fix
```

### For Refactoring

```
1. refactor-cleaner -> Identify dead code
2. [clean up]       -> Remove safely
3. code-reviewer    -> Review changes
4. [verify]         -> Run all tests
```

---

## Adding Your Own Content

### Custom Agents

Create a new `.md` file in `agents/` with this format:

```markdown
---
name: your-agent-name
description: What this agent does and when to use it.
tools: ["Read", "Write", "Edit", "Bash", "Grep", "Glob"]
model: opus
---

# Your Agent Name

You are an expert in [domain]. Your mission is to [objective].

## Core Responsibilities
...

## Workflow
...
```

### Custom Skills

Create a new folder in `skills/your-skill/SKILL.md`:

```markdown
# Your Skill Name

Description of what this skill covers.

## When to Use
- Trigger condition 1
- Trigger condition 2

## Patterns
...

## Examples
...
```

### Custom Rules

Add a new `.md` file in `rules/`:

```markdown
# Your Rule Category

## Mandatory Checks
- Rule 1
- Rule 2

## Best Practices
...
```

---

## Reference Material

The `everything-claude-code/` folder contains reference material from the [Everything Claude Code](https://github.com/affaan-m/everything-claude-code) project:

- **The Shortform Guide** - Setup: skills, hooks, subagents, MCPs, plugins
- **The Longform Guide** - Advanced: token economics, memory, parallelization

Key takeaways applied in this template:
1. **Context window is precious** - Disable unused MCPs and plugins
2. **Subagent architecture** - Delegate to the cheapest sufficient model
3. **Verification loops** - Always verify before proceeding
4. **Continuous learning** - Record corrections as persistent patterns
5. **Session management** - Use handoff docs when hitting context limits

---

## Contributing

1. Fork this repository
2. Create a feature branch (`feat/your-improvement`)
3. Follow the git workflow in `rules/git-workflow.md`
4. Submit a Pull Request

---

## License

MIT - Use this template freely in your projects.
