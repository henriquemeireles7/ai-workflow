# Setup Guide

Step-by-step guide to integrate this AI workflow template into your project.

---

## Step 1: Choose Your Integration Method

### Option A: Copy into existing project (Recommended)

```bash
# From your project root
mkdir -p .ai

# Copy workflow files
cp -r /path/to/ai-workflow/agents/ .ai/agents/
cp -r /path/to/ai-workflow/rules/ .ai/rules/
cp -r /path/to/ai-workflow/skills/ .ai/skills/
cp -r /path/to/ai-workflow/templates/ .ai/templates/
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

### For Claude Code

Copy and customize the `CLAUDE.md` file:

```bash
cp .ai/CLAUDE.md ./CLAUDE.md
```

Edit `CLAUDE.md` to include:
- Your project description
- Tech stack details
- Project-specific conventions
- File structure overview

Claude Code will automatically read this file at the start of every session.

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

## Step 3: Customize Agents

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

## Step 4: Add Project-Specific Skills

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
- not-found.tsx - 404 page

## Data Fetching
- Use React Server Components for data fetching
- Cache with `unstable_cache` or `fetch` cache options
- Revalidate with `revalidatePath` or `revalidateTag`
EOF
```

Common skills to add:
- `skills/your-framework/SKILL.md` (Next.js, Express, Django, etc.)
- `skills/your-database/SKILL.md` (Postgres, MongoDB, etc.)
- `skills/your-deployment/SKILL.md` (Vercel, AWS, etc.)
- `skills/your-auth/SKILL.md` (Auth.js, Clerk, Supabase Auth, etc.)

---

## Step 5: Set Up Task Tracking

Use the `templates/PROJECT-TASKS.md` template for your features:

```bash
cp .ai/templates/PROJECT-TASKS.md docs/tasks/feature-name.md
```

Edit the task file with your feature's phases and tasks.

---

## Step 6: Configure Git Hooks (Optional)

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

## Step 7: Verify Setup

Run this checklist:

- [ ] `CLAUDE.md` or `.cursorrules` exists at project root
- [ ] Agents are accessible to your AI tool
- [ ] Rules are loaded (check AI tool's context)
- [ ] At least one skill is customized for your project
- [ ] Git workflow rules match your team's process
- [ ] Security rules are appropriate for your domain

---

## Directory Structure Reference

After setup, your project should look like:

```
your-project/
├── .ai/                    # AI workflow configuration
│   ├── agents/             # Subagent definitions
│   ├── rules/              # AI behavior rules
│   ├── skills/             # Knowledge modules
│   └── templates/          # Document templates
├── CLAUDE.md               # Claude Code entry point
├── .cursorrules            # Cursor IDE rules
├── src/                    # Your source code
├── tests/                  # Your tests
└── ...
```

---

## Updating the Template

When you improve your workflow, push changes back to your template repo:

```bash
# Copy improved agents back to template
cp .ai/agents/new-agent.md /path/to/ai-workflow/agents/

# Commit and push template changes
cd /path/to/ai-workflow
git add . && git commit -m "feat: add new-agent"
git push
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

### AI doesn't follow the rules
- Ensure rules are in the correct location for your tool
- Rules should be concise - avoid walls of text
- The most important rules should be at the top of each file
