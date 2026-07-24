# PROMPT_29.md
## Enterprise Reverse Engineering Prompt Framework — PROMPT_29: Final Documentation Assembly

---

## 1. Prompt Metadata

- **Prompt ID:** PROMPT_29
- **Phase:** 4
- **Stage:** 4 of 5
- **Dependencies:** ART-01 through ART-28 (all prior artifacts), the engagement manifest, the quality log, the registered Output Adapter.
- **Estimated Tokens:** 14000–22000
- **Output Artifacts:** ART-29 (Suite) — Final Documentation Assembly.
- **Author:** EREPF Framework Office
- **Last Revised:** 2026-07-24

---

## 2. Objective

Produce the Final Documentation Assembly artifact (ART-29) — the engagement's deliverable suite — comprising the engagement index (`<engagement_id>_INDEX.md`), the assembled single-file document (`<engagement_id>_ASSEMBLED.md`) concatenating all approved artifacts in phase order with a generated table of contents and cover page, the export via the registered Output Adapter (default DOCX/PDF), and the integrity seal — with all cross-artifact links resolved, all trailing whitespace stripped, all line endings LF, and all encoding UTF-8 no BOM, applying `OUTPUT_RULES.md` § 11 in full.

---

## 3. When to Invoke

PROMPT_29 is dispatched when ALL of the following predicates hold:

- Phase 3 exit conditions are satisfied per `PROJECT_SPECIFICATION.md` § 5.4.
- PROMPT_26, PROMPT_27, and PROMPT_28 have emitted their completion records. ART-26, ART-27, and ART-28 may be `DRAFT` with orchestrator waiver; PROMPT_29 assembles them as-is and records their `DRAFT` status in the engagement index.
- ART-01 through ART-28 (where produced) are present, non-empty, and accessible.
- The engagement manifest's scope modifier is NOT `SCOPE_TRIAGE` (under `SCOPE_TRIAGE`, only PROMPT_29 is dispatched per `MISSION.md` § 6, but it assembles only the Phase 1 artifacts).
- The registered Output Adapter (per `PROJECT_SPECIFICATION.md` § 10.2) is available; if not, PROMPT_29 emits `BLOCKED` with `TOOL_FAIL`.

---

## 4. Required Inputs

| Input | Type | Consuming Purpose |
|-------|------|-------------------|
| ART-01 through ART-28 | All prior artifacts | The complete artifact set to be assembled. Each artifact's front-matter, body, and sidecar files are consumed. |
| `engagement_manifest.json` | Engagement input | The manifest declares the engagement ID, the artifact registry (which artifacts exist and their statuses), the output root, the scope modifier, and (after PROMPT_29) the `final_integrity_seal`. |
| `quality/quality_log.jsonl` | Engagement input | The quality audit trail. PROMPT_29 records the assembly event in the log and verifies that no `BLOCKING` findings remain unaddressed (escalated `BLOCKING` findings from ART-28 are accepted). |
| `OPERATING_RULES.md` | Framework file | Bind R15 (fingerprint), R17 (citation format — preserved during assembly), R33 (contradiction — verified absent in assembled document). |
| `OUTPUT_RULES.md` | Framework file | The primary governing file for PROMPT_29. Apply § 2 (file-system layout: `final/` directory), § 3 (naming), § 4 (header preservation), § 5 (body structure), § 6 (citation conventions — preserved), § 7 (diagram conventions — Mermaid sources embedded), § 8 (cross-reference conventions — link resolution), § 9 (encoding & line endings: LF, no BOM, no trailing whitespace, final newline), § 11 (final assembly: index, assembled document, export, integrity seal), § 12 (forbidden outputs), § 13 (output adapter contract), § 14 (delivery). |
| `QUALITY_STANDARDS.md` | Framework file | Apply Suite schema (§ 4.1 + Suite type). |
| `PROJECT_SPECIFICATION.md` | Framework file | § 3.1 (Suite artifact type), § 10.2 (Output Adapter contract). |
| Registered Output Adapter | External tool | Transforms the canonical Markdown into the target format (DOCX, PDF, Confluence, Notion). The adapter MUST preserve the Traceability Contract per § 13 of `OUTPUT_RULES.md`. |

---

## 5. Instructions to AI Agent

1. Re-verify `repository_fingerprint` matches ART-01; IF not, emit `BLOCKED` with `INTEGRITY_FAIL`.
2. Verify the engagement manifest's artifact registry lists every artifact to be assembled.
3. Generate the engagement index per § 6.1.
4. Concatenate all approved artifacts in phase order per § 6.2 with a generated table of contents and cover page.
5. Resolve all cross-artifact links per § 6.3.
6. Strip trailing whitespace per § 6.4.
7. Enforce LF line endings per § 6.5.
8. Verify UTF-8 encoding with no BOM per § 6.6.
9. Embed Mermaid sources per § 6.7 (rendered SVGs when available, Mermaid source as fallback).
10. Compute the integrity seal per § 6.8.
11. Invoke the registered Output Adapter per § 6.9 to produce the export.
12. Verify the export preserves the Traceability Contract per § 6.10.
13. Record the assembly event in the quality log per § 6.11.
14. Update the engagement manifest per § 6.12 with `final_integrity_seal` and `final_assembled_document_path`.
15. Emit ART-29 per § 8 with full front-matter, the assembly manifest, the sidecar file inventory, the traceability index, the open questions.
16. Run the Quality Checks in § 9.
17. Emit the Completion Record per `MASTER_PROMPT.md` § 6.

---

## 6. Analysis Procedures

### 6.1 Engagement Index Generation

Generate `<engagement_id>_INDEX.md` at `<output_root>/final/`. The index is the End Consumer's entry point and lists every artifact by ID, title, type, phase, status, and relative path. The index structure:

```markdown
# Engagement Index: <engagement_id>

## Engagement Metadata
- Engagement ID: <uuid>
- Subject: <subject_path>
- Scope modifier: <SCOPE_...>
- Framework version: 1.0.0
- Started: <ISO-8601>
- Assembled: <ISO-8601>
- Integrity seal: <sha-256-hex>

## Artifact Catalog

### Phase 1 — Intake & Cartography
| Artifact ID | Title | Type | Status | Path |
| ART-01 | Repository Boundary Declaration | Manifest | REVIEWED | phase1/ART01_<engagement_id>_boundary-declaration.md |
| ... |

### Phase 2 — Dynamics & Behavior
...

### Phase 3 — Intelligence & Patterns
...

### Phase 4 — Synthesis & Delivery
...

## Diagram Inventory
| Diagram ID | Title | Source Artifact | Mermaid Sidecar | SVG |
| D-CTX-01 | System Context | ART-25 | diagrams/<engagement_id>_ART25_D-CTX-01.mmd | diagrams/<engagement_id>_ART25_D-CTX-01.svg |
| ... |

## Reading Order for End Consumers
1. ART-26 (Rebuild Guide) — start here for "how would I build this?"
2. ART-27 (Developer Handbook) — start here for "how do I work in this codebase?"
3. ART-30 (QA Report) — verify the engagement's quality bar.
4. ART-01 through ART-25 — drill-down for specific concerns.

## Quality Summary
- Aggregate quality score: <0..40>
- Coverage: <0..5>
- Traceability: <0..5>
- Open BLOCKING findings: <int>
- Open MAJOR findings: <int>
- Open MINOR findings: <int>
```

### 6.2 Artifact Concatenation

Concatenate all approved artifacts in phase order to produce `<engagement_id>_ASSEMBLED.md`. The procedure:

1. Order the artifacts by phase (1, 2, 3, 4) and by stage within phase.
2. For each artifact, prepend a phase-and-stage divider: `# Phase N — <Phase Title>` and `## ART-XX: <Title>`.
3. Concatenate the artifacts in order, preserving each artifact's front-matter and body.
4. Generate a cover page at the top: engagement metadata (from the manifest), framework version, assembly timestamp, integrity seal (computed in § 6.8).
5. Generate a table of contents after the cover page, listing every artifact with its phase, stage, title, and page-anchor link.
6. Insert page-break markers (`---`) between artifacts for the export adapter's pagination.

The assembled document is the canonical single-file representation. It is the source for the export adapter and the input to PROMPT_30's QA pass.

### 6.3 Cross-Artifact Link Resolution

Resolve all cross-artifact links in the assembled document. The procedure:

1. Parse every artifact's `Cross-References` section and body for `ART-XX` references.
2. For each reference, resolve the relative path to the assembled document's anchor: `[ART-10](../phase1/ART10_<engagement_id>_call-graph.md)` → `[ART-10](#art-10-call-graph)`.
3. Verify the target anchor exists in the assembled document. Missing anchors are `FAIL` findings recorded in ART-29's `open_questions` and escalated to PROMPT_30.
4. Resolve entity references (`FN-XX`, `K-XX`, etc.) to the Entity Registry (from ART-28 § 6.1). The assembled document does NOT inline the registry; it links to ART-28's Entity Registry section.

Link resolution is idempotent: re-running the procedure on an already-resolved document produces no changes.

### 6.4 Trailing Whitespace Stripping

Strip trailing whitespace from every line of the assembled document per `OUTPUT_RULES.md` § 9.3. The procedure:

1. For each line in the assembled document, remove any trailing space or tab characters.
2. Preserve leading whitespace (indentation is significant in Markdown and YAML).
3. Preserve blank lines (a line with only `\n` is preserved).
4. After stripping, verify no line ends with a space or tab; violations are `FAIL` findings.

The stripping is applied to the assembled document AND to every sidecar file (`.mmd`, `.svg` source where applicable).

### 6.5 LF Line Ending Enforcement

Enforce LF (`\n`) line endings per `OUTPUT_RULES.md` § 9.2. The procedure:

1. For each file (assembled document, index, sidecar files), convert any CRLF (`\r\n`) to LF (`\n`) and any standalone CR (`\r`) to LF.
2. Verify the conversion by scanning the file's bytes for `\r`; any remaining `\r` is a `FAIL` finding.

### 6.6 UTF-8 No-BOM Verification

Verify UTF-8 encoding with no BOM per `OUTPUT_RULES.md` § 9.1. The procedure:

1. For each file, read the first three bytes. If they are `0xEF 0xBB 0xBF` (UTF-8 BOM), strip them.
2. Verify the file is valid UTF-8 by attempting to decode it; decode failures are `FAIL` findings.

### 6.7 Mermaid Source Embedding

Embed Mermaid sources in the assembled document per `OUTPUT_RULES.md` § 7. The procedure:

1. For each diagram referenced by an artifact, locate the sidecar file `<engagement_id>_<artifact_id>_D-XX.mmd`.
2. If an SVG rendering exists (`<engagement_id>_<artifact_id>_D-XX.svg`), embed it via `![D-XX](diagrams/<engagement_id>_<artifact_id>_D-XX.svg)`.
3. If no SVG exists, embed the Mermaid source directly in a fenced code block:
   ```markdown
   **Diagram D-XX: <Title>**
   ```mermaid
   <mermaid-source-from-sidecar>
   ```
   ```
4. Verify the sidecar file is non-empty; empty sidecar files are `FAIL` findings.

### 6.8 Integrity Seal Computation

Compute the integrity seal per `OUTPUT_RULES.md` § 11.4. The procedure:

1. Compute the SHA-256 hash of the assembled document's bytes (after § 6.4, § 6.5, § 6.6 normalization).
2. Record the hash as a 64-character lowercase hexadecimal string.
3. Store the hash in the engagement manifest under `final_integrity_seal`.
4. Embed the hash in the assembled document's cover page and in the engagement index.

The integrity seal is the cryptographic commitment to the assembled document's content. PROMPT_30 verifies the seal at QA time; any modification to the assembled document after sealing invalidates the seal and triggers `INTEGRITY_FAIL` per R35.

### 6.9 Output Adapter Invocation

Invoke the registered Output Adapter per `OUTPUT_RULES.md` § 13. The procedure:

1. Read the engagement manifest's `output_adapter` field. Default adapter: `docx` (produces `<engagement_id>_EXPORT.docx`).
2. Invoke the adapter with the assembled document as input.
3. The adapter MUST:
   - Preserve every claim and its citation (the Traceability Contract).
   - Preserve every diagram (render Mermaid to the target format's native diagram or embed an image).
   - Preserve every cross-reference as a clickable link.
   - Preserve the artifact header as a metadata block in the target format.
   - Emit a single primary deliverable plus optional supplementary files.
   - Record the transformation in `quality_log.jsonl`.
4. If the adapter cannot preserve the Traceability Contract, it MUST refuse to run and emit a `BLOCKED` Completion Record per `OUTPUT_RULES.md` § 13.

The export's path is `<output_root>/final/<engagement_id>_EXPORT.<format>`.

### 6.10 Traceability Contract Verification

Verify the export preserves the Traceability Contract per `PROJECT_SPECIFICATION.md` § 6.3. The procedure:

1. Parse the export and extract every citation.
2. For each citation, verify it matches a citation in the assembled document.
3. Verify the count of citations in the export equals the count in the assembled document (within a small tolerance for format differences).
4. Missing citations are `FAIL` findings; the adapter is non-conformant and the engagement is `BLOCKED`.

### 6.11 Quality Log Recording

Record the assembly event in `<output_root>/quality/quality_log.jsonl`. The entry:

```json
{
  "timestamp": "<ISO-8601>",
  "event": "final_assembly",
  "prompt_id": "PROMPT_29",
  "engagement_id": "<uuid>",
  "assembled_document_path": "<output_root>/final/<engagement_id>_ASSEMBLED.md",
  "index_path": "<output_root>/final/<engagement_id>_INDEX.md",
  "export_path": "<output_root>/final/<engagement_id>_EXPORT.<format>",
  "integrity_seal": "<sha-256-hex>",
  "artifact_count": <int>,
  "diagram_count": <int>,
  "citation_count": <int>,
  "open_blocking_findings": <int>,
  "adapter_used": "<name>",
  "adapter_compliance": "traceability-preserved | traceability-violated"
}
```

The log is append-only per `QUALITY_STANDARDS.md` § 10.

### 6.12 Engagement Manifest Update

Update the engagement manifest with the assembly results. The manifest's new fields:

```json
{
  "final_assembled_document_path": "<output_root>/final/<engagement_id>_ASSEMBLED.md",
  "final_index_path": "<output_root>/final/<engagement_id>_INDEX.md",
  "final_export_path": "<output_root>/final/<engagement_id>_EXPORT.<format>",
  "final_integrity_seal": "<sha-256-hex>",
  "final_assembled_at": "<ISO-8601>",
  "final_adapter_used": "<name>",
  "final_open_blocking_findings": <int>
}
```

The manifest update is the only write PROMPT_29 performs outside `<output_root>/final/`. The manifest is then read by PROMPT_30 for the final QA pass.

---

## 7. Required Outputs

### ART-29 — Final Documentation Assembly

**Type:** Suite.

**Acceptance Criteria:**

- AC-29.1: The artifact file exists at `<output_root>/artifacts/phase4/ART29_<engagement_id>_final-assembly.md`.
- AC-29.2: The front-matter conforms to `QUALITY_STANDARDS.md` § 4.1 and the Suite type extension.
- AC-29.3: The engagement index exists at `<output_root>/final/<engagement_id>_INDEX.md` with the structure in § 6.1.
- AC-29.4: The assembled document exists at `<output_root>/final/<engagement_id>_ASSEMBLED.md` with all approved artifacts concatenated in phase order, a cover page, and a table of contents.
- AC-29.5: The export exists at `<output_root>/final/<engagement_id>_EXPORT.<format>` and preserves the Traceability Contract.
- AC-29.6: The integrity seal is computed and recorded in the engagement manifest.
- AC-29.7: All cross-artifact links are resolved to in-document anchors.
- AC-29.8: No trailing whitespace, no CRLF, no BOM in any assembled file.
- AC-29.9: All Mermaid diagrams are embedded (SVG when available, Mermaid source as fallback).
- AC-29.10: The quality log records the assembly event.

---

## 8. Output Templates

### 8.1 ART-29 Front-Matter

```yaml
---
engagement_id: <uuid>
artifact_id: ART-29
artifact_type: Suite
producing_prompt: PROMPT_29
phase: 4
created_at: <ISO-8601>
last_revised: <ISO-8601>
coverage_fraction: <0..1>
quality_score: { coverage: <0..5>, traceability: <0..5>, accuracy: <0..5>, depth: <0..5>, coherence: <0..5>, precision: <0..5>, completeness: <0..5>, readability: <0..5>, aggregate: <0..40> }
status: DRAFT
repository_fingerprint_recheck: <sha-256-hex>
assembly_manifest:
  engagement_id: <uuid>
  index_path: <relative-path>
  assembled_document_path: <relative-path>
  export_path: <relative-path>
  export_format: docx | pdf | confluence | notion | html
  adapter_used: <name>
  adapter_compliance: traceability-preserved | traceability-violated
  integrity_seal: <sha-256-hex>
  assembled_at: <ISO-8601>
  artifact_count: <int>
  diagram_count: <int>
  citation_count: <int>
  link_count: <int>
  unresolved_links: [ <text> ]
sidecar_inventory:
  mermaid_files: [ <relative-path> ]
  svg_files: [ <relative-path> ]
normalization:
  trailing_whitespace_stripped: true
  line_endings: LF
  encoding: UTF-8-no-BOM
  bom_stripped: true | false
  crlf_converted: true | false
source_coverage:
  - path: <file_path>
    symbol_count: <int>
    line_range: <start-end>
open_questions:
  - id: OQ-01
    question: <text>
    blocking: true | false
traceability_index:
  - claim_id: C-01
    source: <file_path>:<line-range>
    symbol: <name>
sections:
  - id: S-01
    title: <string>
    claims: [C-XX]
---
```

### 8.2 ART-29 Body Skeleton

```markdown
# ART-29: Final Documentation Assembly

## 1. Executive Summary
## 2. Methodology
## 3. Assembly Manifest
   - Engagement ID: <uuid>
   - Index: <path>
   - Assembled document: <path>
   - Export: <path> (format: <format>)
   - Integrity seal: <sha-256-hex>
   - Artifact count: <int>
   - Diagram count: <int>
   - Citation count: <int>
## 4. Normalization Report
   - Trailing whitespace: stripped
   - Line endings: LF (CRLF count converted: <int>)
   - Encoding: UTF-8 no BOM (BOM stripped: true | false)
## 5. Link Resolution Report
   - Total links: <int>
   - Resolved: <int>
   - Unresolved: <list>
## 6. Diagram Embedding Report
   - Mermaid sidecars: <int>
   - SVG renderings: <int>
   - Mermaid-source fallbacks: <int>
## 7. Output Adapter Report
   - Adapter: <name>
   - Compliance: traceability-preserved | traceability-violated
   - Citations in source: <int>
   - Citations in export: <int>
## 8. Quality Log Entry
## 9. Engagement Manifest Update
## 10. Traceability Index
## 11. Open Questions
## 12. Cross-References
```

---

## 9. Quality Checks

### Baseline Checks

- **Q1. Coverage Check** — every artifact in the engagement manifest's artifact registry appears in the assembled document. Threshold ≥ 0.99.
- **Q2. Citation Check** — every citation in the assembled document resolves per the Citation Validation Checklist (ART-28 § 6.2).
- **Q3. Schema Conformance Check** — ART-29 validates against the Suite type extension.
- **Q4. Non-Contradiction Check** — the assembled document contains no contradictions beyond those already flagged in ART-28.
- **Q5. UNVERIFIED Accounting** — every `unresolved link` and every `Mermaid-source fallback` has a corresponding Open Question.
- **Q6. Idempotence Spot-Check** — re-running § 6.4, § 6.5, § 6.6 on the assembled document produces no changes.
- **Q7. Handoff Readiness** — all Handoff Criteria in § 11 are satisfied.

### Prompt-Specific Checks

- **Q-29.A. Index Generation** — the engagement index exists and lists every artifact with ID, title, type, phase, status, and relative path. A missing or incomplete index is `BLOCKING`.
- **Q-29.B. Assembled Document Completeness** — the assembled document contains every approved artifact in phase order, with a cover page and a table of contents. Missing artifacts or missing structural elements are `BLOCKING`.
- **Q-29.C. Cross-Reference Resolution** — every cross-artifact link resolves to an in-document anchor. Unresolved links are `MAJOR` findings and are escalated to PROMPT_30.
- **Q-29.D. Normalization Compliance** — no trailing whitespace, no CRLF, no BOM in any assembled file. Violations are `BLOCKING` per `OUTPUT_RULES.md` § 9.
- **Q-29.E. Mermaid Embedding** — every diagram referenced by an artifact is embedded (SVG or Mermaid source). Missing diagrams are `BLOCKING`.
- **Q-29.F. Integrity Seal** — the integrity seal is computed, recorded in the engagement manifest, and embedded in the cover page. A missing or mismatched seal is `BLOCKING` per `OUTPUT_RULES.md` § 11.4.
- **Q-29.G. Export Traceability Preservation** — the export preserves every citation. Citation-count mismatch is `BLOCKING` per `OUTPUT_RULES.md` § 13.
- **Q-29.H. Quality Log Recording** — the assembly event is recorded in `quality_log.jsonl`. A missing log entry is `MAJOR`.
- **Q-29.I. Engagement Manifest Update** — the engagement manifest is updated with `final_integrity_seal`, `final_assembled_document_path`, and related fields. Missing manifest updates are `BLOCKING`.
- **Q-29.J. Final Newline** — every file ends with exactly one trailing newline per `OUTPUT_RULES.md` § 9.4. Missing or multiple trailing newlines are `MAJOR`.

---

## 10. Common Pitfalls

- Do not assemble artifacts out of phase order; phase order is mandatory per `OUTPUT_RULES.md` § 11.2.
- Always strip trailing whitespace AFTER concatenation; stripping before concatenation risks reintroducing whitespace at concatenation boundaries.
- Always enforce LF line endings on every file including sidecars; CRLF in a sidecar file breaks Mermaid rendering in some adapters.
- Always verify UTF-8 no-BOM; some editors add BOM silently and adapters handle BOM inconsistently.
- Always resolve cross-artifact links before computing the integrity seal; unresolved links left in the sealed document are `MAJOR` findings per Q-29.C.
- Do not invoke an adapter that cannot preserve the Traceability Contract; refuse per `OUTPUT_RULES.md` § 13 and emit `BLOCKED`.
- Always embed Mermaid sources from sidecar files; do not transclude from the artifact body (the body may have been edited; the sidecar is the canonical source).
- Always compute the integrity seal AFTER all normalization; sealing before normalization produces a seal that does not match the delivered document.
- Always record the assembly event in the quality log; the log is the engagement's audit trail per `QUALITY_STANDARDS.md` § 10.
- Always update the engagement manifest; PROMPT_30 reads the manifest for the final QA pass.
- Do not modify the assembled document after sealing; modifications invalidate the seal per R35.
- Always ensure the final newline is exactly one; multiple trailing newlines break some Markdown renderers.

---

## 11. Handoff Criteria

PROMPT_30 consumes ART-29 and the assembled document. Handoff requires ALL of:

- HC-29.1: ART-29 status is `REVIEWED` or `DRAFT` with orchestrator waiver.
- HC-29.2: The engagement index, assembled document, and export all exist at their declared paths.
- HC-29.3: All cross-artifact links are resolved (or unresolved links are escalated to PROMPT_30 as `MAJOR` findings).
- HC-29.4: Normalization is complete (no trailing whitespace, no CRLF, no BOM, one final newline).
- HC-29.5: Every diagram is embedded (SVG or Mermaid source).
- HC-29.6: The integrity seal is computed and recorded in the engagement manifest.
- HC-29.7: The export preserves the Traceability Contract (or the adapter is non-conformant and the engagement is `BLOCKED`).
- HC-29.8: The quality log records the assembly event.
- HC-29.9: `repository_fingerprint_recheck` matches ART-01.
- HC-29.10: No `BLOCKING` open questions remain (escalated findings from ART-28 are accepted; PROMPT_30 decides their resolution).

---

## 12. Cross-References

- **Consumed by:** PROMPT_30 (Self-Review & QA — verifies the integrity seal, re-runs the QA pass against the assembled document, decides engagement completion).
- **Depends on:** ART-01 through ART-28 (all prior artifacts), the engagement manifest, the quality log, the registered Output Adapter.
- **Governing rules:** `OPERATING_RULES.md` R15 (fingerprint), R17 (citation format — preserved), R33 (contradiction — verified absent), R35 (integrity termination on seal invalidation).
- **Schema authority:** `QUALITY_STANDARDS.md` § 4.1, Suite type extension.
- **Output authority:** `OUTPUT_RULES.md` § 2 (file-system layout), § 3 (naming), § 4 (header preservation), § 5 (body structure), § 6 (citations), § 7 (diagrams), § 8 (cross-references), § 9 (encoding & line endings), § 11 (final assembly), § 12 (forbidden outputs), § 13 (output adapter contract), § 14 (delivery).
- **Forward reference:** PROMPT_30 (Self-Review & QA) verifies the integrity seal against the assembled document's current hash; any mismatch triggers `INTEGRITY_FAIL` per R35.

*End of PROMPT_29. Orchestrator may dispatch PROMPT_30 upon satisfaction of § 11.*
