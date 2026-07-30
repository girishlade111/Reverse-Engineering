# 01-repository-intelligence

## Repository Structure Overview

```text
Reverse-Engineering/
├── .git/ [Collapsed]
├── Blackbox with Kimi K2.6/
├── Blackbox with Minimax M2.7/
├── Claude/
├── GLM5.1/
├── Gemini With Gemini 3.1 Pro/
├── Hermes With Deepseek v4 flash/
├── Mistral/
├── Opencode With Deepseek v4 flash/
│   ├── Version 1/
│   └── Version 2/
├── Qwen/
└── README.md
```

## Monorepo Structure & Applications
This repository functions as a documentation-based "monorepo" containing multiple independent variants of the **Enterprise Reverse Engineering Prompt Framework**. Each top-level folder represents a distinct package/service tailored to a specific LLM (Large Language Model) or AI agent runtime.

**Variants (Apps/Services) Found:**
1. `Blackbox with Kimi K2.6`
2. `Blackbox with Minimax M2.7`
3. `Claude`
4. `GLM5.1`
5. `Gemini With Gemini 3.1 Pro`
6. `Hermes With Deepseek v4 flash`
7. `Mistral`
8. `Opencode With Deepseek v4 flash (Version 1 & Version 2)`
9. `Qwen`

There is no traditional workspace tooling (e.g., Nx, Turborepo) because this is a repository of Markdown files, but conceptually they share the same overarching 9-phase (or 10-phase) reverse-engineering methodology documented in the root `README.md`.

## Tech Stack
* **Language(s):** Markdown (`.md`)
* **Framework:** Enterprise Reverse Engineering Prompt Framework (Internal)
* **Package Manager:** N/A
* **Runtime Target:** AI Agents (Claude Code, Gemini, Opencode, Qwen, etc.)

## Entry Points
* **Root Entry Point:** `/README.md` (Provides the global directory and explanation of the framework).
* **Service Entry Points:** Inside each variant directory, the entry points are typically `MASTER_PROMPT.md` and `MASTER_INDEX.md`, which orchestrate the execution of the other prompt files.

## Build & Tooling Setup
* **Bundler:** N/A
* **Linter/Formatter:** N/A (Standard Markdown formatting expected)
* **CI/CD:** N/A
* **Test Runner:** N/A

## Hypothesis of System Function
**Hypothesis:** This system is a comprehensive, multi-model prompt engineering framework designed to guide various Large Language Models (LLMs) through a structured, multi-phase process to reverse-engineer and document unknown software repositories. By separating instructions into discrete phases (e.g., Discovery, Structural Analysis, Deep Code Analysis) and tuning them for specific AI runtimes, the system allows human engineers to feed these prompts into an AI agent, which then autonomously (or semi-autonomously) produces production-grade documentation capable of supporting a full system rebuild.
