# AI Coding Product-Grade Handbook

Purpose: guide AI coding agents to produce product-grade vehicle embedded Linux
C++ changes while spending the minimum context needed for the task.

This is the project-level entry point. Use it to decide which local documents to
read, how to constrain edits, how to verify work, and how to report results. Do
not load offline standards archives unless this handbook explicitly routes the
task there.

The full project framework is
[ai-vibe-coding-framework.md](ai-vibe-coding-framework.md). This handbook is the
operational playbook inside that framework.

## 1. Operating Principles

- Product code first: generated code must be maintainable, testable,
  diagnosable, statically analyzable, and suitable for long-running embedded
  Linux systems.
- Smallest correct change: modify only files required by the requested behavior.
  Do not perform opportunistic rewrites, broad formatting, framework swaps,
  dependency changes, ABI changes, or language-standard changes.
- Evidence before completion: every code change must be paired with relevant
  build, test, formatting, and static-analysis evidence, or a clear explanation
  of why a check could not run.
- Local rules before large sources: cite local rule IDs and sections. Use
  targeted search before opening large references.
- Human-approved deviations only: AI must not invent exceptions to the standard.
  Any rule deviation requires an explicit deviation record and reviewer approval.

## 2. Context Budget Ladder

Use the lowest tier that can complete the task. Move up only when the current
tier is insufficient.

| Tier | Read | Use For | Stop Before |
| :--- | :--- | :--- | :--- |
| H0 | `docs/ai-vibe-coding-framework.md`, this handbook, task prompt, touched files/tests, `.clang-format`, `.clang-tidy` | Small edits, reviews, local fixes | Full raw standards and unrelated modules. |
| H1 | `docs/ai-vibe-coding-cpp-check-rules.md` | Any C++ generation, review, or refactor | Source traceability unless a rule conflict appears. |
| H2 | `docs/vehicle-embedded-linux-cpp-coding-standard.md` | Non-trivial product C++ design, cross-cutting behavior, rule interpretation | Raw OCR/PDF archives. |
| H3 | `docs/references/vehicle-embedded-linux-cpp-standards-traceability.md`, `docs/references/vehicle-embedded-linux-cpp-standards-sources.md`, and `docs/references/raw-source-inventory.md` | Standards updates, rule conflicts, source coverage, audit mapping | Whole large raw files without a search target. |
| H4 | Targeted sections inside `docs/raw/` | Deep reconstruction of source-backed rules | Broad archive reads. |

Token rules:

- Prefer `rg` or scoped search over whole-file reads for large documents.
- Quote rule IDs such as `CPP-AI-002` or checklist names instead of copying long
  passages.
- Do not summarize unrelated sources in final answers.
- Do not inspect `docs/raw/` for ordinary coding tasks.
- Do not modify `docs/raw/` or `skills/`.
- Keep final responses in the compact shape defined in section 9.

## 3. Document Map

| File | Role |
| :--- | :--- |
| `docs/ai-vibe-coding-framework.md` | Overall AI vibe coding framework and source-routing model. |
| `docs/ai-coding-product-grade-handbook.md` | Entry workflow for AI agents. |
| `docs/ai-vibe-coding-cpp-check-rules.md` | Compact C++ P0/P1 checks, token policy, prompt patterns, final response shape. |
| `docs/vehicle-embedded-linux-cpp-coding-standard.md` | Full enforceable project C++ standard with local rule IDs. |
| `docs/references/vehicle-embedded-linux-cpp-standards-traceability.md` | Source-to-rule coverage, Obsidian graph, standards update rules. |
| `docs/references/vehicle-embedded-linux-cpp-standards-sources.md` | Source catalog and rebuild prompts. |
| `docs/references/raw-source-inventory.md` | Read-only raw source inventory and quality notes. |
| `.clang-format` | Required formatting configuration. |
| `.clang-tidy` | Required static-analysis configuration. |
| `AGENTS.md`, `CLAUDE.md`, `.github/copilot-instructions.md` | Tool-specific entry instructions. |

## 4. Task Start Contract

Before editing C++ code, identify:

1. Requested behavior and explicit non-goals.
2. Files likely to change and tests likely to verify the change.
3. Relevant local rules by document section or rule ID.
4. Ownership, lifetime, threading, timing, error, security, persistence, or
   Linux/POSIX boundaries affected by the task.
5. Verification commands available in this repository.

If any of these are unclear and cannot be discovered from local context, ask a
focused question before editing. If a reasonable assumption is safe, state the
assumption and continue with the smallest reversible change.

## 5. Product-Grade C++ Baseline

Default to C++14 and the repository toolchain configuration.

Required by default:

- RAII for every resource, including file descriptors, sockets, timers, epoll,
  shared memory, locks, and thread ownership.
- Explicit ownership in APIs: use values, references, `std::unique_ptr`, or
  documented non-owning pointers according to the existing module style.
- Explicit error handling through `Result`, `Status`, typed errors, or the
  module's existing equivalent.
- Bounded runtime behavior: no unbounded queues, retries, waits, log loops,
  caches, allocations in hot paths, or container growth.
- Deterministic tests covering success paths, boundaries, failure paths, and
  resource exhaustion where applicable.
- Testable adapters around Linux/POSIX calls instead of direct system calls in
  business logic.

Prohibited unless a human-approved deviation exists:

- Exceptions as normal control flow, RTTI, `dynamic_cast`, naked `new/delete`,
  `malloc/free` in business logic, C-style casts, unapproved `reinterpret_cast`,
  mutation through `const_cast`, `system`, `popen`, shell string construction,
  global mutable state, hidden singletons, detached threads, default-capture
  lambdas, ignored fallible return values, and sensitive data in logs.

Use `docs/ai-vibe-coding-cpp-check-rules.md` section 3 for P0 rejection rules
and section 4 for P1 justification rules.

## 6. Edit Workflow

1. Read only the current context tier and touched files.
2. Search for existing module patterns before introducing a new helper,
   abstraction, dependency, or error type.
3. Add or update tests first when behavior changes. For documentation-only
   edits, verify links, headings, and entry consistency instead.
4. Make a surgical diff. Preserve public APIs, ABI, build system, and language
   standard unless the task explicitly requires changing them.
5. Run the narrowest meaningful verification, then broaden if shared behavior
   was touched.
6. Report verification evidence and remaining risk without restating the full
   standard.

Avoid:

- Full-file rewrites when a patch is enough.
- Refactoring unrelated code while implementing a feature.
- Reading generated archives to answer a local coding question.
- Adding generic flexibility for a single concrete requirement.
- Generating demo-only code without tests, error handling, or resource bounds.

## 7. Review Workflow

For C++ review, lead with defects and cite local rules only when useful.

Review passes:

- Interface: ownership, lifetime, blocking behavior, thread safety, errors, and
  time units are visible at the API boundary.
- Memory: RAII covers every resource; state is initialized; references cannot
  dangle.
- Error: fallible APIs are checked; `errno`, `EINTR`, short I/O, timeouts, and
  resource exhaustion are handled.
- Concurrency: threads have owners and stop paths; shared state is synchronized;
  waits and locks are bounded.
- Determinism: hot paths avoid unbounded allocation, retry, wait, log, and
  container behavior.
- Type safety: conversions, narrowing, sign changes, enum/integer mixing, and
  bit operations are explicit and tested.
- Linux boundary: POSIX APIs are wrapped behind testable adapters with
  `CLOEXEC`, permissions, paths, and signal behavior controlled.
- Security: all external input is bounded and validated; secrets and sensitive
  vehicle identifiers do not enter logs or normal config.
- Testing: boundary, failure-path, timeout, stop-path, and resource-exhaustion
  cases are covered.

## 8. Verification Matrix

Choose checks based on the touched surface.

| Change Type | Required Evidence |
| :--- | :--- |
| C++ behavior | Relevant unit/integration tests, build command, formatting, clang-tidy or project static-analysis equivalent. |
| Linux/POSIX wrapper | Failure-path tests for `errno`, `EINTR`, short I/O, close/cleanup, permissions, and resource exhaustion as applicable. |
| Concurrency/timing | Deterministic stop-path, timeout, cancellation, and bounded-wait tests. Prefer fake clocks or controlled executors. |
| Security/input handling | Boundary, malformed input, oversize input, permission, and redaction tests. |
| Documentation only | Markdown/link consistency, entrypoint consistency, and scoped diff review. |
| Standards update | Traceability/source map update plus targeted source evidence. |

If a required check cannot run, state the command, why it could not run, and the
remaining risk.

## 9. Minimal Response Shape

For code changes:

```text
Changed: <files and behavior>
Verified: <commands and outcomes>
Notes: <unverified checks, assumptions, residual risk>
```

For code review:

```text
Findings:
- <severity> <file:line> <issue and impact>

Verified: <commands or inspection scope>
Notes: <test gaps or assumptions>
```

For documentation or standards updates:

```text
Changed: <docs and navigation updates>
Verified: <link/search/status checks>
Notes: <sources not read, if intentionally skipped>
```

Do not include full diffs, long quotations, broad source summaries, or unrelated
style commentary unless explicitly requested.

## 10. Prompt Patterns

Small product C++ change:

```text
Use docs/ai-coding-product-grade-handbook.md and
docs/ai-vibe-coding-cpp-check-rules.md first. Read the full coding standard only
if needed. Implement the smallest product-grade C++14 change, update tests, run
format/static-analysis/build/test checks, and report compact verification
evidence.
```

Focused review:

```text
Review this C++ change against docs/ai-coding-product-grade-handbook.md and
docs/ai-vibe-coding-cpp-check-rules.md. Lead with P0/P1 findings and concrete
behavioral risks. Cite local rule IDs; do not restate the full standard.
```

Standards update:

```text
Read the handbook, main coding standard, traceability index, source map, and
only targeted source sections found by search. Update enforceable local rules,
traceability, and entry references without copying long source text.
```

Token-constrained task:

```text
Use H0/H1 context only unless a local rule conflict appears. Search before
reading large files. Cite local rule IDs and keep the final answer to Changed /
Verified / Notes.
```

## 11. Deviation Handling

AI may identify a possible deviation need, but must not approve it.

Use this record format when a human requests a deviation:

```text
Deviation ID:
Rule:
File/Function:
Reason:
Risk:
Alternatives considered:
Mitigation:
Verification:
Owner:
Approval:
Expiry or review date:
```

No approval means no deviation.

## 12. Maintenance Rules

- Keep this handbook short enough to read at task start.
- Put detailed enforceable C++ rules in
  `docs/vehicle-embedded-linux-cpp-coding-standard.md`.
- Put compact AI checks in `docs/ai-vibe-coding-cpp-check-rules.md`.
- Put source coverage updates in the traceability and source-map documents.
- Keep raw source inventory in `docs/references/raw-source-inventory.md`.
- Do not duplicate offline source text in this handbook.
- Do not edit `docs/raw/` or `skills/` during framework maintenance.
- When adding a new rule, give it a stable local ID in the coding standard and
  reference that ID from compact AI documents.
