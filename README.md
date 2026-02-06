# AI Workflow Template

A battle-tested template for **agentic AI development** -- the center of truth for coding with AI agents (Claude Code, Cursor, Copilot, etc.).

This template supports **two modes** of AI-assisted development:

- **Interactive Mode** - Human-in-the-loop with agents, rules, and skills (Cursor, Claude Code sessions)
- **Autonomous Mode** - Headless AI running in a bash loop with specs, plans, and auto-commit (Claude Code CLI)

---

## What's Inside

```
ai-workflow/
├── loop.sh                        # Autonomous loop script (plan/build/plan-work)
│
├── prompts/                       # Prompt templates for autonomous mode
│   ├── PROMPT_plan.md             # Planning prompt (specs -> plan)
│   ├── PROMPT_build.md            # Build prompt (plan -> code)
│   └── PROMPT_plan_work.md        # Hybrid prompt (plan one + build one)
│
├── specs/                         # Feature requirements (input for planning)
│   └── EXAMPLE_SPEC.md            # Spec template
│
├── sessions/                      # Auto-generated session logs (gitignored)
│
├── agents/                        # Subagent definitions
│   ├── planner.md                 # Feature planning specialist
│   ├── architect.md               # System design specialist
│   ├── tdd-guide.md               # Test-driven development enforcer
│   ├── code-reviewer.md           # Code quality & security reviewer
│   ├── security-reviewer.md       # OWASP Top 10 & vulnerability scanner
│   ├── build-error-resolver.md    # Build/type error fixer
│   ├── e2e-runner.md              # Playwright E2E testing specialist
│   ├── database-reviewer.md       # PostgreSQL optimization specialist
│   ├── refactor-cleaner.md        # Dead code & dependency cleaner
│   └── doc-updater.md             # Documentation & codemap maintainer
│
├── rules/                         # AI behavior guidelines
│   ├── agents.md                  # When to delegate to which agent
│   ├── coding-style.md            # Code conventions & anti-patterns
│   ├── git-workflow.md            # Commit format, branching, PR process
│   ├── patterns.md                # Common code patterns & templates
│   ├── performance.md             # Model selection & context management
│   ├── security.md                # Mandatory security checks
│   └── testing.md                 # TDD workflow & coverage requirements
│
├── skills/                        # Reusable knowledge modules
│   ├── continuous-learning/       # Self-improving pattern detection
│   ├── backend-patterns/          # Backend conventions
│   ├── frontend-patterns/         # Frontend conventions
│   └── ...                        # Add your own
│
├── templates/                     # Document templates
│   ├── AGENTS.md                  # Operational brief template (for autonomous mode)
│   ├── IMPLEMENTATION_PLAN.md     # Task plan template
│   ├── HANDOFF.md                 # Session handoff document
│   ├── ADR.md                     # Architecture Decision Record
│   └── PROJECT-TASKS.md           # Task tracking template
│
├── CLAUDE.md                      # Template for Claude Code users
├── .cursorrules                   # Template for Cursor IDE users
├── SETUP.md                       # Step-by-step customization guide
│
└── everything-claude-code/        # Reference: advanced agentic patterns
    ├── the-shortform-guide.md     # Setup guide for Claude Code
    └── the-longform-guide.md      # Advanced techniques & optimization
```

---

## Two Modes of Development

### Mode 1: Interactive (Human in the Loop)

Use this in **Cursor** or **Claude Code sessions** where you're actively coding with AI.

```
You: "Add user authentication"
  |
  v
planner agent -> breaks it down into tasks
  |
  v
tdd-guide agent -> writes tests first, then implements
  |
  v
code-reviewer agent -> reviews the code
  |
  v
security-reviewer agent -> checks for vulnerabilities
  |
  v
doc-updater agent -> updates documentation
  |
  v
git commit -> conventional commit message
```

**When to use:** Day-to-day development, code reviews, debugging, learning.

### Mode 2: Autonomous (AI Runs Solo)

Use the **loop.sh** script with **Claude Code CLI** for long-running, unattended tasks.

```
You: ./loop.sh plan        # AI reads specs/ and creates IMPLEMENTATION_PLAN.md
You: ./loop.sh build       # AI executes tasks one-by-one, commits each, pushes

  loop.sh
    |
    v
  [Read prompt] -> [Feed to Claude CLI] -> [Claude does work]
    |                                            |
    v                                            v
  [Git commit + push] <-------- [Task done, mark [x] in plan]
    |
    v
  [Next iteration] -> [Read prompt again] -> [Find next [ ] task] -> ...
    |
    v
  [All tasks [x]] -> DONE
```

**When to use:** Implementing a full feature from specs, building boilerplate, running through a plan overnight.

---

## Quick Start

### 1. Clone this template

```bash
git clone https://github.com/henriquemeireles7/ai-workflow.git
cd ai-workflow
```

### 2. Copy into your project

```bash
# Copy the workflow files into your project
cp -r agents/ /path/to/your-project/.ai/agents/
cp -r rules/ /path/to/your-project/.ai/rules/
cp -r skills/ /path/to/your-project/.ai/skills/
cp -r templates/ /path/to/your-project/.ai/templates/

# Copy autonomous loop files
cp loop.sh /path/to/your-project/loop.sh
cp -r prompts/ /path/to/your-project/prompts/
cp -r specs/ /path/to/your-project/specs/
mkdir -p /path/to/your-project/sessions/

# For Claude Code users
cp CLAUDE.md /path/to/your-project/CLAUDE.md

# For Cursor users
cp .cursorrules /path/to/your-project/.cursorrules
```

### 3. Customize

See [SETUP.md](./SETUP.md) for detailed customization instructions.

---

## Autonomous Mode: Step by Step

### Prerequisites

```bash
# Install Claude Code CLI
npm install -g @anthropic-ai/claude-code

# Verify it works
claude --version
```

### Step 1: Write Your Specs

Create requirement documents in `specs/`:

```bash
cp specs/EXAMPLE_SPEC.md specs/user-auth.md
# Edit specs/user-auth.md with your requirements
```

### Step 2: Create Your Operational Brief

```bash
cp templates/AGENTS.md AGENTS.md
# Edit AGENTS.md with your project details (keep it under 80 lines!)
```

### Step 3: Generate the Plan

```bash
./loop.sh plan
# Claude reads specs/ and creates IMPLEMENTATION_PLAN.md
# Review the plan - edit if needed
```

### Step 4: Execute the Plan

```bash
./loop.sh build
# Claude picks up tasks one-by-one from IMPLEMENTATION_PLAN.md
# Each task: implement -> test -> commit -> push -> next
# Stops when all tasks are marked [x]
```

### Step 5: Review

```bash
# Check progress
cat IMPLEMENTATION_PLAN.md | grep -E '^\s*- \['

# Review session logs
ls sessions/

# Review git log
git log --oneline
```

### Alternative: Plan + Work (Hybrid)

For exploratory work or when you don't have full specs yet:

```bash
./loop.sh plan-work
# Each iteration: pick one small task, plan it, build it, commit
# Great for prototyping and iterative development
```

---

## Interactive Mode: Agents

Agents are specialized AI assistants. Use them in Cursor or Claude Code sessions:

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

---

## Rules

Rules define AI behavior and are loaded by the AI tool automatically:

- **coding-style.md** - Naming conventions, file organization, anti-patterns
- **testing.md** - TDD workflow, 80% coverage requirement
- **security.md** - Mandatory security checks before every commit
- **git-workflow.md** - Conventional commits, PR process
- **performance.md** - Model selection, context window management
- **agents.md** - When to delegate to which agent

---

## Skills

Reusable knowledge modules:

- **continuous-learning** - Detects patterns from corrections and saves them
- **backend-patterns** - Backend conventions and patterns
- **frontend-patterns** - Frontend conventions and patterns
- Add your own! (`skills/your-domain/SKILL.md`)

---

## Key Principles

These principles apply to both modes:

1. **One task, one commit.** Keep changes small and reviewable.
2. **Tests first.** Write the failing test, then implement. (TDD)
3. **Context is precious.** Keep AGENTS.md short. Keep prompts focused.
4. **Steer upstream, not downstream.** Good specs and codebase patterns produce better AI output than micromanaging the AI's process.
5. **Trust the loop.** In autonomous mode, let it iterate. Observe and tune prompts between runs, not during.
6. **Disposable plans.** IMPLEMENTATION_PLAN.md is a living document. Replan when reality diverges from the plan.
7. **Verify, don't assume.** Always run tests before marking done.

---

## Reference Material

The `everything-claude-code/` folder contains reference material from the [Everything Claude Code](https://github.com/affaan-m/everything-claude-code) project:

- **The Shortform Guide** - Setup: skills, hooks, subagents, MCPs, plugins
- **The Longform Guide** - Advanced: token economics, memory, parallelization

---

## Contributing

1. Fork this repository
2. Create a feature branch (`feat/your-improvement`)
3. Follow the git workflow in `rules/git-workflow.md`
4. Submit a Pull Request

---

## License

MIT - Use this template freely in your projects.
