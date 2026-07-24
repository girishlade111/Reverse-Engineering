# OUTPUT_RULES.md
## Enterprise Reverse Engineering Prompt Framework — Output Rules

> **Document Type:** Format & Delivery Rules
> **Framework Version:** 1.0.0
> **Authority:** Binding. Enforced by PROMPT_29 and PROMPT_30.
> **Audience:** All agents, orchestrators, output adapters.

---

## 1. Purpose of This Document

This document defines the **format, structure, naming, encoding, location, and delivery rules** for every artifact the framework produces. Output discipline is what makes the framework's artifacts machine-parseable, cross-referenceable, and portable across consumers. Without output rules, the framework's artifacts would be a pile of well-intentioned Markdown files with no interoperability; with output rules, they form a queryable knowledge base.

The rules are exhaustive. They cover the file-system layout, the artifact header, the body structure, the diagram conventions, the citation conventions, the cross-reference conventions, the encoding, the line-ending policy, the directory tree, and the final assembly. A new agent should be able to produce conformant artifacts by reading this document alone; a new consumer should be able to parse them by reading this document alone.

These rules are enforced twice: once by the producing prompt's Quality Checks (self-check) and once by PROMPT_29 (Final Documentation Assembly) and PROMPT_30 (QA). Non-conformance is a `BLOCKING` finding.

---

## 2. File-System Layout

### 2.1 Output Root
All outputs are written under `<output_root>/`, declared in the engagement manifest. The orchestrator MUST NOT write outside this root.

### 2.2 Directory Tree

```
<output_root>/
├── engagement_manifest.json          # Stage 0 output (immutable)
├── artifacts/
│   ├── phase1/
│   │   ├── ART01_<engagement_id>_boundary-declaration.md
│   │   ├── ART02_<engagement_id>_tech-stack.md
│   │   ├── ...
│   ├── phase2/
│   ├── phase3/
│   ├── phase4/
├── diagrams/
│   ├── <engagement_id>_<diagram_id>.mmd
│   ├── <engagement_id>_<diagram_id>.svg
├── completion/
│   ├── <engagement_id>_<prompt_id>_completion.md
├── quality/
│   ├── quality_log.jsonl
├── reviews/
│   ├── <engagement_id>_<artifact_id>_review.md
└── final/
    ├── <engagement_id>_INDEX.md
    ├── <engagement_id>_ASSEMBLED.md
    └── <engagement_id>_EXPORT.<format>     # produced by output adapter
```

### 2.3 Phase Directories
Each phase directory holds only artifacts produced by that phase's prompts. Cross-phase artifacts are forbidden; if a Phase 2 prompt needs a Phase 1 artifact, it reads it from `phase1/` and writes its own output to `phase2/`.

### 2.4 Diagram Sidecars
Diagrams referenced by an artifact are stored as sidecar files in `diagrams/`. The artifact embeds the diagram by relative path. Mermaid source (`.mmd`) is always emitted; rendered formats (`.svg`, `.png`) are emitted when an adapter is available.

---

## 3. Artifact File Naming

### 3.1 Naming Pattern
```
<artifact_id>_<engagement_id>_<slug>.<ext>
```
- `artifact_id`: `ART-XX` zero-padded to 2 digits (e.g., `ART03`).
- `engagement_id`: the engagement's UUID.
- `slug`: lowercase-kebab, derived from the artifact's title, max 40 chars.
- `ext`: `.md` for narrative artifacts; `.mmd`/`.svg` for diagrams; `.json` for pure-data manifests where declared.

### 3.2 Slug Derivation
The slug is derived from the artifact title by: lowercasing, replacing non-alphanumeric runs with single hyphens, trimming leading/trailing hyphens, truncating to 40 chars at a word boundary.

### 3.3 Reserved Names
The following names are reserved and MUST NOT be used for artifacts: `INDEX`, `README`, `CHANGELOG`, `LICENSE`. These are framework-level files in `final/`.

---

## 4. Artifact Header (YAML Front-Matter)

Every artifact file begins with a YAML front-matter block conforming to `QUALITY_STANDARDS.md` § 4.1. The header is the artifact's machine-readable identity; consumers parse it before reading the body.

### 4.1 Required Header Fields
`engagement_id`, `artifact_id`, `artifact_type`, `producing_prompt`, `phase`, `created_at`, `last_revised`, `coverage_fraction`, `quality_score` (all eight dimensions + aggregate), `status`, `source_coverage`, `open_questions`, `traceability_index`.

### 4.2 Forbidden in Header
Narrative content, prose, or commentary. The header is data only.

### 4.3 Header Immutability
Once an artifact is `REVIEWED`, its header is immutable except for `last_revised` and `status`. Edits to substantive content require a new versioned artifact, not an in-place edit.

---

## 5. Body Structure

### 5.1 Standard Body Sections
After the header, the body contains:

1. **Title** (`# ART-XX: <Title>`)
2. **Executive Summary** (3–5 sentences; what the artifact contains and its key findings)
3. **Methodology** (how the artifact was produced; the procedure applied)
4. **Findings** (the substantive content; sectioned per the artifact's nature)
5. **Traceability Index** (mirror of the header's `traceability_index` in human-readable form)
6. **Open Questions** (mirror of the header's `open_questions`)
7. **Cross-References** (links to related artifacts by ID)

### 5.2 Section Numbering
Sections are numbered hierarchically (`1.`, `1.1`, `1.1.1`). Depth beyond 4 levels is forbidden; restructure instead.

### 5.3 Paragraph Depth
Per the framework's content-depth rule: every paragraph contains at least 3–5 sentences; every section contains at least 150–200 words of body content. Single-sentence paragraphs are forbidden except as transitional statements.

### 5.4 Lists
Lists are left-aligned (not justified). Each list item is followed by a line break. Multiple items on one line are forbidden.

---

## 6. Citation Conventions

### 6.1 Inline Citation Format
Inline citations use the form `(file:line-range, symbol)` immediately after the claim:
> The `dispatchRequest()` function routes by method type (`src/router/dispatch.ts:142-198`, symbol: `dispatchRequest`).

### 6.2 Claim IDs
Each claim is assigned a stable ID `C-XX` within the artifact. The ID appears in the inline citation as `([C-07] file:line-range, symbol)` and is resolved in the Traceability Index.

### 6.3 Aggregate Citations
Aggregate claims cite the procedure: `([C-12] enumeration: all files matching src/services/**/*.ts, 42 matches)`.

### 6.4 External References
References to entities outside the subject repository (e.g., a library's API) are marked `EXTERNAL` and cite the library's documentation URL where available.

---

## 7. Diagram Conventions

### 7.1 Mermaid as Default
Mermaid is the default diagram language. All flowcharts, sequence diagrams, class diagrams, state diagrams, and entity-relationship diagrams are emitted as Mermaid source.

### 7.2 Diagram Header
Every Mermaid block is preceded by a caption:
```
**Diagram D-XX: <Title>**
```mermaid
<diagram>
```
```

### 7.3 Diagram IDs
Diagrams are numbered `D-XX` within the artifact. The Mermaid sidecar file is named `<engagement_id>_<artifact_id>_D-XX.mmd`.

### 7.4 Diagram Types by Use Case
- **Flowchart** — control flow, decision logic.
- **Sequence Diagram** — request/response, event ordering, multi-actor workflows.
- **Class Diagram** — class hierarchies, interface implementations.
- **State Diagram** — state machines, lifecycle.
- **Entity-Relationship** — database schemas, domain models.
- **C4 Diagram** — system context, containers, components (used in architecture handbooks).
- **Graph (LR/TD)** — call graphs, dependency graphs, data flow.

### 7.5 Diagram Quality
- Every node and edge carries a label.
- Every edge in a call/dependency graph cites its source (`edge: file:line`).
- Diagrams are kept to ≤ 30 nodes for readability; larger graphs are decomposed into sub-diagrams with a master index diagram.

---

## 8. Cross-Reference Conventions

### 8.1 Artifact References
References to other artifacts use the form `ART-XX` and link to the artifact's file by relative path: `[ART-10](../phase1/ART10_<engagement_id>_call-graph.md)`.

### 8.2 Entity References
References to entities use the entity's stable ID: `FN-042` for a function, `K-007` for a class. The ID resolves via the entity registry (a manifest produced by PROMPT_28).

### 8.3 Bidirectional Links
Where artifact A references artifact B, artifact B's Cross-References section MUST include a back-reference to A. PROMPT_28 verifies bidirectionality.

---

## 9. Encoding & Line Endings

### 9.1 Encoding
All artifacts are UTF-8 encoded. Byte-order marks are forbidden.

### 9.2 Line Endings
LF (`\n`) only. CRLF is forbidden.

### 9.3 Trailing Whitespace
Trailing whitespace on any line is forbidden. PROMPT_29 strips trailing whitespace on assembly.

### 9.4 Final Newline
Every file ends with exactly one trailing newline.

---

## 10. Language & Tone

### 10.1 Default Language
English for internal engineering documentation. User-facing documentation (handbooks) matches the subject repository's primary human language unless the orchestrator's `language_directive` overrides.

### 10.2 Tone
Objective, neutral, professional. Imperative for procedural content ("Enumerate…", "Verify…"); declarative for findings ("The module exports…"). Colloquialisms, marketing language, and hedging are forbidden.

### 10.3 Terminology Consistency
A concept is referred to by one term throughout an artifact. Synonym drift is forbidden. The glossary in `PROJECT_SPECIFICATION.md` § 12 is the terminology authority.

### 10.4 Mixed-Language Typography
When an artifact contains both code identifiers and prose, code identifiers are rendered in backticks. When mixing CJK and Latin text, scripts are not mixed within a word.

---

## 11. Final Assembly (PROMPT_29)

### 11.1 Index Generation
PROMPT_29 generates `<engagement_id>_INDEX.md` listing every artifact by ID, title, type, phase, and relative path. The index is the entry point for the End Consumer.

### 11.2 Assembled Document
PROMPT_29 generates `<engagement_id>_ASSEMBLED.md` concatenating all approved artifacts in phase order, with a generated table of contents, cover page, and cross-artifact link resolution. This is the canonical single-file representation.

### 11.3 Export
PROMPT_29 invokes the registered Output Adapter to produce `<engagement_id>_EXPORT.<format>` (default: `.docx`, `.pdf`). The export preserves the Traceability Contract; links in the export resolve to source locations or to the assembled document's sections.

### 11.4 Integrity Seal
PROMPT_29 computes a hash of the assembled document and records it in `engagement_manifest.json` under `final_integrity_seal`. The seal is verified at PROMPT_30.

---

## 12. Forbidden Outputs

The following are forbidden and trigger `BLOCKING` findings:

1. **Un-traced claims** (violates `OPERATING_RULES.md` R17).
2. **Fabricated entities** (violates R21).
3. **Cross-phase artifacts** (violates § 2.3).
4. **Edits to `REVIEWED` artifacts** without versioning (violates § 4.3).
5. **Missing headers** (violates § 4).
6. **Diagrams without captions or IDs** (violates § 7.2).
7. **CRLF line endings or BOM** (violates § 9).
8. **Trailing whitespace** (violates § 9.3).
9. **Justified lists or multi-item lines** (violates § 5.4).
10. **Single-sentence paragraphs** (violates § 5.3).

---

## 13. Output Adapter Contract

Output adapters transform the canonical Markdown into other formats. Adapters MUST:

1. Preserve every claim and its citation.
2. Preserve every diagram (render Mermaid to the target format's native diagram or embed an image).
3. Preserve every cross-reference as a clickable link.
4. Preserve the artifact header as a metadata block in the target format.
5. Emit a single primary deliverable plus optional supplementary files.
6. Record the transformation in `quality_log.jsonl`.

Adapters that cannot preserve the Traceability Contract MUST refuse to run and emit a `BLOCKED` Completion Record.

---

## 14. Delivery

### 14.1 Primary Deliverable
The primary deliverable is `<engagement_id>_EXPORT.docx` (or `.pdf` per orchestrator directive), located in `<output_root>/final/`.

### 14.2 Supplementary Deliverables
- `<engagement_id>_ASSEMBLED.md` — the canonical single-file Markdown.
- `<engagement_id>_INDEX.md` — the artifact index.
- `diagrams/` — the diagram sidecar directory.
- `quality/quality_log.jsonl` — the quality audit trail.

### 14.3 Delivery Confirmation
The orchestrator confirms delivery by verifying the `final_integrity_seal` and the QA Report's zero-blocking status. Delivery without QA clearance is forbidden.

---

*End of Output Rules. Proceed to `PROMPT_01.md`.*
