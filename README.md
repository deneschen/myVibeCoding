# vibe coding

## Documents

- [AI vibe coding 框架](docs/ai-vibe-coding-framework.md)
- [AI coding 产品级指导手册](docs/ai-coding-product-grade-handbook.md)
- [AI vibe coding C++ 检查规则](docs/ai-vibe-coding-cpp-check-rules.md)
- [车载嵌入式 Linux C++ 开发规范](docs/vehicle-embedded-linux-cpp-coding-standard.md)
- [规范来源索引](docs/references/vehicle-embedded-linux-cpp-standards-sources.md)
- [来源覆盖矩阵和 Obsidian 图谱](docs/references/vehicle-embedded-linux-cpp-standards-traceability.md)
- [raw 资料清单](docs/references/raw-source-inventory.md)

## Recommended AI Workflow

1. Start with [AI vibe coding 框架](docs/ai-vibe-coding-framework.md).
2. Use [AI coding 产品级指导手册](docs/ai-coding-product-grade-handbook.md)
   and [AI vibe coding C++ 检查规则](docs/ai-vibe-coding-cpp-check-rules.md)
   for everyday coding.
3. Read the full [车载嵌入式 Linux C++ 开发规范](docs/vehicle-embedded-linux-cpp-coding-standard.md)
   only when the compact rules do not give enough detail for a product-grade
   decision.
4. Use references and `docs/raw/` only for standards updates, source coverage,
   audit mapping, or rule conflicts. Do not modify `docs/raw/` or `skills/`.

## AI Coding Entrypoints

- Codex: [AGENTS.md](AGENTS.md)
- Claude: [CLAUDE.md](CLAUDE.md)
- VS Code Copilot: [.github/copilot-instructions.md](.github/copilot-instructions.md)

## Tooling

- Format: [.clang-format](.clang-format)
- Static analysis: [.clang-tidy](.clang-tidy)
- Editor defaults: [.editorconfig](.editorconfig)
