# Continuous Learning Skill

Extract and persist patterns from AI sessions to avoid repeating mistakes.

## When to Use

- When the user corrects the AI
- When a solution is discovered after multiple attempts
- When a project-specific convention is established
- When a workaround for a framework quirk is found

## How It Works

### Pattern Detection

Look for these signals during sessions:

| Signal | Action |
|--------|--------|
| User says "no, do it like X" | Record X as preferred pattern |
| Same error fixed 2+ times | Record fix as learned pattern |
| Workaround discovered | Record workaround with context |
| "Always do X" or "Never do Y" | Record as project rule |

### Recording Patterns

When a pattern is detected, append to the relevant skill:

```markdown
## Learned Patterns

### YYYY-MM-DD: [Pattern Name]
**Context**: [When this applies]
**Pattern**: [What to do]
**Example**:
```code
[Code example]
```
**Rationale**: [Why this is the right approach]
```

### Where to Record

| Pattern Type | Record In |
|--------------|-----------|
| Backend patterns | `skills/backend-patterns/SKILL.md` |
| Frontend patterns | `skills/frontend-patterns/SKILL.md` |
| Testing patterns | `skills/tdd-workflow/SKILL.md` |
| Security patterns | `skills/security-review/SKILL.md` |
| General coding | `skills/coding-standards/SKILL.md` |
| Project-specific | `project-log.md` |

## Session End Protocol

At the end of each significant session:

1. **Review corrections**: What did the user correct?
2. **Review discoveries**: What solutions were discovered?
3. **Update project-log.md**: Record what worked/didn't work
4. **Update relevant skill**: Append any new learned patterns
5. **Create handoff if needed**: Use `templates/HANDOFF.md`

## Example: Recording a Correction

**User says**: "Don't use `any` type, always use `unknown` for external data"

**Action**: Append to `skills/coding-standards/SKILL.md`:

```markdown
## Learned Patterns

### 2026-02-04: External Data Typing
**Context**: When receiving data from external sources (API responses, user input)
**Pattern**: Use `unknown` instead of `any`, then validate/narrow
**Example**:
```typescript
// ❌ Bad
const data: any = await response.json();

// ✅ Good
const data: unknown = await response.json();
const validated = schema.parse(data); // Zod validation
```
**Rationale**: `unknown` forces explicit type narrowing, catching runtime errors at compile time
```

## Anti-Patterns to Avoid

- Don't record one-time fixes
- Don't record simple typo corrections
- Don't record external service issues
- Don't duplicate existing patterns

## Integration with User Rules

Add to Cursor user rules:
```
When I correct you, append the correction as a learned pattern to the relevant skill file.
```
