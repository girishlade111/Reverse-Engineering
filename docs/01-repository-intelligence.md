# 01 - Repository Intelligence

## Repository Structure Overview

```text
Reverse-Engineering/
├── .git/                                        [Collapsed - version control]
├── Antigravity with Gemini 3.1 Pro high/        [Example output - completed framework run]
│   ├── 00-INDEX.md
│   ├── 01-repository-intelligence.md
│   ├── 02-file-folder-analysis/
│   ├── 03-prompt-template-docs/
│   ├── 04-architecture/
│   ├── 05-diagrams.md
│   ├── 06-ai-agent-workflow.md
│   ├── 07-tech-stack.md
│   ├── 08-conditional-docs/
│   └── 09-developer-handbook-rebuild-guide.md
├── Blackbox with Kimi K2.6/                     [Variant - 7 domain subfolders]
│   ├── DOMAIN_2_DISCOVERY/
│   ├── DOMAIN_3_ARCHITECTURE/
│   ├── DOMAIN_4_DEEP_INTELLIGENCE/
│   ├── DOMAIN_5_WORKFLOW/
│   ├── DOMAIN_6_DOCUMENTATION/
│   ├── DOMAIN_7_VALIDATION/
│   └── [8 infrastructure files]
├── Blackbox with Minimax M2.7/                  [Variant - 4 subfolders]
│   ├── checklists/
│   ├── modules/
│   ├── standards/
│   ├── templates/
│   └── [18 root-level files]
├── Claude/                                      [Variant - Prompts/ subfolder]
│   ├── Prompts/
│   ├── reverse-engineering-master-prompt.md
│   └── REVERSE_ENGINEERING_PROMPT_COMBINED.md
├── Gemini With Gemini 3.1 Pro/                  [Variant - minimal]
│   ├── Master Index.md
│   ├── Mission Directive.md
│   └── Operating Rules.md
├── GLM5.1/                                      [Variant - flat structure]
│   ├── [8 infrastructure .md files]
│   ├── [30 prompt .md files]
│   └── Enterprise_Reverse_Engineering_Prompt_Framework_TechnicalDoc_2026-07-24.docx
├── Hermes With Deepseek v4 flash/               [Canonical reference variant]
│   ├── Phase 1 - Discovery/
│   ├── Phase 2 - Structural Analysis/
│   ├── Phase 3 - Architecture Reconstruction/
│   ├── Phase 4 - Deep Code Analysis/
│   ├── Phase 5 - AI and Automation Analysis/
│   ├── Phase 6 - Integration and Boundary Analysis/
│   ├── Phase 7 - Documentation Generation/
│   ├── Phase 8 - Validation and Quality/
│   ├── Phase 9 - Rebuild Package/
│   └── [13 infrastructure files]
├── Mistral/                                     [Variant - Version 1 + Mistral 2/]
│   ├── Version 1.md
│   └── Mistral 2/
│       ├── All in one version/
│       └── [11 framework files]
├── Opencode With Deepseek v4 flash/             [Variant - two versions]
│   ├── Version 1/                               [37 files]
│   └── Version 2/                               [39 files]
├── Qwen/                                        [Variant - prompts/ + templates/]
│   ├── prompts/
│   ├── templates/
│   └── [10 root-level files]
└── README.md                                    [Root entry point]
```

## Monorepo Structure and Variant Detection

This repository functions as a documentation-only monorepo containing multiple independent implementations of the **Enterprise Reverse Engineering Prompt Framework (EREPF)**. Each top-level folder represents either a framework variant tailored to a specific AI model or an example output produced by executing the framework.

There is no workspace tooling (no Nx, Turborepo, Lerna, or pnpm workspaces) because this is a pure Markdown repository with zero executable code.

### Variants Detected

| # | Variant Name | Directory | Files | Target Model/Runtime | Status |
|---|---|---|---|---|---|
| 1 | Hermes + Deepseek v4 flash | `Hermes With Deepseek v4 flash/` | 49 | Deepseek v4 flash via Hermes | Canonical reference |
| 2 | Opencode + Deepseek v4 flash (V1) | `Opencode With Deepseek v4 flash/Version 1/` | 37 | Deepseek v4 flash via Opencode | Complete |
| 3 | Opencode + Deepseek v4 flash (V2) | `Opencode With Deepseek v4 flash/Version 2/` | 39 | Deepseek v4 flash via Opencode | Complete (v3.0) |
| 4 | Claude | `Claude/` | 20 | Claude Code / VS Code | Complete |
| 5 | GLM 5.1 | `GLM5.1/` | 39 | GLM 5.1 | Complete |
| 6 | Gemini 3.1 Pro | `Gemini With Gemini 3.1 Pro/` | 3 | Gemini 3.1 Pro | Foundational (minimal) |
| 7 | Qwen | `Qwen/` | 15 | Qwen | Complete |
| 8 | Mistral | `Mistral/` | 13 | Mistral (v1 + v2) | Complete |
| 9 | Blackbox + Kimi K2.6 | `Blackbox with Kimi K2.6/` | 36 | Kimi K2.6 via Blackbox | Complete |
| 10 | Blackbox + Minimax M2.7 | `Blackbox with Minimax M2.7/` | 37 | Minimax M2.7 via Blackbox | Complete |

### Non-Variant Directories

| # | Directory | Files | Purpose |
|---|---|---|---|
| 1 | `Antigravity with Gemini 3.1 Pro high/` | 14 | Example output from a completed framework execution against the Antigravity project |

### Root Files

| # | File | Purpose |
|---|---|---|
| 1 | `README.md` | Global entry point; documents framework purpose, architecture, 9-phase pipeline, usage instructions, and variant comparison |

**Total files in repository:** 303

## Tech Stack

| Dimension | Value |
|---|---|
| Language(s) | Markdown (`.md`), one `.docx` (GLM5.1 variant) |
| Framework | Enterprise Reverse Engineering Prompt Framework (internal, non-executable) |
| Package Manager | N/A |
| Runtime Target | AI coding agents (Claude Code, Opencode, Hermes, Qwen, Gemini, Blackbox, Mistral) |
| Build System | None |
| Test Framework | None (quality gates are embedded within prompt files as textual checklists) |
| Infrastructure | None (pure documentation; no servers, containers, or CI/CD) |

### Per-Variant Stack Breakdown

All variants share the same stack profile: Markdown files targeting AI agent runtimes. The sole exception is `GLM5.1/`, which includes one `.docx` binary file alongside its Markdown prompts.

## Entry Points

### Global Entry Point

| Entry Point | File | Purpose |
|---|---|---|
| Root README | `README.md` | Framework overview, variant comparison, usage instructions, 9-phase pipeline documentation |

### Per-Variant Entry Points

| Variant | Primary Entry Point | Secondary Entry Point |
|---|---|---|
| Hermes + Deepseek v4 flash | `MASTER_PROMPT.md` | `MASTER_INDEX.md` |
| Opencode V1 | `MASTER_PROMPT.md` | `MASTER_INDEX.md` |
| Opencode V2 | `MASTER_PROMPT.md` | `MASTER_INDEX.md` |
| Claude | `reverse-engineering-master-prompt.md` | `REVERSE_ENGINEERING_PROMPT_COMBINED.md` |
| GLM 5.1 | `MASTER_PROMPT.md` | `MASTER_INDEX.md` |
| Gemini 3.1 Pro | `Master Index.md` | `Mission Directive.md` |
| Qwen | `MASTER_PROMPT.md` | `MASTER_INDEX.md` |
| Mistral (V1) | `Version 1.md` | N/A (single file) |
| Mistral (V2) | `Mistral 2/MASTER_PROMPT.md` | `Mistral 2/MASTER_INDEX.md` |
| Blackbox + Kimi K2.6 | `MASTER_PROMPT.md` | `MASTER_INDEX.md` |
| Blackbox + Minimax M2.7 | `MASTER_PROMPT.md` | `MASTER_INDEX.md` |

The entry points function as orchestrator prompts: they instruct the AI agent to load infrastructure files (rules, standards, specifications) and then execute analysis prompts sequentially.

## Build and Tooling Setup

| Category | Status |
|---|---|
| Bundler | N/A |
| Linter/Formatter | N/A (standard Markdown formatting expected) |
| CI/CD | N/A |
| Test Runner | N/A |
| Docker | N/A |
| Package Lock Files | N/A |
| Pre-commit Hooks | N/A |

This repository requires zero build infrastructure. It is consumed by loading Markdown files directly into AI agent context windows.

## System Hypothesis

This repository is a comprehensive, multi-model prompt engineering framework designed to guide Large Language Models (LLMs) through a structured, multi-phase process to reverse engineer and document unknown software repositories. The framework implements a three-layer architecture (infrastructure, orchestration, execution) where infrastructure files establish governance rules and quality standards, an orchestrator prompt sequences the pipeline, and 28-36 specialized execution prompts perform progressive analysis from initial discovery through architecture reconstruction to final validation.

By maintaining 10 independent variants, each tailored to a specific AI model's strengths and context window characteristics, the repository serves as both a production tool and a comparative research platform for evaluating prompt engineering approaches across different LLM runtimes. The canonical Hermes variant provides the most complete implementation (49 files, 9 phases, 36 prompts), while lighter variants (Gemini at 3 files) demonstrate how the same methodology can be compressed for models with larger context windows or different operational modes.
