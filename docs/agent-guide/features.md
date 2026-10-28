# PdfCraft — funcionalidades e módulos

Última atualização: 2026-10-28.

## Fluxo principal

| Etapa | Entradas | Responsabilidade e dependências | Saídas |
|---|---|---|---|
| Abrir e editar | Bytes PDF, árvore de objetos e pedidos de edição | [cos](../../crates/cos/), [content](../../crates/content/), [model](../../crates/model/), [engine](../../crates/engine/), [automation](../../crates/automation/) | Documento PDF com histórico e objetos preservados |
| Avaliar e produzir saída | Estado editado e parâmetros de saída | [render](../../crates/render/), [edit](../../crates/edit/), [forms](../../crates/forms/), [organize](../../crates/organize/), [export](../../crates/export/) | Páginas renderizadas, PDFs salvos e resultados de ferramentas |
| Acesso pelo usuário ou agente | Gestos, argumentos CLI ou chamadas estruturadas | [apps](../../apps/), [engine](../../crates/engine/) e interfaces descritas abaixo | Estado atualizado, resposta ou erro; comportamento detalhado nos contratos |

A tabela mostra o caminho conceitual, não uma garantia de suporte a todos os formatos. APIs, estruturas, unidades, efeitos colaterais e limites ficam na [arquitetura](../architecture.md), nos documentos de assunto e nos módulos indicados.

## Contratos e exemplos de uso

- [development.md](../development.md)
- [cli-batch.md](../cli-batch.md)
- [editing-existing-text.md](../editing-existing-text.md)
- [pdf-spec-parity.md](../pdf-spec-parity.md)
- [windows-renderer-startup.md](../windows-renderer-startup.md)
- [mcp.md](../mcp.md)

## Inventário de bibliotecas

Responsabilidades resumidas a partir dos manifestos e comentários de módulo do checkout. As dependências listadas são as internas diretas declaradas em `dependencies` (incluindo condicionais); não incluem dependências de teste/build nem expansão de features. O manifesto de cada linha é a fonte exata.

| Módulo / manifesto | Responsabilidade declarada | Dependências internas diretas |
|---|---|---|
| [a11y](../../crates/a11y/Cargo.toml) | Accessibility: the Accessibility Checker (32 rules), its report and fixes | `pdfcraft-content`, `pdfcraft-cos`, `pdfcraft-filters`, `pdfcraft-fonts`, `pdfcraft-model` |
| [annot](../../crates/annot/Cargo.toml) | Annotations (comments): builders, appearance streams and edits, ISO 32000-2 §12.5 | `pdfcraft-cos`, `pdfcraft-fonts`, `pdfcraft-model` |
| [audit](../../crates/audit/Cargo.toml) | Verification tooling: audit documents for faux redactions (opaque covers over extractable text) | `pdfcraft-content`, `pdfcraft-cos`, `pdfcraft-edit`, `pdfcraft-model` |
| [automation](../../crates/automation/Cargo.toml) | Agent control for PdfCraft: a headless tool set over the engine and an MCP server | `pdfcraft-cos`, `pdfcraft-engine`, `pdfcraft-organize`, `pdfcraft-platform`, `pdfcraft-render` |
| [compare](../../crates/compare/Cargo.toml) | Compare files: word-level text differences between two documents, grouped into changes with their places on the pages | Nenhuma declarada nesta categoria |
| [content](../../crates/content/Cargo.toml) | Content streams (ISO 32000-2 §7.8, §8–9): operators with source spans, inline images, serializer, matrices | `pdfcraft-cos` |
| [cos](../../crates/cos/Cargo.toml) | PDF object layer: lazy tolerant parsing, copy-on-write edits, incremental and full saving | `pdfcraft-crypt`, `pdfcraft-filters` |
| [create](../../crates/create/Cargo.toml) | Create PDFs: blank documents, from images (PNG, JPEG) and from plain text | `pdfcraft-cos`, `pdfcraft-fonts`, `pdfcraft-model` |
| [crypt](../../crates/crypt/Cargo.toml) | PDF standard security handler (ISO 32000-2 §7.6): RC4 and AES, revisions 2–6 | Nenhuma declarada nesta categoria |
| [edit](../../crates/edit/Cargo.toml) | Page content editing: headers and footers, watermarks, backgrounds, Bates numbers (ISO 32000-2 §14.8) | `pdfcraft-annot`, `pdfcraft-content`, `pdfcraft-cos`, `pdfcraft-fonts`, `pdfcraft-model` |
| [engine](../../crates/engine/Cargo.toml) | PdfCraft engine façade: session, documents, command/tool catalogue | `pdfcraft-a11y`, `pdfcraft-annot`, `pdfcraft-audit`, `pdfcraft-compare`, `pdfcraft-cos`, `pdfcraft-create`, `pdfcraft-edit`, `pdfcraft-export`, `pdfcraft-fonts`, `pdfcraft-forms`, `pdfcraft-geom`, `pdfcraft-js`, `pdfcraft-measure`, `pdfcraft-model`, `pdfcraft-ocr`, `pdfcraft-optimize`, `pdfcraft-organize`, `pdfcraft-preflight`, `pdfcraft-print`, `pdfcraft-redact`, `pdfcraft-render`, `pdfcraft-sign`, `pdfcraft-xfa`, `pdfcraft-xfdf` |
| [export](../../crates/export/Cargo.toml) | Export a PDF to HTML, Word (.docx) and RTF from its paragraphs and images | Nenhuma declarada nesta categoria |
| [filters](../../crates/filters/Cargo.toml) | PDF stream filters (ISO 32000-2 §7.4): Flate, LZW, predictors, ASCII85, ASCIIHex, RunLength | Nenhuma declarada nesta categoria |
| [fonts](../../crates/fonts/Cargo.toml) | Font metrics and text encoding helpers for generated appearances (standard fonts, WinAnsi) | `pdfcraft-cos` |
| [forms](../../crates/forms/Cargo.toml) | Interactive forms (AcroForm): field model, filling, appearances, reset (ISO 32000-2 §12.7) | `pdfcraft-content`, `pdfcraft-cos`, `pdfcraft-fonts`, `pdfcraft-model` |
| [geom](../../crates/geom/Cargo.toml) | Geometry primitives for PdfCraft (points, rects, page transforms) | Nenhuma declarada nesta categoria |
| [js](../../crates/js/Cargo.toml) | Acrobat JavaScript: a sandboxed interpreter (boa) with the form object model subset (event, getField, app, util, console) | Nenhuma declarada nesta categoria |
| [measure](../../crates/measure/Cargo.toml) | Calibrated 2D PDF measurements, viewports and snapping | `pdfcraft-annot`, `pdfcraft-content`, `pdfcraft-cos`, `pdfcraft-model` |
| [model](../../crates/model/Cargo.toml) | Typed views over the PDF object graph (page tree with inherited attributes, page geometry) | `pdfcraft-cos` |
| [ocr](../../crates/ocr/Cargo.toml) | Scan & OCR: recognise text in page images (ocrs) and write it as an invisible, searchable text layer | `pdfcraft-fonts` |
| [optimize](../../crates/optimize/Cargo.toml) | The PDF Optimizer: image downsampling and recompression, discarding objects, clean-up | `pdfcraft-annot`, `pdfcraft-content`, `pdfcraft-cos`, `pdfcraft-filters`, `pdfcraft-model` |
| [oracle](../../crates/oracle/Cargo.toml) | External oracle processes PdfCraft checks its own output against, and the rule that a missing one is loud | Nenhuma declarada nesta categoria |
| [organize](../../crates/organize/Cargo.toml) | Page and document-structure edits: rotate, delete, move, insert pages; document info | `pdfcraft-content`, `pdfcraft-cos` |
| [platform](../../crates/platform/Cargo.toml) | Operating-system services shared by the PdfCraft desktop app and automation tools | Nenhuma declarada nesta categoria |
| [preflight](../../crates/preflight/Cargo.toml) | Standards: verify PDF/A (ISO 19005) conformance and convert documents to PDF/A-2b or 3b | `pdfcraft-content`, `pdfcraft-cos`, `pdfcraft-model` |
| [print](../../crates/print/Cargo.toml) | Printing: page selection, sizing (fit, actual, shrink, custom), multiple pages per sheet, booklets, posters, comments & forms; print-ready PDF and the system spooler | `pdfcraft-content`, `pdfcraft-cos`, `pdfcraft-model` |
| [redact](../../crates/redact/Cargo.toml) | Redaction (architecture §11.2): marks, apply (glyphs, images, vectors, XObjects, annotations, fields), verification | `pdfcraft-annot`, `pdfcraft-content`, `pdfcraft-cos`, `pdfcraft-edit`, `pdfcraft-fonts`, `pdfcraft-forms`, `pdfcraft-model`, `pdfcraft-xfa` |
| [render](../../crates/render/Cargo.toml) | Page rendering (bootstrap: hayro) and document inspection for PdfCraft | `pdfcraft-cos`, `pdfcraft-fonts` |
| [sign](../../crates/sign/Cargo.toml) | Digital signatures (ISO 32000-2 §12.8, PAdES): X.509, CMS, PKCS #12, signing and validation | `pdfcraft-annot`, `pdfcraft-cos`, `pdfcraft-fonts`, `pdfcraft-model` |
| [testkit](../../crates/testkit/Cargo.toml) | Synthetic PDF fixtures for PdfCraft's tests: one way to write the bytes, so the tests differ only where they mean to | Nenhuma declarada nesta categoria |
| [ui-egui](../../crates/ui-egui/Cargo.toml) | The egui shell of PdfCraft | `pdfcraft-cos`, `pdfcraft-engine`, `pdfcraft-fonts`, `pdfcraft-model`, `pdfcraft-platform`, `pdfcraft-render` |
| [xfa](../../crates/xfa/Cargo.toml) | XFA forms: reads a dynamic form's XDP packets and lays its template out as PDF pages and AcroForm fields | `pdfcraft-cos`, `pdfcraft-fonts` |
| [xfdf](../../crates/xfdf/Cargo.toml) | Comment and form data exchange: XFDF (ISO 19444-1) and FDF import/export, form data as XML, CSV and tab-delimited text | `pdfcraft-annot`, `pdfcraft-cos`, `pdfcraft-forms`, `pdfcraft-model` |

Ao mudar uma entrada, saída ou dependência, atualize primeiro o contrato do módulo e depois esta navegação. [Voltar ao índice](../INDEX.md).
