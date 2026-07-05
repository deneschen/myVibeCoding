# Codex Instructions

Before generating or editing C++ code in this repository, use the context ladder
in `docs/ai-vibe-coding-framework.md`. Start with:

1. `docs/ai-vibe-coding-framework.md`
2. `docs/ai-coding-product-grade-handbook.md`
3. `docs/ai-vibe-coding-cpp-check-rules.md`
4. `.clang-format`, `.clang-tidy`, and the touched module files/tests.

Read `docs/vehicle-embedded-linux-cpp-coding-standard.md` when generating
non-trivial product C++ code, reviewing cross-cutting behavior, resolving a rule
question, or when the compact rules do not give enough detail.

Read `docs/references/vehicle-embedded-linux-cpp-standards-sources.md` and
`docs/references/vehicle-embedded-linux-cpp-standards-traceability.md` only
when updating the standard, resolving rule conflicts, or answering source
coverage questions.

Do not modify `docs/raw/` or `skills/`. Treat `docs/raw/` as a read-only raw
source archive and `skills/` as a read-only workflow archive.

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
- Save tokens by following the handbook context ladder, using targeted searches,
  and citing local rule IDs instead of reading or restating large offline source
  files.
