# Handoff Document

Use this template when:
- Hitting context limits
- Ending a long session
- Switching to a different task/project
- Another AI session needs to continue your work

---

## Session Info
- **Date**: YYYY-MM-DD
- **Branch**: `branch-name`
- **Duration**: ~X hours

## Current State

### What's Working
- [Feature/component that's functional]
- [Tests that are passing]

### What's Broken
- [Known issues]
- [Failing tests]

## In Progress

### Active Task
[What was being worked on when session ended]

### Partial Work
- File: `path/to/file.ts`
- State: [What's done, what's left]

## Blockers

### Technical Blockers
- [Error/issue preventing progress]
- [Missing dependency or config]

### Decision Blockers
- [Needs user input on X]
- [Architecture decision pending]

## Next Steps

### Immediate (resume with these)
1. [ ] First thing to do
2. [ ] Second thing to do

### After That
3. [ ] Follow-up task
4. [ ] Another task

## Key Decisions Made

| Decision | Rationale | Date |
|----------|-----------|------|
| [What was decided] | [Why] | YYYY-MM-DD |

## Files Modified This Session

| File | Change Type | Notes |
|------|-------------|-------|
| `path/to/file.ts` | Created/Modified/Deleted | [Brief note] |

## Commands to Run

```bash
# To restore state
git checkout branch-name
npm install

# To verify current state
npm run validate

# To continue work
[specific command if applicable]
```

## Context for Continuation

[Any important context the next session needs to know - architecture decisions, 
user preferences stated, edge cases discovered, etc.]
