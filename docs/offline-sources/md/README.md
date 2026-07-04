# Offline Markdown Source Quality

This directory contains only local Markdown files that are useful as AI context
for rebuilding the vehicle embedded Linux C++ coding standard.

## Primary Sources

- `cpp-core-guidelines.md`
  - Source: C++ Core Guidelines upstream Markdown.
  - Quality: high.
  - Use for ownership, lifetime, interfaces, RAII, error handling,
    concurrency, type safety, and general C++ design guidance.

- `google-cpp-style-guide.md`
  - Source: Google C++ Style Guide public page.
  - Quality: high.
  - Use for naming, formatting, comments, header hygiene, include discipline,
    readability, and practical C++ style decisions.

## Curated References

- `sei-cert-cpp-reference.md`
  - Source: consolidated SEI CERT C++ public entry, 2016 Edition asset page,
    download-form status, and rule-category links.
  - Quality: high as a reference summary.
  - Use for secure-coding themes and category coverage. Do not treat it as full
    SEI CERT rule text.

## Sources Kept Outside Markdown

- AUTOSAR C++14 is kept as original PDF:
  `../pdf/autosar-cpp14-guidelines.pdf`.
- MISRA C++ standards are licensed publications. Keep official links in the
  source map and require licensed documents for audits.

## Excluded Material

Low-signal page captures were removed after review. Examples include website
navigation pages, shop pages, registration forms, web search pages, external
sharing links, and page footers.
