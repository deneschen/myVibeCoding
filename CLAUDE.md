# Claude Instructions

Before generating or editing C++ code in this repository, read:

1. `docs/vehicle-embedded-linux-cpp-coding-standard.md`
2. `docs/references/vehicle-embedded-linux-cpp-standards-sources.md`
3. `.clang-format` and `.clang-tidy`

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
