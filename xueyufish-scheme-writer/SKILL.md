---
name: xueyufish-scheme-writer
description: Write design documents in preferred format. Load when asked to create requirements, architecture, development designs, FAQs, status reports, or risk reports.
---

# Scheme Writer

Write design documents using my preferred format.

## Usage

1. **Identify document type** from user request
2. **Load the matching guide** from `examples/` (see routing table below)
3. **Follow the guide's instructions** for format, tone, and content

## Document Type Routing

| Document Type | Guide File | Description |
|---------------|-----------|-------------|
| Architecture / Development / Requirements / Data Design | `examples/arch-designs.md` | R&D design documents |
| FAQ | `examples/faq-answers.md` | Frequently asked questions |
| General Communication | `examples/general-comms.md` | Other project communications |

If the document type doesn't match any guide above, ask the user for clarification.
