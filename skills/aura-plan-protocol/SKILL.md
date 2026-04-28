---
name: aura-plan-protocol
description: Guidelines for creating and managing implementation plans with citations, phases, and status tracking within the Aura Subagent Suite.
---

# Aura Plan Protocol

**Applies to:** aura (orchestrator) — used in lightweight mode when tracking multi-step work

**Purpose:** Structured implementation plans that track progress, cite research, and provide clear handoff between phases.

## TL;DR Checklist

When creating or updating a plan, ensure:

- [ ] YAML frontmatter with `status`, `phase`, `updated`
- [ ] `## Goal` section (one sentence)
- [ ] `## Context & Decisions` table with citations (`ref:artifact-name`)
- [ ] Phases with status markers: `[COMPLETE]`, `[IN PROGRESS]`, `[PENDING]`
- [ ] Tasks with hierarchical numbering (1.1, 1.2, 2.1)
- [ ] Only ONE task marked `← CURRENT`
- [ ] Citations for all research-based decisions

---

## When to Use

1. Starting a multi-step implementation (lightweight mode)
2. After receiving a complex user request
3. When tracking progress across phases
4. After research that informs architectural decisions

## When NOT to Use

1. Simple one-off tasks — use built-in todos instead
2. Pure research/exploration — use delegations only
3. Quick fixes that don't need tracking
4. Full production features — use the 5-gate pipeline instead

---

## Plan Format

Use `todowrite` with this exact markdown structure:

```markdown
---
status: in-progress
phase: [Phase Name]
updated: YYYY-MM-DD
---

## Plan: [Goal]

### Context and Decisions
| Decision | Rationale | Source |
|----------|-----------|--------|
| CHOICE | WHY | `ref:DELEGATION_ID` |

### Phase 1: NAME [COMPLETE]
- [x] 1.1 Completed task
- [x] 1.2 Another completed task -> `ref:DELEGATION_ID`

### Phase 2: NAME [IN PROGRESS]
- [ ] **2.1 Current task** <- CURRENT
- [ ] 2.2 Pending task

### Phase 3: NAME [PENDING]
- [ ] 3.1 Future task
```

### Phase Status Markers

| Marker | Meaning |
|--------|---------|
| `[PENDING]` | Not yet started |
| `[IN PROGRESS]` | Currently being worked on |
| `[COMPLETE]` | Finished successfully |
| `[BLOCKED]` | Waiting on dependencies |

### Critical Rules

1. Only ONE phase may be `[IN PROGRESS]` at any time
2. Only ONE task may have `<- CURRENT` marker at any time
3. Move `<- CURRENT` immediately when starting a new task
4. Mark tasks `[x]` immediately after completing them

---

## State Machine

### Plan Lifecycle
```
not-started -> in-progress -> complete
                         ↘ blocked
```

### Phase Lifecycle
```
[PENDING] -> [IN PROGRESS] -> [COMPLETE]
                          ↘ [BLOCKED]
```

### Task Lifecycle
```
[ ] unchecked -> [x] checked
```

---

## Citations and Delegations

Citations reference delegation research outputs (artifacts in `.aura/`):

1. You delegate research: `task` to `aura-researcher` or `aura-explore`
2. Research completes with structured artifact (e.g., `aura-RESEARCH-CODE.md`)
3. You cite that research in the plan

### When to Cite

| Situation | Action |
|-----------|--------|
| Architectural decision based on research | Add to Context and Decisions table |
| Task informed by research | Append `-> ref:artifact-name` to task line |
| Implementation detail from research | Inline citation in Notes |

### NEVER
- Make up delegation references
- Skip citations for research-based decisions
- Cite without actually having done the research

---

## Examples

### ✅ CORRECT: Well-formed plan

```markdown
---
status: in-progress
phase: Implementation
updated: 2026-04-28
---

## Plan: Add JWT authentication with refresh token support

### Context and Decisions
| Decision | Rationale | Source |
|----------|-----------|--------|
| Use bcrypt (12 rounds) | Industry standard, balance of security/speed | `ref:aura-RESEARCH-WEB` |
| JWT with refresh tokens | Stateless auth, mobile-friendly | `ref:aura-RESEARCH-WEB` |

### Phase 1: Research [COMPLETE]
- [x] 1.1 Research auth patterns -> `ref:aura-RESEARCH-WEB`
- [x] 1.2 Evaluate token strategies -> `ref:aura-RESEARCH-WEB`

### Phase 2: Implementation [IN PROGRESS]
- [x] 2.1 Set up project structure
- [ ] **2.2 Add password hashing** <- CURRENT
- [ ] 2.3 Implement JWT generation

### Phase 3: Testing [PENDING]
- [ ] 3.1 Write unit tests
- [ ] 3.2 Integration tests
```

### ❌ WRONG: Missing frontmatter context

```markdown
## Plan: Add authentication

- [ ] Set up auth
- [ ] Make it work
```

**Error:** No Context & Decisions table, no citations, tasks are vague.

### ❌ WRONG: Multiple CURRENT markers

```markdown
### Phase 2: Implementation [IN PROGRESS]
- [ ] **2.1 Task one** <- CURRENT
- [ ] **2.2 Task two** <- CURRENT
```

**Error:** Only one task may be marked CURRENT.

### ❌ WRONG: Decision without citation

```markdown
### Context and Decisions
| Decision | Rationale | Source |
|----------|-----------|--------|
| Use Redis | It's fast | - |
```

**Error:** Decisions must cite research with `ref:artifact-name`.

---

## Troubleshooting

| Error / Problem | Fix |
|-----------------|-----|
| "No clear goal" | Add a specific, one-sentence goal at the top |
| "Multiple CURRENT markers" | Remove `<- CURRENT` from all but the active task |
| "Invalid citation format" | Use `ref:artifact-name` format (e.g., `ref:aura-RESEARCH-WEB`) |
| "Missing decisions table" | Add `### Context and Decisions` with cited rationale |
| "Empty phase" | Add at least one task to each phase |
| "Invalid phase status" | Use `[PENDING]`, `[IN PROGRESS]`, `[COMPLETE]`, or `[BLOCKED]` |

---

## Before Saving Checklist

Before finalizing a plan, verify:

- [ ] **Frontmatter context:** Is there a Context & Decisions table?
- [ ] **Goal:** Is there a clear, one-sentence goal?
- [ ] **Citations:** Are all research-based decisions cited with `ref:artifact-name`?
- [ ] **Single CURRENT:** Is exactly one task marked `<- CURRENT`?
- [ ] **Valid markers:** Do all phases use valid status markers?
- [ ] **Hierarchical IDs:** Are tasks numbered correctly (1.1, 1.2, 2.1)?
