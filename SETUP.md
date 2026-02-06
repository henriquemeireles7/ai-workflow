# Setup Guide

Step-by-step guide to integrate the AI workflow template into your project.

---

## Step 1: Choose Your Integration Method

### Option A: Copy into existing project (Recommended)

```bash
# From your project root
mkdir -p .ai

# Copy interactive mode files
cp -r /path/to/ai-workflow/agents/ .ai/agents/
cp -r /path/to/ai-workflow/rules/ .ai/rules/
cp -r /path/to/ai-workflow/skills/ .ai/skills/
cp -r /path/to/ai-workflow/templates/ .ai/templates/

# Copy autonomous mode files
cp /path/to/ai-workflow/loop.sh ./loop.sh
chmod +x loop.sh
cp -r /path/to/ai-workflow/prompts/ ./prompts/
cp -r /path/to/ai-workflow/specs/ ./specs/
mkdir -p sessions/
```

### Option B: Use as Git submodule

```bash
# Add as submodule
git submodule add https://github.com/YOUR_USERNAME/ai-workflow.git .ai

# Later, update to latest
git submodule update --remote .ai
```

### Option C: GitHub template repository

1. Click "Use this template" on GitHub
2. Clone your new repo
3. Copy files into your projects as needed

---

## Step 2: Configure Your AI Tool

### For Claude Code (Interactive Mode)

Copy and customize the `CLAUDE.md` file:

```bash
cp .ai/CLAUDE.md ./CLAUDE.md
```

Edit `CLAUDE.md` to include:
- Your project description
- Tech stack details
- Project-specific conventions
- File structure overview

Claude Code reads this file at the start of every session.

### For Claude Code (Autonomous Mode)

Create the operational brief:

```bash
cp .ai/templates/AGENTS.md ./AGENTS.md
```

Edit `AGENTS.md` with:
- Project name and description
- Tech stack
- Project structure
- Key commands (dev, test, lint, build)
- Architecture rules
- Current status

Keep AGENTS.md **under 80 lines** - this is prime context window space.

### For Cursor

Copy and customize the `.cursorrules` file:

```bash
cp .ai/.cursorrules ./.cursorrules
```

Or create modular rules in `.cursor/rules/`:

```bash
mkdir -p .cursor/rules
cp .ai/rules/*.md .cursor/rules/
```

### For Other AI Tools

Most AI coding tools support a system prompt or rules file. Adapt the content from `rules/` to your tool's format.

---

## Step 3: Set Up Autonomous Mode

### 3a. Prerequisites

```bash
# Install Claude Code CLI
npm install -g @anthropic-ai/claude-code

# Verify
claude --version
```

### 3b. Write Your First Spec

```bash
cp specs/EXAMPLE_SPEC.md specs/my-feature.md
```

Edit the spec with real requirements using the Given/When/Then format for acceptance criteria.

### 3c. Create AGENTS.md

```bash
cp templates/AGENTS.md ./AGENTS.md
```

This is the first file Claude reads every iteration. It must contain:
- What the project is
- How to run it (commands)
- Code style rules
- Architecture constraints

### 3d. Test the Loop

```bash
# Dry run: generate a plan
./loop.sh plan

# Review the plan
cat IMPLEMENTATION_PLAN.md

# If plan looks good, execute it
./loop.sh build
```

### 3e. Add to .gitignore

Add these to your `.gitignore`:

```
# Autonomous loop session logs
sessions/
sessions/*.log
sessions/*.md
```

---

## Step 4: Customize Agents

Each agent in `agents/` has sections marked with:

```
> **CUSTOMIZE**: Description of what to change
```

Go through each agent and:

1. **architect.md** - Add your project's tech stack and architecture
2. **code-reviewer.md** - Add project-specific review checklist items
3. **security-reviewer.md** - Add domain-specific security checks
4. **e2e-runner.md** - Add your critical user journeys
5. **build-error-resolver.md** - Add framework-specific error patterns
6. **refactor-cleaner.md** - Define "never remove" and "safe to remove" lists

---

## Step 5: Add Project-Specific Skills

Create skills for your project's domain:

```bash
# Example: Add a Next.js skill
mkdir -p .ai/skills/nextjs
cat > .ai/skills/nextjs/SKILL.md << 'EOF'
# Next.js Patterns

## App Router Conventions
- Server Components by default
- Use 'use client' only when needed
- Prefer Server Actions for mutations

## File Conventions
- page.tsx - Route page
- layout.tsx - Shared layout
- loading.tsx - Loading UI
- error.tsx - Error boundary
EOF
```

Common skills to add:
- `skills/your-framework/SKILL.md` (Next.js, Express, Django, etc.)
- `skills/your-database/SKILL.md` (Postgres, MongoDB, etc.)
- `skills/your-deployment/SKILL.md` (Vercel, AWS, etc.)
- `skills/your-auth/SKILL.md` (Auth.js, Clerk, Supabase Auth, etc.)

---

## Step 6: Write Specs for Autonomous Mode

The autonomous planner reads all files in `specs/` to build the implementation plan. Good specs include:

1. **Clear requirements** - Specific, testable ("Users can log in with email/password")
2. **Acceptance criteria** - Given/When/Then format
3. **Priority** - P0/P1/P2/P3
4. **Out of scope** - What NOT to build

Example structure:

```bash
specs/
├── user-auth.md         # Authentication feature
├── user-profiles.md     # Profile management
├── api-endpoints.md     # REST API spec
└── database-schema.md   # Data model
```

---

## Step 7: Configure Git Hooks (Optional)

Add pre-commit validation:

```bash
# .husky/pre-commit
npm run lint
npm run test
```

Add to `package.json`:

```json
{
  "scripts": {
    "validate": "npm run lint && npm run test && npm run build"
  }
}
```

---

## Step 8: Verify Setup

### Interactive Mode Checklist
- [ ] `CLAUDE.md` or `.cursorrules` exists at project root
- [ ] Agents are accessible to your AI tool
- [ ] Rules are loaded (check AI tool's context)
- [ ] At least one skill is customized for your project
- [ ] Git workflow rules match your team's process
- [ ] Security rules are appropriate for your domain

### Autonomous Mode Checklist
- [ ] `claude` CLI is installed and working
- [ ] `AGENTS.md` exists at project root (under 80 lines)
- [ ] `loop.sh` is executable (`chmod +x loop.sh`)
- [ ] `prompts/` folder has all three prompt files
- [ ] At least one spec exists in `specs/`
- [ ] `sessions/` is in `.gitignore`
- [ ] Git branch is set up for the feature

---

## Directory Structure Reference

After full setup, your project should look like:

```
your-project/
├── .ai/                       # AI workflow configuration
│   ├── agents/                # Subagent definitions
│   ├── rules/                 # AI behavior rules
│   ├── skills/                # Knowledge modules
│   └── templates/             # Document templates
│
├── loop.sh                    # Autonomous loop script
├── prompts/                   # Prompt templates
│   ├── PROMPT_plan.md
│   ├── PROMPT_build.md
│   └── PROMPT_plan_work.md
├── specs/                     # Feature requirements
├── sessions/                  # Session logs (gitignored)
│
├── AGENTS.md                  # Operational brief (autonomous mode)
├── IMPLEMENTATION_PLAN.md     # Generated task list (autonomous mode)
├── CLAUDE.md                  # Claude Code entry point
├── .cursorrules               # Cursor IDE rules
│
├── src/                       # Your source code
├── tests/                     # Your tests
└── ...
```

---

## Autonomous Loop Modes

| Mode | Command | When to Use |
|------|---------|-------------|
| **plan** | `./loop.sh plan` | You have specs and want a structured plan |
| **build** | `./loop.sh build` | You have a plan and want to execute it |
| **plan-work** | `./loop.sh plan-work` | Prototyping, no specs yet, iterative work |

### Typical Flow

```bash
# 1. Write specs
vim specs/my-feature.md

# 2. Create plan from specs
./loop.sh plan

# 3. Review and edit the plan
vim IMPLEMENTATION_PLAN.md

# 4. Execute the plan
./loop.sh build

# 5. Review results
git log --oneline
cat IMPLEMENTATION_PLAN.md
```

---

## Troubleshooting

### AI tool doesn't see the agents
- Verify file paths match your AI tool's expected locations
- For Claude Code: agents go in `.claude/agents/` or project root `agents/`
- For Cursor: rules go in `.cursor/rules/` or `.cursorrules`

### Context window too small
- Disable unused MCPs and plugins
- Use smaller models for simple tasks (see `rules/performance.md`)
- Compact context regularly
- Keep AGENTS.md under 80 lines

### AI doesn't follow the rules
- Ensure rules are in the correct location for your tool
- Rules should be concise - avoid walls of text
- The most important rules should be at the top of each file

### Autonomous loop runs but doesn't make progress
- Check `sessions/` for raw logs
- Review the prompt - is it clear enough?
- Review IMPLEMENTATION_PLAN.md - are tasks specific and small enough?
- Check AGENTS.md - does it have the right commands?

### Autonomous loop stops early
- Check max iterations (default: 25, increase with `./loop.sh build 50`)
- Check if all tasks are marked `[x]` in IMPLEMENTATION_PLAN.md
- Review the last session log for errors

### Claude CLI not found
- Install: `npm install -g @anthropic-ai/claude-code`
- Verify: `claude --version`
- Check your PATH if installed but not found
