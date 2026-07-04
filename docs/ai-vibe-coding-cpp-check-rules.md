# AI Vibe Coding C++ Check Rules

Purpose: give Copilot, Codex, Claude, and other AI coding tools a compact,
high-signal checklist for producing product-grade vehicle embedded Linux C++
while avoiding unnecessary context and token consumption.

This document is the operational entry point. The full rule source of truth is
[vehicle-embedded-linux-cpp-coding-standard.md](vehicle-embedded-linux-cpp-coding-standard.md).
Source traceability is maintained in
[vehicle-embedded-linux-cpp-standards-traceability.md](references/vehicle-embedded-linux-cpp-standards-traceability.md).

## 1. Token Budget Policy

Use staged context. Do not load large offline sources by default.

| Stage | Read | Use When | Do Not Read Yet |
| :--- | :--- | :--- | :--- |
| T0 | This file, touched source/header/test files, `.clang-format`, `.clang-tidy` | Every C++ task | Offline source archives. |
| T1 | Main coding standard | Any product C++ generation or non-trivial review | Full AUTOSAR OCR, Core Guidelines, Google style guide. |
| T2 | Traceability index and source map | Standards updates, rule conflicts, source coverage questions | Full offline source text unless a specific topic requires it. |
| T3 | Specific offline source section found by targeted search | Deep rule reconstruction or audit mapping | Whole-file reads of large archives without a search target. |

Rules for saving tokens:

- Prefer targeted search over reading large files.
- Quote rule IDs and section names instead of copying long source text.
- Do not restate the full standard in answers; cite the relevant local rule.
- Do not summarize unrelated files.
- Do not inspect generated archives unless the task is about standards,
  traceability, or source reconstruction.

## 2. AI Work Contract

Before editing C++ code, the AI must identify:

- task scope and files likely to change;
- relevant project rules by section or rule ID;
- tests or checks that can verify the change;
- any unclear ownership, threading, lifetime, error, or platform behavior.

During editing, the AI must:

- keep the diff limited to the requested behavior;
- avoid unrelated formatting and opportunistic refactors;
- preserve ABI, language standard, build system, and public API unless the task
  explicitly requires a change;
- write product code, not demo code.

After editing, the AI must report:

- changed files;
- verification commands and outcomes;
- checks that could not run and why;
- residual risks or assumptions.

## 3. P0 Rejection Rules

Reject or rewrite AI-generated C++ that contains any of the following unless a
human-approved deviation exists:

- exceptions as normal error handling;
- RTTI dependency or `dynamic_cast`;
- naked `new`, naked `delete`, `malloc`, or `free` in business logic;
- C-style casts;
- unapproved `reinterpret_cast` or mutation through `const_cast`;
- `system`, `popen`, shell command construction, or untrusted command strings;
- global mutable state or hidden singleton state;
- detached threads;
- default-capture lambdas;
- unbounded queues, caches, retries, loops, waits, or container growth;
- ignored return values from fallible APIs;
- unwrapped POSIX file descriptor, socket, timer, epoll, process, or signal
  handling in business logic;
- logging of secrets, tokens, certificates, personal data, or sensitive vehicle
  identifiers.

## 4. P1 Justification Rules

Require a short design reason, tests, and review attention for:

- `std::shared_ptr` or shared ownership;
- dynamic allocation after startup or in hot paths;
- templates beyond simple type-safe utilities;
- operator overloads;
- inheritance and virtual dispatch;
- atomics with non-default memory ordering;
- background threads or async callbacks;
- direct path, permission, IPC, network, or persistence handling;
- floating-point control decisions;
- third-party library use or new build dependencies.

## 5. Product C++ Review Passes

Run these passes mentally before proposing or accepting code:

| Pass | Check |
| :--- | :--- |
| Interface | Ownership, lifetime, blocking behavior, thread safety, errors, and time units are explicit. |
| Memory | RAII owns every resource; no leaks, dangling references, hidden ownership, or uninitialized state. |
| Error | Failures return `Result`/`Status`/typed errors; `errno`, short I/O, `EINTR`, and resource exhaustion are handled. |
| Concurrency | Threads have owners and stop paths; shared data is synchronized; locks are simple and bounded. |
| Determinism | Hot paths avoid unbounded allocation, waits, retries, logs, and container growth. |
| Type Safety | Narrowing, sign conversion, overflow, enum/integer mixing, and bit operations are explicit and tested. |
| Linux Boundary | POSIX APIs are behind testable RAII wrappers; permissions, `CLOEXEC`, paths, and signals are controlled. |
| Security | All external input is bounded and validated; secrets never enter logs or normal config. |
| Testing | Boundary, failure-path, resource-exhaustion, timeout, and concurrency-stop cases are covered. |

## 6. Token-Efficient Prompt Patterns

Small implementation:

```text
Use docs/ai-vibe-coding-cpp-check-rules.md and the touched files only unless a
rule conflict appears. Implement the smallest product-grade C++14 change.
Apply P0/P1 checks, update tests, and report verification evidence.
```

Code review:

```text
Review this C++ change against docs/ai-vibe-coding-cpp-check-rules.md.
Lead with P0/P1 findings only. Do not restate the full standard. Cite local
rule IDs or sections when useful.
```

Standards update:

```text
Read the main standard, source map, traceability index, and only the targeted
offline source sections needed for this topic. Update enforceable rules without
duplicating source text.
```

Deep source reconstruction:

```text
Use targeted search inside offline-sources first. Open full large source files
only when the search result cannot answer the rule mapping or audit question.
```

## 7. Minimal AI Response Shape

For C++ coding tasks, keep final responses short:

```text
Changed: <files and behavior>
Verified: <commands and results>
Notes: <unverified checks, assumptions, residual risk>
```

Do not include full diffs, long source quotations, or broad standards summaries
unless the user explicitly asks.
