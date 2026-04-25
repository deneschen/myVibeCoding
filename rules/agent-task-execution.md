# Agent Task Execution Rules

> Karpathy Guidelines + Skill invocation system. Agent MUST follow before every task.

## Phase 1: Task Start — Think Before Acting

### 1.1 Skill Applicability Check (Mandatory)

On receiving a task, MUST first:

1. Evaluate every available skill's `description` for relevance
2. Even 1% chance a skill applies → MUST invoke to confirm
3. Priority: Process skills first (brainstorming, debugging), Implementation skills second (TDD, implementation)
4. If inapplicable, skip; if applicable, follow exactly

Banned rationalizations:
- "Just a simple question" → Questions are tasks, check skills
- "Let me look at code first" → Skills tell you HOW to look, check first
- "I remember this skill" → Skills update, read current version
- "This skill is overkill" → Skipping process makes simple things complex
- "Let me do this one thing first" → Check BEFORE doing anything

### 1.2 Surface Assumptions & Ambiguity

Before implementing, MUST:
- State assumptions explicitly — if uncertain, ask, don't choose silently
- Present multiple interpretations — if ambiguous, list all, don't pick one
- Challenge complexity — if simpler approach exists, say so; push back when warranted
- Stop when confused — name what's confusing, ask, don't guess

Output format:

    ## Task Understanding
    - Goal: [one-line description]
    - Assumptions: [list all]
    - Ambiguities: [if any, list interpretations]
    - Simplest approach: [if simpler than requested, explain]

## Phase 2: Design — Simplicity First

### 2.1 Minimum Code Principle

Only write minimum code that solves the problem. Nothing speculative:
- No features beyond what was asked
- No abstractions for single-use code
- No unrequested "flexibility" or "configurability"
- No error handling for impossible scenarios
- If 200 lines can be 50, rewrite it

Self-check: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

### 2.2 Verifiable Execution Plan

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

Multi-step tasks MUST output a plan:

    1. [Step] → verify: [check]
    2. [Step] → verify: [check]
    3. [Step] → verify: [check]

Strong success criteria → loop independently. Weak criteria ("make it work") → constant clarification.

## Phase 3: Code Execution — Surgical Changes

### 3.1 Surgical Changes

Touch only what you must. Clean up only your own mess.

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting
- Don't refactor things that aren't broken
- Match existing style, even if you'd do it differently
- If you notice unrelated dead code, mention it — don't delete it

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused
- Don't remove pre-existing dead code unless asked

Ultimate test: Every changed line must trace directly to the user's request.

### 3.2 Tool Invocation Rules

1. Read before write — read current content first, confirm search matches exactly
2. Batch parallel — read multiple independent files in parallel to reduce round-trips
3. Minimal change — prefer `apply_diff` over `write_to_file` full rewrites
4. Verify results — confirm tool returns success before proceeding
5. Command execution — state purpose clearly; prefer project-native toolchains

## Phase 4: Completion — Evidence Before Assertions

### 4.1 Must Verify Before Claiming Done

Before claiming complete, MUST:
1. Run verification commands (tests, build, lint) and confirm output
2. Confirm all planned verification points passed
3. If verification fails, fix and re-verify — don't skip

Principle: Evidence first, assertions second. No verification output = not done.

### 4.2 Completion Output Rules

When using `attempt_completion`:
- Result must be finalized, requiring no further user input
- Don't end with questions or "need further help?"
- Use clickable link format: [`symbol`](path:line)
- Summarize what was done, why, and how verified

## Skills Reference

Priority: Process > Implementation. "Build X" → brainstorming first, then implementation. "Fix bug" → debugging first, then domain skills.

Process skills (determine HOW to work):
- `brainstorming` — before creative work: explore intent, requirements, design
- `systematic-debugging` — bug/test failure/unexpected behavior: find root cause before fixing
- `test-driven-development` — before implementing features or fixing bugs: write tests first
- `writing-plans` — multi-step task before coding: break into executable plan
- `executing-plans` — have a ready plan: load, review, step through
- `karpathy-guidelines` — writing/reviewing/refactoring code: avoid overcomplication, surgical changes
- `using-superpowers` — every conversation start: establish skill discovery and invocation flow
- `verification-before-completion` — before claiming done: must run verification, evidence before assertions

Implementation skills (guide specific execution):
- `dispatching-parallel-agents` — 2+ independent tasks: dispatch specialized agents in parallel
- `subagent-driven-development` — execute plan in current session: dispatch subagents + two-stage review
- `requesting-code-review` — task complete / before merge: dispatch reviewer subagent
- `receiving-code-review` — received review feedback: evaluate rigorously, don't implement blindly
- `finishing-a-development-branch` — implementation done, tests pass: choose merge/PR/cleanup
- `using-git-worktrees` — feature needs isolation: create independent work tree
- `writing-skills` — create/edit skills: use TDD method to verify skill docs

## Task Execution Checklist

Run at the start of every task:
- [ ] Skill check: evaluate all available skills, invoke applicable ones
- [ ] Understand task: state goal, assumptions, ambiguities, simplest approach
- [ ] Make plan: break task into verifiable steps
- [ ] Minimal implementation: only write necessary code, no speculative extensions
- [ ] Surgical changes: only change what you must, match existing style
- [ ] Verify completion: run verification commands, confirm output passes
- [ ] Submit result: use attempt_completion with finalized result
