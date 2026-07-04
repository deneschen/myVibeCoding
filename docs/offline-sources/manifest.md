# Offline Standards Archive Manifest

- Last curated at: 2026-07-04
- Source map: `docs/references/vehicle-embedded-linux-cpp-standards-sources.md`
- Local Markdown references: `md/`
- Original PDFs: `pdf/`

This manifest lists only files that exist in the current offline archive. Web
page captures that were too noisy for AI context were removed after review.

## Primary Local Markdown References

- C++ Core Guidelines:
  - Source: https://raw.githubusercontent.com/isocpp/CppCoreGuidelines/master/CppCoreGuidelines.md
  - Local file: `md/cpp-core-guidelines.md`
  - Quality: primary source, high signal, complete upstream Markdown with local archive header.
- Google C++ Style Guide:
  - Source: https://google.github.io/styleguide/cppguide.html
  - Local file: `md/google-cpp-style-guide.md`
  - Quality: primary style source, high signal, converted from public page.
- SEI CERT C++ consolidated reference:
  - Current entry: https://cmu-sei.github.io/secure-coding-standards/sei-cert-cpp-coding-standard/
  - 2016 asset page: https://resources.sei.cmu.edu/library/asset-view.cfm?assetID=494932
  - Download form: https://resources.sei.cmu.edu/forms/secure-coding-cpp-form.cfm
  - Legacy direct PDF URL response: https://resources.sei.cmu.edu/downloads/secure-coding/assets/sei-cert-cpp-coding-standard-2016-v01.pdf
  - Local file: `md/sei-cert-cpp-reference.md`
  - Quality: curated reference, high signal, not full SEI rule text.

## Original PDFs

- AUTOSAR C++14 Guidelines PDF:
  - Source: https://www.autosar.org/fileadmin/standards/R19-03/AP/AUTOSAR_RS_CPP14Guidelines.pdf
  - Local file: `pdf/autosar-cpp14-guidelines.pdf`
  - Quality: original PDF, primary automotive C++14 safety subset source.

## Not Retained Or Not Downloaded

- AUTOSAR Adaptive Platform and document-search web captures were not retained
  because the authoritative local artifact is the AUTOSAR C++14 PDF.
- MISRA C++ standard PDFs were not downloaded because MISRA standards are
  licensed publications. Keep official links and access notes in the source map.
- MISRA public website/shop pages were removed because they were low-signal
  navigation and purchase pages, not rule content.
- Google C++ Style Guide Chinese translation page was removed because the
  archived page was a low-signal index page, not the full translated guide.
- SEI CERT C++ registration and asset pages were consolidated into
  `md/sei-cert-cpp-reference.md`; source capture files were removed.

## Maintenance

- Do not reference local files in this manifest unless they exist.
- Keep licensed standards as links and access notes unless the project has
  explicit redistribution rights.
- Prefer official upstream URLs over mirrors.
