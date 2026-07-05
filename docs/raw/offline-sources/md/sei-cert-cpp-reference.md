# SEI CERT C++ Consolidated Reference

- Consolidated at: 2026-07-04
- Purpose: high-signal local reference for rebuilding the vehicle embedded
  Linux C++ coding standard.
- Rights note: This file is an original consolidation of public SEI/CERT page
  metadata and access notes. It is not a copy of the SEI CERT C++ rule text.

## Source URLs

- Current online standard entry:
  https://cmu-sei.github.io/secure-coding-standards/sei-cert-cpp-coding-standard/
- 2016 Edition asset page:
  https://resources.sei.cmu.edu/library/asset-view.cfm?assetID=494932
- 2016 Edition registration/download form:
  https://resources.sei.cmu.edu/forms/secure-coding-cpp-form.cfm
- Legacy direct PDF URL response:
  https://resources.sei.cmu.edu/downloads/secure-coding/assets/sei-cert-cpp-coding-standard-2016-v01.pdf

## What This File Is

This document merges the useful information from the local SEI CERT C++ source
captures and removes page chrome, registration controls, unrelated footer
content, attachment noise, and duplicated registration-page text.

Use this file as the primary SEI CERT C++ local reference for AI-assisted
rewrites of the project C++ coding standard. Use the source URLs above for
fresh verification when network access is available.

## What This File Is Not

This file is not the full SEI CERT C++ Coding Standard and does not reproduce
the rule text. It is a curated source summary and link map. For audit work,
rule-level interpretation, or compliance evidence, consult the official SEI
pages and the official 2016 Edition PDF after completing the SEI download flow.

## Current Online Standard Status

The current SEI CERT C++ online standard entry is a development-facing site.
It states that the C++ rules reflect current secure-coding-community thinking
and that mature material is released in official report or book form. The page
also notes that the C++ standard currently exposes no recommendations.

For project use, treat the current site as:

- A live index for SEI CERT C++ rule areas.
- A link source for rule category pages and errata.
- A security taxonomy input, not a standalone complete offline rule corpus.

## 2016 Edition Status

The 2016 Edition is titled:

`SEI CERT C++ Coding Standard: Rules for Developing Safe, Reliable, and Secure Systems in C++ (2016 Edition)`

The SEI asset and registration pages describe the 2016 Edition as a free online
download intended to promote adoption of secure coding practices for C and C++.
The current download path requires completing the SEI registration form. The
old direct PDF URL now returns the registration/download response rather than a
standalone PDF file.

The 2016 Edition is relevant because it is the official release artifact
referenced by the SEI pages. If the project needs precise CERT rule text, do
not infer it from this summary. Download the official PDF through SEI and store
it according to the project's licensing and redistribution policy.

## Rule Category Map

The current online SEI CERT C++ entry lists these rule categories:

| Category | Area | Vehicle Embedded C++ relevance |
| --- | --- | --- |
| DCL | Declarations and Initialization | Initialization safety, declaration clarity, object state validity. |
| EXP | Expressions | Undefined behavior, expression side effects, order of evaluation. |
| INT | Integers | Overflow, conversion, signedness, range checks, protocol fields. |
| CTR | Containers | Iterator validity, bounds, ownership, capacity and resource control. |
| STR | Characters and Strings | Input validation, encoding, length checks, termination, injection risks. |
| MEM | Memory Management | Lifetime, allocation failure, ownership, leaks, double-free prevention. |
| FIO | Input Output | File handling, path safety, permissions, short I/O, resource cleanup. |
| ERR | Exceptions and Error Handling | Failure propagation, exception boundaries, recovery and diagnostics. |
| OOP | Object Oriented Programming | Inheritance safety, virtual dispatch, construction/destruction hazards. |
| CON | Concurrency | Data races, synchronization, thread lifecycle, deadlock avoidance. |
| MSC | Miscellaneous | Cross-cutting secure-coding practices and rule catch-all areas. |

## Security Themes To Feed Into The Project C++ Standard

For a vehicle embedded Linux C++ coding standard, map the SEI CERT C++ material
into these enforceable project themes:

- Avoid undefined and implementation-defined behavior in arithmetic, casts,
  object lifetime, initialization, aliasing, and expression evaluation.
- Treat all external input as untrusted, including network, IPC, diagnostics,
  configuration, persistence, OTA, environment variables, and device data.
- Validate integer ranges before conversion, allocation, indexing, serialization,
  deserialization, and timeout calculations.
- Enforce explicit ownership and lifetime. Prefer RAII and project-approved
  smart-pointer patterns over manual resource management.
- Bound memory, container, queue, parser, retry, and logging growth to prevent
  denial-of-service behavior in long-running services.
- Convert platform failures into project error domains. Do not ignore return
  values from system calls, library calls, parsing functions, lock operations,
  or persistence operations.
- Define exception policy at module boundaries. For this project, product code
  defaults to no exception-based error propagation; third-party exceptions must
  be caught and converted at approved boundaries.
- Keep file, path, process, and I/O operations behind wrappers that enforce
  permissions, ownership, cleanup, error mapping, and testability.
- Make concurrency ownership explicit: thread owner, stop protocol, join path,
  locking order, callback execution context, and timeout behavior.
- Keep object-oriented designs conservative: stable interfaces, virtual
  destructors where polymorphism is used, no construction/destruction surprises,
  and no inheritance when composition is sufficient.

## AI Usage Guidance

When using this SEI reference with Copilot, Codex, Claude, or another AI coding
tool, give the model these constraints:

- Use this file for secure-coding themes and category coverage.
- Do not invent SEI rule IDs or quote SEI rule text unless the official rule
  page or PDF is available in the current context.
- Prefer project-specific enforceable rules over broad security slogans.
- For vehicle embedded Linux C++, translate SEI themes into concrete checks:
  ownership, initialization, integer bounds, input validation, error handling,
  path safety, resource limits, and concurrency safety.
- If a requested rule needs exact SEI wording, stop and ask for the official
  source rather than reconstructing it from memory.

## Access And Licensing Notes

- The current online SEI CERT C++ entry is public and should be cited by URL.
- The 2016 Edition is described by SEI as a free online download, but the
  current flow requires registration.
- The legacy direct PDF URL should not be treated as a PDF source unless the
  downloaded response has a `%PDF-` file header.
- This repository keeps summaries and links, not copied full rule text.

## Links Worth Keeping

- Current SEI CERT C++ entry:
  https://cmu-sei.github.io/secure-coding-standards/sei-cert-cpp-coding-standard/
- Rule index:
  https://cmu-sei.github.io/secure-coding-standards/sei-cert-cpp-coding-standard/rules
- Errata for SEI CERT C++ Coding Standard 2016 Edition:
  https://cmu-sei.github.io/secure-coding-standards/sei-cert-cpp-coding-standard/errata-for-sei-cert-cpp-coding-standard-2016-edition
- DCL:
  https://cmu-sei.github.io/secure-coding-standards/sei-cert-cpp-coding-standard/rules/declarations-and-initialization-dcl/
- EXP:
  https://cmu-sei.github.io/secure-coding-standards/sei-cert-cpp-coding-standard/rules/expressions-exp/
- INT:
  https://cmu-sei.github.io/secure-coding-standards/sei-cert-cpp-coding-standard/rules/integers-int/
- CTR:
  https://cmu-sei.github.io/secure-coding-standards/sei-cert-cpp-coding-standard/rules/containers-ctr/
- STR:
  https://cmu-sei.github.io/secure-coding-standards/sei-cert-cpp-coding-standard/rules/characters-and-strings-str/
- MEM:
  https://cmu-sei.github.io/secure-coding-standards/sei-cert-cpp-coding-standard/rules/memory-management-mem/
- FIO:
  https://cmu-sei.github.io/secure-coding-standards/sei-cert-cpp-coding-standard/rules/input-output-fio/
- ERR:
  https://cmu-sei.github.io/secure-coding-standards/sei-cert-cpp-coding-standard/rules/exceptions-and-error-handling-err/
- OOP:
  https://cmu-sei.github.io/secure-coding-standards/sei-cert-cpp-coding-standard/rules/object-oriented-programming-oop/
- CON:
  https://cmu-sei.github.io/secure-coding-standards/sei-cert-cpp-coding-standard/rules/concurrency-con/
- MSC:
  https://cmu-sei.github.io/secure-coding-standards/sei-cert-cpp-coding-standard/rules/miscellaneous-msc/
- 2016 Edition asset page:
  https://resources.sei.cmu.edu/library/asset-view.cfm?assetID=494932
- 2016 Edition registration/download form:
  https://resources.sei.cmu.edu/forms/secure-coding-cpp-form.cfm

## Removed During Consolidation

The following low-signal content from the source captures was intentionally
not carried forward:

- Global navigation menus.
- Search UI text.
- Registration form controls and required-field labels.
- Footer links unrelated to the standard.
- External sharing URLs.
- Image attachment names.
- Duplicate text from the legacy PDF URL response and the registration form.
