# Claude Instructions

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
- Save tokens by following the handbook context ladder, using targeted searches,
  and citing local rule IDs instead of reading or restating large offline source
  files.
