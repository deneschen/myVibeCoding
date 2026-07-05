# Vehicle Embedded Linux C++ Coding Standard Sources

This file is the stable source map for repeatedly rebuilding and improving the
project C++ coding standard with AI tools such as VS Code Copilot, Codex, and
Claude.

Use these links as references. Do not copy paid or copyrighted standard text
verbatim. Summarize principles, cite the source, and express project-specific
rules in original wording.

## Offline Archive

Offline copies generated from this source map are stored under:

- Compact AI coding checklist: `docs/ai-vibe-coding-cpp-check-rules.md`
- Framework: `docs/ai-vibe-coding-framework.md`
- Raw source inventory: `docs/references/raw-source-inventory.md`
- Protected raw source archive: `docs/raw/offline-sources/`
- Curated raw Markdown references: `docs/raw/offline-sources/md/`
- OCR/PDF-derived raw Markdown references: `docs/raw/offline-sources/pdf/`
- Coverage and Obsidian graph index:
  `docs/references/vehicle-embedded-linux-cpp-standards-traceability.md`

Original web-page source files are intentionally not retained after conversion.
The raw source inventory lists only local files that currently exist. Do not
modify `docs/raw/` or `skills/` when updating this source map.

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
- Local curated aggregate: `docs/raw/offline-sources/md/sei-cert-cpp-secure-coding-aggregate.md`
- Local 2016 OCR: `docs/raw/offline-sources/pdf/sei-cert-cpp-2016.pdf_by_PaddleOCR-VL-1.6.md`
- AI usage note: Use as the secure coding baseline for undefined behavior,
  integer safety, memory management, error handling, concurrency, I/O, and
  object-oriented misuse.

### SEI CERT C Coding Standard

- Name: SEI CERT C Coding Standard
- Current source family: https://cmu-sei.github.io/secure-coding-standards/sei-cert-c-coding-standard/
- GitHub organization/source entry: https://github.com/cmu-sei/secure-coding-standards
- Local curated aggregate: `docs/raw/offline-sources/md/sei-cert-c-secure-coding-aggregate.md`
- Local 2016 OCR: `docs/raw/offline-sources/pdf/sei-cert-c-2016.pdf_by_PaddleOCR-VL-1.6.md`
- AI usage note: Use for C API, POSIX, signal, environment, file I/O, memory,
  strings, and trust-boundary rules that affect C++ Linux boundary wrappers.

## Supplemental Embedded And Secure Coding References

### HICPP

- Name: High Integrity C++ Coding Standard Version 4.0
- Source entry from local OCR metadata: https://www.codingstandard.com/
- Local OCR: `docs/raw/offline-sources/pdf/hi-cpp-4.0.pdf_by_PaddleOCR-VL-1.6.md`
- AI usage note: Supplemental C++ safety and static-enforceability reference.
  Use for rule conflicts or standards strengthening, not as everyday context.

### ESCR C++

- Name: Embedded System development Coding Reference guide, C++ Language Edition
- Publisher: IPA/SEC, Japan
- Local OCR: `docs/raw/offline-sources/pdf/escr-cpp-3.0.pdf_by_PaddleOCR-VL-1.6.md`
- AI usage note: Supplemental embedded C++ quality reference. The local file
  name says `3.0`, while the document text identifies ESCR C++ Ver. 2.0; record
  that caveat in audits.

### ESCR C

- Name: Embedded System development Coding Reference guide, C Language Edition
- Publisher: IPA/SEC, Japan
- Local OCR: `docs/raw/offline-sources/pdf/escr-c-3.0.pdf_by_PaddleOCR-VL-1.6.md`
- AI usage note: Supplemental embedded C quality reference for C/POSIX boundary
  and low-level wrapper rules.

### ANSSI Secure C

- Name: Rules for secure C language software development
- Publisher: ANSSI
- Source entry from local OCR metadata: https://www.ssi.gouv.fr/en/
- Local OCR: `docs/raw/offline-sources/pdf/anssi-fr-c-v1.4.pdf_by_PaddleOCR-VL-1.6.md`
- AI usage note: Supplemental secure C reference for undefined behavior,
  preprocessor/macros, compiler hardening, inputs, memory, and sensitive data.

### Barr-C

- Name: Barr-C:2018 Embedded C Coding Standard
- Source entry from local OCR metadata: https://barrgroup.com/coding-standard
- Local OCR: `docs/raw/offline-sources/pdf/barr-c-2018.pdf_by_PaddleOCR-VL-1.6.md`
- AI usage note: Supplemental embedded C style and readability reference. Observe
  license constraints; do not copy rule text into project standards.

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
Read docs/references/vehicle-embedded-linux-cpp-standards-sources.md and
docs/references/vehicle-embedded-linux-cpp-standards-traceability.md first.
Read docs/references/raw-source-inventory.md before searching docs/raw.
Rebuild the vehicle embedded Linux C++ coding standard from the listed sources.
Target product-grade automotive code. Prioritize AUTOSAR C++14, MISRA C++,
SEI CERT C/C++, C++ Core Guidelines, Google C++ Style Guide, HICPP, ESCR,
ANSSI, and Barr-C in that order unless a customer requirement says otherwise.
Do not copy paid or copyrighted standard text verbatim. Express enforceable
project rules in original wording. Include rules that constrain AI-generated
code style and allowed C++ features. Include VS Code Copilot, Codex, and Claude
usage instructions and review checklists. Use the traceability matrix to avoid
duplicating source material that is already covered by project rules. Keep the
compact AI vibe coding checklist aligned with the main standard so common code
tasks do not need to read the full source archive.
```

## Maintenance Rules

- Prefer official URLs over mirrors, blogs, and copied PDFs.
- Record access constraints, version numbers, and redirects.
- Keep links stable and human-readable.
- If a referenced standard is licensed, summarize only project policy and rule
  intent; require the licensed document for audits.
- When adding sources, explain how they affect enforceable project rules.
- Update the raw source inventory and traceability index whenever source
  inventory or rule coverage changes.
- Keep `docs/ai-vibe-coding-cpp-check-rules.md` short and operational; it
  should point to the main standard instead of duplicating full rule text.
- Do not modify `docs/raw/` or `skills/` while maintaining the framework.
