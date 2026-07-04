# Codex Instructions

Before generating or editing C++ code in this repository, read:

1. `docs/ai-vibe-coding-cpp-check-rules.md`
2. `docs/vehicle-embedded-linux-cpp-coding-standard.md`
3. `.clang-format`, `.clang-tidy`, and the touched module files/tests.

Read `docs/references/vehicle-embedded-linux-cpp-standards-sources.md` and
`docs/references/vehicle-embedded-linux-cpp-standards-traceability.md` only
when updating the standard, resolving rule conflicts, or answering source
coverage questions.

For vehicle embedded Linux C++ code:

- Default to C++14.
- Produce product-grade code, not demo code.
- Use RAII, explicit ownership, bounded resource use, deterministic tests, and
  explicit `Result`/`Status` style error handling.
- Do not introduce exceptions, RTTI, naked `new/delete`, C-style casts,
  `system`/`popen`, global mutable state, default-capture lambdas, detached
  threads, or unbounded containers/retry loops.
- Wrap Linux/POSIX calls behind testable RAII adapters.
- Follow repository formatting and static-analysis configuration.
- Update or add tests with code changes.
- Report build, test, formatting, and static-analysis verification. If a check
  cannot be run, state that clearly.
- Save tokens by using targeted searches and local rule IDs instead of reading
  or restating large offline source files.
