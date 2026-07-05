# GitHub Copilot Instructions

This repository targets product-grade vehicle embedded Linux C++.

Before suggesting C++ code, follow the context ladder in:

- `docs/ai-vibe-coding-framework.md`
- `docs/ai-coding-product-grade-handbook.md`

Start with:

- `docs/ai-vibe-coding-cpp-check-rules.md`
- `.clang-format` and `.clang-tidy`

Read `docs/vehicle-embedded-linux-cpp-coding-standard.md` when generating
non-trivial product C++ code, reviewing cross-cutting behavior, resolving a rule
question, or when compact rules do not give enough detail.

Use `docs/references/vehicle-embedded-linux-cpp-standards-sources.md` and
`docs/references/vehicle-embedded-linux-cpp-standards-traceability.md` only for
standards updates, rule conflicts, or source coverage questions.

Do not modify `docs/raw/` or `skills/`. Treat `docs/raw/` as a read-only raw
source archive and `skills/` as a read-only workflow archive.

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
- Save context by using the handbook context ladder, targeted searches, and
  local rule IDs instead of restating large standards sources.
