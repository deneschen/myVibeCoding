# AI Vibe Coding Framework

Purpose: provide a high-availability, high-quality framework for AI-assisted
coding against the project C++ coding standard. The framework keeps everyday AI
coding fast and compact while preserving source traceability for standards
maintenance and audits.

Protected source areas:

- `docs/raw/` is a read-only raw-source archive for this project. Do not edit,
  normalize, summarize in place, or move files there during AI coding tasks.
- `skills/` is a read-only workflow-skill archive for this project. Do not edit
  it as part of coding-standard work.

## 1. Framework Layers

| Layer | Files | Purpose | Default AI Use |
| :--- | :--- | :--- | :--- |
| L0 Entrypoints | `README.md`, `AGENTS.md`, `CLAUDE.md`, `.github/copilot-instructions.md` | Route each tool to the same project workflow. | Read the relevant entrypoint automatically. |
| L1 AI workflow | `docs/ai-coding-product-grade-handbook.md`, `docs/ai-vibe-coding-cpp-check-rules.md`, `rules/agent-task-execution.md` | Define task start, context budget, edit discipline, review passes, and reporting shape. | Read for every C++ task. |
| L2 Enforceable standard | `docs/vehicle-embedded-linux-cpp-coding-standard.md`, `.clang-format`, `.clang-tidy`, `.editorconfig` | Define product-grade C++14 rules and tool gates. | Read targeted sections when compact rules are insufficient. |
| L3 Traceability | `docs/references/vehicle-embedded-linux-cpp-standards-sources.md`, `docs/references/vehicle-embedded-linux-cpp-standards-traceability.md`, `docs/references/raw-source-inventory.md` | Explain source coverage, archive inventory, quality notes, and rule-to-source mapping. | Read for standards updates, rule conflicts, audits, and source coverage. |
| L4 Raw and workflow archives | `docs/raw/`, `skills/` | Preserve raw source material and reusable workflow skills. | Search/read only when explicitly routed. Never modify for this framework. |

## 2. Context Budget Model

Use the smallest context that can safely complete the task.

| Tier | Read | Use When | Do Not Read Yet |
| :--- | :--- | :--- | :--- |
| V0 | Tool entrypoint, this framework, touched files/tests, `.clang-format`, `.clang-tidy` | Ordinary coding tasks and local reviews. | Raw archives, full source standards, unrelated modules. |
| V1 | `docs/ai-coding-product-grade-handbook.md` and `docs/ai-vibe-coding-cpp-check-rules.md` | Any C++ generation, review, or refactor. | Traceability/source inventory unless a rule question appears. |
| V2 | Targeted sections of `docs/vehicle-embedded-linux-cpp-coding-standard.md` | Non-trivial product design, cross-module behavior, rule interpretation. | Raw OCR and full source documents. |
| V3 | Source map, traceability, raw source inventory | Standards updates, source coverage, rule conflicts, audit mapping. | Full raw files without a search target. |
| V4 | Targeted search results in `docs/raw/` | Deep reconstruction, source-backed rule mapping, or audit evidence. | Whole raw archive reads. |

Token policy:

- Search before reading large files.
- Prefer local rule IDs over quoted source text.
- Treat aggregate references as higher signal than OCR output.
- Keep ordinary AI coding out of `docs/raw/`.
- Use final responses shaped as `Changed / Verified / Notes`.

## 3. Source Priority

For vehicle embedded Linux C++ rules, resolve conflicts in this order:

1. Laws, customer requirements, safety/security plans, and project approvals.
2. AUTOSAR C++14, MISRA C++, SEI CERT C/C++ intent, and customer-approved
   static-analysis profiles.
3. `docs/vehicle-embedded-linux-cpp-coding-standard.md`.
4. Module design docs, public interfaces, tests, and existing local style.
5. C++ Core Guidelines, Google C++ Style Guide, HICPP, ESCR, ANSSI, Barr-C, and
   other supplemental references.
6. Raw OCR content only as searchable evidence, never as direct normative text.

Use `docs/references/raw-source-inventory.md` to decide whether a local source
is authoritative, curated, supplemental, OCR-only, or licensed/redistribution
constrained.

## 4. AI Coding Workflow

Every product C++ task follows this flow:

1. Understand scope: requested behavior, explicit non-goals, likely files,
   affected ownership/lifetime/threading/error/security/Linux boundaries.
2. Route context: use V0/V1 first; move to V2/V3/V4 only when needed.
3. Find local patterns: search touched modules before inventing helper APIs,
   abstractions, wrappers, or error types.
4. Make the smallest product-grade change: preserve ABI, build system, language
   standard, public APIs, and unrelated formatting.
5. Verify: run the narrowest meaningful checks, then broaden when shared
   behavior was touched.
6. Report compactly: changed files, verification evidence, skipped checks with
   reasons, and residual risk.

## 5. Product-Grade Code Bar

Generated C++ must satisfy the project baseline:

- C++14 by default.
- RAII for every resource.
- Explicit ownership, lifetime, blocking, threading, error, and time-unit
  contracts.
- Bounded memory, containers, waits, retries, logs, queues, and allocations.
- Explicit `Result`/`Status`/typed-error style failures.
- Testable Linux/POSIX adapters around system boundaries.
- Deterministic tests for success, boundary, failure, timeout, stop-path, and
  resource-exhaustion behavior when relevant.

P0 rejections include exceptions as normal error handling, RTTI dependency,
naked `new/delete`, C-style casts, `system`/`popen`, global mutable state,
default-capture lambdas, detached threads, unbounded growth/waits/retries, and
unwrapped POSIX resources in business logic.

## 6. Review Gates

Use these gates before accepting AI output:

| Gate | Required Question |
| :--- | :--- |
| Scope | Does every changed line trace to the task? |
| Interface | Are ownership, lifetime, blocking, thread safety, errors, and time units explicit? |
| Resource | Does RAII cover memory, fd/socket/timer/epoll, locks, threads, and cleanup? |
| Error | Are fallible calls, `errno`, short I/O, `EINTR`, timeouts, and resource exhaustion handled? |
| Determinism | Are waits, retries, queues, logs, allocations, and containers bounded? |
| Security | Are external inputs validated and secrets kept out of logs/config/core dumps? |
| Testability | Can time, I/O, random, environment, hardware, and POSIX boundaries be replaced in tests? |
| Verification | Did build/test/format/static analysis run, or is the missing evidence explained? |

## 7. Maintenance Workflow

When standards or source material changes:

1. Update source metadata in `docs/references/raw-source-inventory.md`.
2. Update official/source access notes in
   `docs/references/vehicle-embedded-linux-cpp-standards-sources.md`.
3. Update coverage and graph links in
   `docs/references/vehicle-embedded-linux-cpp-standards-traceability.md`.
4. Add or revise enforceable rules in
   `docs/vehicle-embedded-linux-cpp-coding-standard.md` only when source intent
   changes a project rule, review gate, tool gate, or AI-output constraint.
5. Keep `docs/ai-vibe-coding-cpp-check-rules.md` compact and operational.

Do not copy long source excerpts into project standards. Summarize project
policy in original wording and cite local source nodes.

## 8. Prompt Contracts

Small implementation:

```text
Use docs/ai-vibe-coding-framework.md,
docs/ai-coding-product-grade-handbook.md, and
docs/ai-vibe-coding-cpp-check-rules.md. Stay within V0/V1 unless a rule question
requires V2+. Implement the smallest product-grade C++14 change, update tests,
run relevant checks, and report Changed / Verified / Notes.
```

Code review:

```text
Review against docs/ai-vibe-coding-framework.md and
docs/ai-vibe-coding-cpp-check-rules.md. Lead with P0/P1 defects and concrete
behavioral risks. Cite local rule IDs; do not restate raw source text.
```

Standards update:

```text
Read the framework, main standard, source map, traceability index, and raw
source inventory. Search docs/raw only for targeted source evidence. Update
enforceable rules and indexes without editing docs/raw or skills.
```

## 9. Completion Evidence

A task is not complete until there is evidence for:

- Correct files changed and protected folders untouched.
- Local Markdown links resolve.
- No obvious placeholders remain.
- `git diff --check` or equivalent whitespace validation passes.
- Required build/test/format/static-analysis commands ran, or the reason for
  skipping them is explicit.
