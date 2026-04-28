---
name: aura-code-philosophy
description: Internal logic and data flow philosophy (The 5 Laws of Elegant Defense). Agents must load this before writing or reviewing backend/logic code.
---

# Aura Code Philosophy: The 5 Laws of Elegant Defense

**Applies to:** aura-implementer, aura-reviewer

**Scope:** Internal logic, data flow, backend code, React hooks, state management, utilities — any code where functionality matters more than appearance.

**Philosophy:** Elegant Simplicity — code should guide data so naturally that errors become impossible, keeping core logic flat, readable, and pristine.

## The 5 Laws

### 1. The Law of the Early Exit (Guard Clauses)
- Indentation is the enemy of simplicity. Deep nesting hides bugs.
- Handle edge cases, nulls, and errors at the very top of functions.
- Use `if (!valid) return; doWork();` instead of `if (valid) { doWork(); }`.

### 2. Make Illegal States Unrepresentable (Parse, Don't Validate)
- Don't check data repeatedly; structure it so it can't be wrong.
- Parse inputs at the boundary. Once data enters internal logic, it must be in trusted, typed state.
- Removes defensive checks deep in algorithmic code, keeping core logic pristine.

### 3. The Law of Atomic Predictability
- A function must never surprise the caller.
- Functions should be pure where possible. Same Input = Same Output. No hidden mutations.
- Avoid `void` functions that mutate global state. Return new data structures instead.

### 4. The Law of Fail Fast, Fail Loud
- Silent failures cause complexity later.
- If a state is invalid, halt immediately with a descriptive error. Do not try to patch bad data.
- Keeps logic simple by never accounting for half-broken states.

### 5. The Law of Intentional Naming
- Comments are often a crutch for bad code.
- Variables and functions must be named so clearly that logic reads like an English sentence.
- `isUserEligible` > `check()`. The name itself guarantees the boolean logic.

## Adherence Checklist

Before completing your task, verify:
- [ ] **Guard Clauses:** Are all edge cases handled at the top with early returns?
- [ ] **Parsed State:** Is data parsed into trusted types at the boundary?
- [ ] **Purity:** Are functions predictable and free of hidden mutations?
- [ ] **Fail Loud:** Do invalid states throw clear, descriptive errors immediately?
- [ ] **Readability:** Does the logic read like an English sentence?
