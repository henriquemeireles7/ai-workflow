# Agent Usage Guidelines

## When to Delegate to Agents

| Agent | When to Use |
|-------|------------|
| **planner** | Before starting any feature >1 file |
| **architect** | System design, scalability decisions |
| **tdd-guide** | Writing any new feature (tests first!) |
| **code-reviewer** | After writing or modifying code |
| **security-reviewer** | Auth code, user input, API endpoints, financial ops |
| **build-error-resolver** | Build failures, type errors |
| **e2e-runner** | Creating/maintaining E2E tests |
| **database-reviewer** | SQL queries, schema changes, migrations |
| **refactor-cleaner** | Dead code cleanup, dependency audit |
| **doc-updater** | Updating codemaps and documentation |

## Agent Workflow Order

For new features:
1. **planner** - Create implementation plan
2. **tdd-guide** - Write tests first, then implement
3. **code-reviewer** - Review the code
4. **security-reviewer** - Check security (if applicable)
5. **doc-updater** - Update documentation

For bug fixes:
1. **tdd-guide** - Write failing test reproducing the bug
2. **build-error-resolver** - If build errors involved
3. **code-reviewer** - Review the fix

## Rules for Agents

- Each agent gets ONE clear task
- Agents produce ONE clear output
- Use `/clear` between agents to free context
- Store intermediate outputs in files
- Never skip the review phase
