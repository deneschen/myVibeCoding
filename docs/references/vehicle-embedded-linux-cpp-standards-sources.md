# Vehicle Embedded Linux C++ Coding Standard Sources

This file is the stable source map for repeatedly rebuilding and improving the
project C++ coding standard with AI tools such as VS Code Copilot, Codex, and
Claude.

Use these links as references. Do not copy paid or copyrighted standard text
verbatim. Summarize principles, cite the source, and express project-specific
rules in original wording.

## Offline Archive

Offline copies generated from this source map are stored under:

- Manifest: `docs/offline-sources/manifest.md`
- Curated local Markdown references: `docs/offline-sources/md/`
- Original PDFs: `docs/offline-sources/pdf/`

Original web-page source files are intentionally not retained after conversion.
The offline manifest lists only local files that currently exist.

## User-Provided Raw Links

Keep this section unchanged unless the user provides a replacement. These are
the exact links supplied when the source map was created.

- AUTOSAR Adaptive Platform Standards:
  https://www.google.com/url?sa=E&q=https%3A%2F%2Fwww.autosar.org%2Fstandards%2Fadaptive-platform
- MISRA Website:
  https://www.google.com/url?sa=E&q=https%3A%2F%2Fwww.misra.org.uk%2F
- CERT C++ Coding Standard Wiki:
  https://www.google.com/url?sa=E&q=https%3A%2F%2Fwiki.sei.cmu.edu%2Fconfluence%2Fpages%2Fviewpage.action%3FpageId%3D88046682
- C++ Core Guidelines:
  https://www.google.com/url?sa=E&q=https%3A%2F%2Fisocpp.github.io%2FCppCoreGuidelines%2FCppCoreGuidelines
- Google C++ Style Guide, Chinese translation:
  https://www.google.com/url?sa=E&q=https%3A%2F%2Fzh-google-styleguide.readthedocs.io%2Fen%2Flatest%2Fgoogle-cpp-styleguide%2F
- Google C++ Style Guide, English original:
  https://www.google.com/url?sa=E&q=https%3A%2F%2Fgoogle.github.io%2Fstyleguide%2Fcppguide.html

## Official Safety And Security Standards

### AUTOSAR C++14 Guidelines

- Name: Guidelines for the use of the C++14 language in critical and safety-related systems
- Official entry: https://www.autosar.org/standards/adaptive-platform
- Official document search: https://www.autosar.org/search
- Search keywords: `Guidelines for the use of the C++14 language in critical and safety-related systems`
- Known document metadata: `R19-03`, `AP`, `RS`, `AUTOSAR_RS_CPP14Guidelines.pdf`
- AI usage note: Treat as the primary automotive C++14 safety subset reference.
  Prefer rule intent and project policy summaries over copying AUTOSAR text.

### MISRA C++

- Name: MISRA C++ coding guidelines
- Official site: https://misra.org.uk/
- Publications: https://misra.org.uk/publications/
- Shop / PDF copies: https://misra.org.uk/shop/
- AI usage note: MISRA documents are usually paid/licensed. Do not reproduce
  rule text. Capture compliance process requirements: deviations, tool checks,
  traceability, code review evidence, and static analysis gates.

### SEI CERT C++ Coding Standard

- Name: SEI CERT C++ Coding Standard
- Official wiki: https://wiki.sei.cmu.edu/confluence/pages/viewpage.action?pageId=88046682
- Current redirected site: https://cmu-sei.github.io/secure-coding-standards/sei-cert-cpp-coding-standard/
- GitHub organization/source entry: https://github.com/cmu-sei/secure-coding-standards
- AI usage note: Use as the secure coding baseline for undefined behavior,
  integer safety, memory management, error handling, concurrency, I/O, and
  object-oriented misuse.

## General C++ Design And Style References

### C++ Core Guidelines

- Name: C++ Core Guidelines
- Official site: https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines
- GitHub repository: https://github.com/isocpp/CppCoreGuidelines
- AI usage note: Use for modern C++ design intent, ownership, lifetime,
  resource management, type safety, interfaces, error handling, and concurrency.
  Project rules may be stricter than the Core Guidelines for embedded safety.

### Google C++ Style Guide

- Name: Google C++ Style Guide
- English original: https://google.github.io/styleguide/cppguide.html
- Source repository: https://github.com/google/styleguide
- Chinese translation: https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/
- AI usage note: Use mainly for naming, formatting, include organization,
  header hygiene, API clarity, comments, and code readability. Do not import
  Google-specific build or ownership assumptions blindly.

## Project-Specific Rebuild Prompt

When asking an AI model to update the C++ coding standard, include this prompt:

```text
Read docs/references/vehicle-embedded-linux-cpp-standards-sources.md first.
Rebuild the vehicle embedded Linux C++ coding standard from the listed sources.
Target product-grade automotive code. Prioritize AUTOSAR C++14, MISRA C++,
SEI CERT C++, C++ Core Guidelines, and Google C++ Style Guide in that order.
Do not copy paid or copyrighted standard text verbatim. Express enforceable
project rules in original wording. Include rules that constrain AI-generated
code style and allowed C++ features. Include VS Code Copilot, Codex, and Claude
usage instructions and review checklists.
```

## Maintenance Rules

- Prefer official URLs over mirrors, blogs, and copied PDFs.
- Record access constraints, version numbers, and redirects.
- Keep links stable and human-readable.
- If a referenced standard is licensed, summarize only project policy and rule
  intent; require the licensed document for audits.
- When adding sources, explain how they affect enforceable project rules.
