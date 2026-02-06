# [Feature Name] Tasks

TDD Workflow: Write tests FIRST (RED), implement to pass (GREEN), refactor (IMPROVE).
Each task maps to exactly ONE file.

---

## Phase 1: Foundation

### task0001: [Task Name]
File: [path/to/file.ts]
Description: [What this task does]
Status: pending

### task0002: [Task Name]
File: [path/to/file.ts]
Description: [What this task does]
Status: pending
Depends: task0001

---

## Phase 2: Core Feature (TDD)

### task0003: [Feature] tests
File: [path/to/feature.test.ts]
Description: Write tests for [feature] - [what to test].
Status: pending

### task0004: [Feature] implementation
File: [path/to/feature.ts]
Description: Implement [feature] to make tests pass.
Status: pending
Depends: task0003

---

## Phase 3: Integration (TDD)

### task0005: [Integration] tests
File: [path/to/integration.test.ts]
Description: Write integration tests for [what].
Status: pending
Depends: task0004

### task0006: [Integration] implementation
File: [path/to/integration.ts]
Description: Implement [integration] to make tests pass.
Status: pending
Depends: task0005

---

## Phase 4: E2E Tests

### task0007: E2E [flow name]
File: [path/to/e2e.spec.ts]
Description: Write E2E tests for [user journey].
Status: pending
Depends: task0006

---

## Task Status Legend

- **pending** - Not started
- **in-progress** - Currently being worked on
- **completed** - Done and verified
- **blocked** - Waiting on dependency or decision
