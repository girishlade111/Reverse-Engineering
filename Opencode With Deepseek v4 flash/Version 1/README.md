# Enterprise Reverse Engineering Prompt Framework

A modular, extensible, reusable prompt framework that guides any AI coding agent through a complete, multi-phase reverse engineering process for any software repository.

## Overview

This framework enables systematic understanding of any codebase — from initial scouting through deep code analysis to final documentation. It is language-agnostic, framework-agnostic, and scales from small single-file tools to monorepos with 10,000+ files.

## Contents

| Layer | Files | Purpose |
|-------|-------|---------|
| **Core Framework** | 8 files | Mission, rules, quality standards, output specs |
| **Phase Prompts** | 23 files (Phases 00–22) | Sequential reverse engineering workflow |
| **Supporting Artifacts** | 4 files | Checklist, templates, diagram guide, glossary |
| **Operational** | 2 files | Troubleshooting, this README |
| **Total** | **37 files** | |

## Quick Start

### 1. Place the framework
Copy the `re-prompt-framework/` directory alongside the repository you want to analyze.

### 2. Read the entry point
Start with `MASTER_PROMPT.md` to understand the complete workflow.

### 3. Execute phases sequentially
Run PROMPT_01 through PROMPT_23 in order. Phase 00 (Scouting) first, Phase 19 (Validation) last.

### 4. Use supporting tools
- `CHECKLIST.md` — track progress against 250+ validation items
- `TEMPLATES.md` — format all output consistently
- `DIAGRAM_GUIDE.md` — generate clear Mermaid diagrams
- `TROUBLESHOOTING.md` — resolve common failures

## Architecture

```
Core Framework (Layer 1)
    ↓ defines rules for
Phase Prompts (Layer 2)
    ↓ executed by
AI Agent
    ↓ produces
re-docs/ (structured output)
```

Phase dependency chain is strictly linear — each phase depends on its predecessor's outputs.

## Usage Modes

| Mode | When | How |
|------|------|-----|
| **Single Session** | Repos <1000 files | Execute all phases in one session |
| **Multi-Session** | Repos 1000-10000+ files | Scout first, then analyze modules independently, assemble results |
| **Targeted** | Specific question | Run only relevant phases in order |

## File Inventory

See `MASTER_INDEX.md` for the complete file inventory and navigation map.

## Phase Map

```
Phase 00: Scouting (PROMPT_01)
Phase 01: Structure (PROMPT_02)
Phase 02: Build & Config (PROMPT_03)
Phase 03: Dependencies (PROMPT_04)
Phase 04: Tech Stack (PROMPT_05)
Phase 05: Modules (PROMPT_06)
Phase 06: Deep Read (PROMPT_07)
Phase 07: Architecture (PROMPT_08)
Phase 08: Data Flow (PROMPT_09)
Phase 09: Call Graph (PROMPT_10)
Phase 10: Features (PROMPT_11)
Phase 11: Algorithms (PROMPT_12)
Phase 12: Design Patterns (PROMPT_13)
Phase 13: API Boundaries (PROMPT_14)
Phase 14: State & Events (PROMPT_15)
Phase 15: Error Handling & Reliability (PROMPT_16)
Phase 16: AI Workflows (PROMPT_17)
Phase 17: Configuration & Environment (PROMPT_18)
Phase 18: Documentation Generation (PROMPT_19)
Phase 19: Validation (PROMPT_20)
Phase 20: Streaming & Reactive (PROMPT_21) [Extended]
Phase 21: Authentication Architecture (PROMPT_22) [Extended]
Phase 22: Deployment Architecture (PROMPT_23) [Extended]
```
