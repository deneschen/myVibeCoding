# Claude Instructions

Before generating or editing C++ code in this repository, read:

1. `docs/ai-vibe-coding-cpp-check-rules.md`
2. `docs/vehicle-embedded-linux-cpp-coding-standard.md`
3. `.clang-format`, `.clang-tidy`, and the touched module files/tests.

Read `docs/references/vehicle-embedded-linux-cpp-standards-sources.md` and
`docs/references/vehicle-embedded-linux-cpp-standards-traceability.md` only
when updating the standard, resolving rule conflicts, or answering source
coverage questions.

Follow the project automotive embedded Linux C++ subset:

- Default language standard: C++14.
- Use RAII, explicit ownership, bounded resource use, deterministic tests, and
  explicit error types.
- Avoid speculative abstractions and unrelated refactors.
- Do not generate exceptions, RTTI, naked `new/delete`, C-style casts,
  shell execution, global mutable state, detached threads, or unbounded runtime
  resource growth.
- Treat all external input as untrusted.
- Make POSIX boundaries testable through wrappers.
- Follow repository formatting and static-analysis configuration.
- Add or update tests for code changes and summarize verification evidence.
- Save tokens by using targeted searches and local rule IDs instead of reading
  or restating large offline source files.
