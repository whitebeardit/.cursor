---
name: planning
description: Planning and orchestration patterns for turning messy context into an incremental, verifiable execution plan. Use when you need a step-by-step strategy before coding or delegating to subagents.
---

# Planning & Orchestration

This skill helps you transform an ambiguous or messy situation into a **small-steps execution plan** with clear verification criteria, aligned with the repository's conventions.

## When to Use

**✅ DO Use:**
- When the user asks for a plan/strategy instead of code
- When requirements are unclear or the scope is large
- When you need to sequence tasks (tests → refactor → security → docs)
- When coordinating multiple subagents (testing, refactor, security, documentation)
- When you must preserve constraints (architecture rules, contracts, dependencies)

**❌ DON'T Use:**
- Trivial tasks with 1–2 obvious steps
- When the user explicitly asked for implementation right now (skip planning and implement)
- When you already have a validated plan and only need execution

## Core Concepts

### 1) Constraints First
Capture non-negotiables before proposing steps:
- Tech stack/runtime constraints (Node/TS, frameworks)
- Architecture rules (layer boundaries, repositories required, etc.)
- Contract-first requirements (OpenAPI, runtime validation)
- Observability/security baselines (logging, correlation id, rate limit, helmet)

### 2) Incremental Delivery (Micro-iterations)
Prefer **small, verifiable increments** over big-bang rewrites. Each iteration should have:
- A single goal
- Minimal change set
- A verification step (tests, lint, manual smoke check)
- A clear rollback story (what to revert if it fails)

### 3) Verification Is Part of the Plan
Every planned step must have at least one verification method:
- **Automated**: unit/integration tests, contract validation, linting
- **Manual**: curl/Postman checks for a narrow endpoint behavior
- **Safety**: ensure sensitive fields (e.g., passwords) never leak

## Planning Patterns

### Pattern 1: Situation Snapshot
In 5–10 lines, summarize:
- What exists today (endpoints, modules, structure)
- What is broken/risky (security holes, missing validation, lack of tests)
- What must not change (API contract, core behavior, compatibility)

### Pattern 2: Gap-to-Goal Mapping
List goals and map each to a concrete deliverable:
- **Tests** → test files + scenarios + data builders + how to run
- **Architecture** → target folder structure + boundaries + dependency injection approach
- **Security** → middleware + validation + secrets/token strategy + error handling

### Pattern 3: Micro-Iteration Plan (Ralph Loop)
Produce steps as **small loops**:
- Goal
- Changes (specific files/modules)
- Risks
- Verification
- Next-step prompt (optional) to delegate to a specialist subagent

### Pattern 4: “Next Prompts” (Delegation)
When appropriate, propose copy-pastable prompts to delegate execution:
- `/agt-dev-test` for tests
- `/agt-dev-refactor` for layering/refactors
- `/agt-dev-security` for hardening

## Output Template

When delivering a plan, use this structure:

1. **Summary**
2. **Constraints & Assumptions**
3. **Current Risks / Findings**
4. **Plan (small steps)**:
   - Step name
   - Goal
   - Changes (files/modules)
   - Verification
5. **Suggested Next Prompts** (optional, copy/paste)

## Key Principles

1. **Small steps beat perfect steps**
2. **Name files and verification explicitly**
3. **Respect architecture boundaries and contracts**
4. **Security and observability are not “later”**
