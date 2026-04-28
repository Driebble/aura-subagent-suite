---
name: aura-plan-review
description: Criteria for reviewing implementation plans against quality standards. Load this when auditing plans for citation quality, completeness, and actionability.
---

# Aura Plan Review

> **Load this skill** when reviewing implementation plans (not code).

## TL;DR

Systematic plan review focused on 3 quality categories: **Citation Quality**, **Completeness**, and **Actionability**. Plans created via `todowrite` should be evaluated against these criteria before execution.

## When to Use This Skill

- When reviewing implementation plans before execution
- When auditing plan quality after creation
- When verifying plans meet documentation standards
- As part of the plan validation workflow

---

## Plan Review Checklist

### 1. Citation Quality

| Requirement | Check |
|-------------|-------|
| Decisions reference sources | `ref:artifact-name` format used (e.g., `ref:aura-RESEARCH-CODE`) |
| No unsubstantiated claims | Architectural decisions cite research |
| Research phases show refs | Completed research tasks include citations |
| Citations are verifiable | IDs match actual artifact outputs in `PROJ_DIR/.aura/` |

**Red Flags:**
- Decisions table with empty or `-` in Source column
- Claims like "industry standard" or "best practice" without citation
- Research tasks marked complete without `-> ref:id`

### 2. Completeness

| Requirement | Check |
|-------------|-------|
| Goal is specific | Measurable outcome, not vague intent |
| Phases are logical | Sequential, with clear progression |
| Edge cases considered | Error handling, failure modes addressed |
| Notes section present | Key decisions and observations documented |
| Context & Decisions table | Captures architectural choices with rationale |

**Goal Quality Examples:**
- ❌ "Improve authentication" (vague)
- ❌ "Make it better" (unmeasurable)
- ✅ "Add JWT authentication with refresh token support" (specific)
- ✅ "Migrate user table to PostgreSQL with zero downtime" (measurable)

### 3. Actionability

| Requirement | Check |
|-------------|-------|
| Tasks are specific | Clear what file/component is affected |
| No ambiguous tasks | Avoids "investigate" or "figure out" without scope |
| Dependencies clear | Sequential tasks show logical order |
| Implementation path obvious | Developer can start without clarification |

**Actionability Examples:**
- ❌ "Set up the backend" (too vague)
- ❌ "Make it work" (no implementation path)
- ✅ "Create `src/auth/jwt.ts` with sign/verify functions" (specific file)
- ✅ "Add bcrypt password hashing to `UserService.create()`" (clear scope)

---

## Severity Classification

| Severity | Icon | Criteria | Action Required |
|----------|------|----------|-----------------|
| Critical | 🔴 | Missing citations for key decisions, no clear goal, unactionable tasks | Must fix before execution |
| Major | 🟠 | Vague tasks, incomplete phases, missing edge case handling | Should fix |
| Minor | 🟡 | Missing notes, unclear dependencies, incomplete rationale | Nice to fix |
| Nitpick | 🟢 | Style preferences, wording suggestions | Optional |

---

## Output Format

Structure your plan review as:

```markdown
## Plan Review

### Files Reviewed
- `PLAN.md` (or plan content from `todowrite`)

### Overall Assessment
APPROVE | REQUEST_CHANGES | NEEDS_DISCUSSION

### Summary
2-3 sentence overview of plan quality.

### Issues

#### 🔴 Critical
- [Issue description with specific location]

#### 🟠 Major
- [Issue description with specific location]

#### 🟡 Minor
- [Issue description with specific location]

#### 🟢 Nitpick
- [Suggestion]

### Quality Assessment

| Check | Status |
|-------|--------|
| Goal is specific and measurable | PASS / FAIL |
| Citations support key decisions | PASS / FAIL |
| Tasks are actionable | PASS / FAIL |
| Edge cases addressed | PASS / FAIL |

### Positive Observations
- [What's done well - always include at least one]
```

---

## What NOT to Do

- Do NOT evaluate code quality (that's `aura-code-review`'s job)
- Do NOT execute or modify the plan during review
- Do NOT skip citation verification for decisions
- Do NOT accept vague goals or ambiguous tasks
- Do NOT forget to note positive observations

---

## Adherence Checklist

Before completing a plan review, verify:

- [ ] All 3 quality categories analyzed (Citations, Completeness, Actionability)
- [ ] Severity assigned to each finding
- [ ] Specific locations noted for all issues
- [ ] Quality Assessment table completed
- [ ] Positive observations noted
- [ ] Output follows the standard format
