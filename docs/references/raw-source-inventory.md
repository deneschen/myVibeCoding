# Raw Source Inventory

Purpose: list the read-only raw and curated source material under `docs/raw/`
without requiring AI agents to open the raw archive for ordinary coding tasks.

`docs/raw/` is a protected folder for this project. Do not edit, normalize,
move, or regenerate files under it when updating the coding-standard framework.

## 1. Usage Rules

- Ordinary C++ coding tasks must not read raw sources by default.
- Standards updates and audits should start with this inventory, the source map,
  and the traceability index.
- Use targeted search before opening any large raw file.
- Prefer curated aggregate files over OCR files when both cover the same topic.
- Treat OCR output as searchable support material, not authoritative wording.
- Do not copy long source excerpts into project rules.

## 2. Curated Markdown References

| Source Node | Local File | Role | AI Priority |
| :--- | :--- | :--- | :--- |
| `CORE` | [cpp-core-guidelines.md](../raw/offline-sources/md/cpp-core-guidelines.md) | Upstream C++ Core Guidelines Markdown; ownership, lifetime, interfaces, RAII, errors, concurrency, type safety. | Targeted search for design-rule reconstruction. |
| `GOOGLE` | [google-cpp-style-guide.md](../raw/offline-sources/md/google-cpp-style-guide.md) | Google C++ Style Guide extraction; style, headers, naming, comments, formatting, readability. | Targeted search for style conflicts. |
| `CERT-REF` | [sei-cert-cpp-reference.md](../raw/offline-sources/md/sei-cert-cpp-reference.md) | Curated SEI CERT C++ access and category reference. | Prefer over OCR for quick CERT category orientation. |
| `CERT-C-AGG` | [sei-cert-c-secure-coding-aggregate.md](../raw/offline-sources/md/sei-cert-c-secure-coding-aggregate.md) | High-signal project-owned aggregation of CERT C rules and recommendations. | Prefer for C/POSIX boundary and C API rule families. |
| `CERT-CPP-AGG` | [sei-cert-cpp-secure-coding-aggregate.md](../raw/offline-sources/md/sei-cert-cpp-secure-coding-aggregate.md) | High-signal project-owned aggregation of CERT C++ rule families. | Prefer for C++ security rule families. |

## 3. OCR / PDF-Derived Markdown

| Source Node | Local File | Source Role | Quality / Use Note |
| :--- | :--- | :--- | :--- |
| `AUTOSAR-19-OCR` | [autosar-cpp14-guidelines.pdf_by_PaddleOCR-VL-1.6.md](../raw/offline-sources/pdf/autosar-cpp14-guidelines.pdf_by_PaddleOCR-VL-1.6.md) | AUTOSAR C++14 Guidelines R19-03 searchable OCR. | Primary local AUTOSAR OCR. Search helper only; verify against approved standard for audits. |
| `AUTOSAR-17-OCR` | [autosar-cpp14-2017.pdf_by_PaddleOCR-VL-1.6.md](../raw/offline-sources/pdf/autosar-cpp14-2017.pdf_by_PaddleOCR-VL-1.6.md) | AUTOSAR C++14 Guidelines 17-03 searchable OCR. | Historical baseline and change comparison support. |
| `HICPP-OCR` | [hi-cpp-4.0.pdf_by_PaddleOCR-VL-1.6.md](../raw/offline-sources/pdf/hi-cpp-4.0.pdf_by_PaddleOCR-VL-1.6.md) | High Integrity C++ Coding Standard v4.0 searchable OCR. | Supplemental safety/style reference, especially static enforceability and deviation concepts. |
| `ESCR-CPP-OCR` | [escr-cpp-3.0.pdf_by_PaddleOCR-VL-1.6.md](../raw/offline-sources/pdf/escr-cpp-3.0.pdf_by_PaddleOCR-VL-1.6.md) | IPA/SEC ESCR C++ coding reference searchable OCR. | File name says 3.0, content states ESCR C++ Ver. 2.0. Treat as C++ embedded quality reference with naming caveat. |
| `ESCR-C-OCR` | [escr-c-3.0.pdf_by_PaddleOCR-VL-1.6.md](../raw/offline-sources/pdf/escr-c-3.0.pdf_by_PaddleOCR-VL-1.6.md) | IPA/SEC ESCR C language embedded coding reference searchable OCR. | Useful for C/POSIX boundary and embedded C quality concepts. |
| `ANSSI-C-OCR` | [anssi-fr-c-v1.4.pdf_by_PaddleOCR-VL-1.6.md](../raw/offline-sources/pdf/anssi-fr-c-v1.4.pdf_by_PaddleOCR-VL-1.6.md) | ANSSI secure C language development rules searchable OCR. | Supplemental secure C reference, useful for macros, undefined behavior, inputs, memory, and toolchain hardening. |
| `BARR-C-OCR` | [barr-c-2018.pdf_by_PaddleOCR-VL-1.6.md](../raw/offline-sources/pdf/barr-c-2018.pdf_by_PaddleOCR-VL-1.6.md) | Barr-C Embedded C Coding Standard searchable OCR. | Supplemental embedded C style/reference material; observe license constraints. |
| `CERT-C-2016-OCR` | [sei-cert-c-2016.pdf_by_PaddleOCR-VL-1.6.md](../raw/offline-sources/pdf/sei-cert-c-2016.pdf_by_PaddleOCR-VL-1.6.md) | SEI CERT C 2016 searchable OCR. | Use only when aggregate file is insufficient or audit requires source-level detail. |
| `CERT-CPP-2016-OCR` | [sei-cert-cpp-2016.pdf_by_PaddleOCR-VL-1.6.md](../raw/offline-sources/pdf/sei-cert-cpp-2016.pdf_by_PaddleOCR-VL-1.6.md) | SEI CERT C++ 2016 searchable OCR. | Use only when aggregate/reference files are insufficient or audit requires source-level detail. |

## 4. Source-to-Rule Themes

| Theme | Preferred Sources | Main Project Rule Areas |
| :--- | :--- | :--- |
| Automotive C++14 safety subset | `AUTOSAR-19-OCR`, `AUTOSAR-17-OCR`, MISRA links in source map | Language baseline, restricted features, compliance, deviations, static analysis. |
| Secure C++ | `CERT-CPP-AGG`, `CERT-REF`, `CERT-CPP-2016-OCR` | Errors, lifetime, memory, containers, OOP, concurrency, AI P0/P1 checks. |
| Secure C/POSIX boundary | `CERT-C-AGG`, `CERT-C-2016-OCR`, `ANSSI-C-OCR` | C APIs, POSIX wrappers, signals, strings, buffers, environment, file I/O. |
| Embedded C/C++ quality | `ESCR-CPP-OCR`, `ESCR-C-OCR`, `BARR-C-OCR`, `HICPP-OCR` | Readability, deterministic behavior, module boundaries, comments, style, enforceability. |
| General C++ design | `CORE`, `GOOGLE` | RAII, interfaces, ownership, type safety, header hygiene, naming, formatting. |

## 5. Maintenance Checklist

When raw source material changes:

1. Update this inventory with file path, source role, quality note, and AI
   priority.
2. Update `vehicle-embedded-linux-cpp-standards-sources.md` if the official
   source list or usage guidance changes.
3. Update `vehicle-embedded-linux-cpp-standards-traceability.md` source nodes,
   coverage matrix, Obsidian links, and graph.
4. Update `docs/ai-vibe-coding-framework.md` only if the context-routing model
   changes.
5. Do not edit `docs/raw/` while performing the inventory update.
