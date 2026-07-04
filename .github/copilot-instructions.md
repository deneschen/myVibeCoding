# GitHub Copilot Instructions

This repository targets product-grade vehicle embedded Linux C++.

Before suggesting C++ code, follow:

- `docs/vehicle-embedded-linux-cpp-coding-standard.md`
- `docs/references/vehicle-embedded-linux-cpp-standards-sources.md`
- `.clang-format` and `.clang-tidy`

Hard constraints:

- Default to C++14.
- Use RAII, explicit ownership, bounded containers, monotonic time, and
  `Result`/`Status` style error returns.
- Do not suggest exceptions, RTTI, naked `new/delete`, C-style casts,
  `system`/`popen`, global mutable state, default-capture lambdas, detached
  threads, or unwrapped POSIX calls.
- Include tests for new behavior.
- Keep changes local to the requested task.
- Follow repository formatting and static-analysis configuration.
