# Performance & Context Management

## AI Model Selection Strategy

**Fast/Light models** (e.g., Haiku, GPT-4o-mini):
- Lightweight agents with frequent invocation
- Simple code generation
- File exploration and search
- Documentation writing

**Standard models** (e.g., Sonnet, GPT-4o):
- Main development work
- Orchestrating multi-agent workflows
- Complex coding tasks
- Code reviews

**Deep reasoning models** (e.g., Opus, o1):
- Complex architectural decisions
- Maximum reasoning requirements
- Security analysis
- Debugging complex bugs

## Context Window Management

Avoid last 20% of context window for:
- Large-scale refactoring
- Feature implementation spanning multiple files
- Debugging complex interactions

Lower context sensitivity tasks:
- Single-file edits
- Independent utility creation
- Documentation updates
- Simple bug fixes

## Build Troubleshooting

If build fails:
1. Use **build-error-resolver** agent
2. Analyze error messages
3. Fix incrementally
4. Verify after each fix
