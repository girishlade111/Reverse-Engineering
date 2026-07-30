# Phase 7: Tech Stack

> Technology analysis of the Enterprise Reverse Engineering Framework. Unlike traditional software projects with compiled languages and package managers, this repository's "tech stack" consists of a documentation language, a custom prompt framework, and runtime environment requirements.

---

## 1. Language Analysis

### Primary Language: Markdown

| Property | Value |
|----------|-------|
| Language | Markdown (GitHub Flavored Markdown, CommonMark superset) |
| File extension | `.md` |
| File count | ~60+ files across two framework variants |
| Lines of "code" | ~15,000+ lines of structured prompt text |
| Compilation | None (interpreted by LLM at runtime) |
| Type system | None |
| Package manager | None |

### Rationale for Markdown

Markdown was selected as the sole implementation language for the following engineering reasons:

1. **Token efficiency:** Markdown uses minimal syntax overhead (headings, bullets, bold) compared to alternatives like XML or JSON. Every token in the LLM's context window carries semantic content rather than structural boilerplate. A prompt encoded in Markdown consumes approximately 30-40% fewer tokens than the same content in structured XML.

2. **Universal LLM compatibility:** Every major LLM (GPT-4, Claude, Gemini, DeepSeek, Llama) natively understands Markdown formatting. No special parsing or preprocessing is required. The model can immediately interpret headers as hierarchy, bullets as lists, and code fences as distinct content blocks.

3. **Human readability:** Framework authors and operators can read, edit, and review prompts without specialized tooling. Any text editor suffices. No build step is required between editing and execution.

4. **Version control friendliness:** Markdown produces clean diffs in Git. Changes to prompts are visible line-by-line, enabling meaningful code review of framework modifications.

5. **Zero runtime dependencies:** Markdown requires no interpreter, compiler, or runtime. The LLM itself is the "interpreter." This eliminates an entire class of deployment and compatibility issues.

### Language Limitations

| Limitation | Impact | Mitigation |
|-----------|--------|------------|
| No type checking | Prompt interface contracts are informal | `PROMPT_DESIGN_GUIDE.md` defines conventions |
| No static validation | Malformed prompts detected only at runtime | `VALIDATION_CHECKLISTS.md` provides manual checks |
| No import mechanism | Cross-file references are by convention, not enforced | `PROMPT_DEPENDENCY_MAP.md` encodes the DAG explicitly |
| No parameterization | Variables cannot be injected programmatically | Context Summary mechanism provides dynamic input |

---

## 2. Framework Analysis

### Framework: Custom Prompt Orchestration Framework

This is not a third-party framework. It is a bespoke prompt engineering framework authored specifically for enterprise-grade repository reverse engineering.

| Property | Value |
|----------|-------|
| Framework name | Enterprise Reverse Engineering Prompt Framework |
| Author | Framework designer (human) |
| Architecture | Three-layer (Infrastructure / Orchestration / Execution) |
| Configuration mechanism | Infrastructure files (Layer 1) |
| Orchestration mechanism | `MASTER_PROMPT.md` (Layer 2) |
| Extension mechanism | Add prompt files following `PROMPT_DESIGN_GUIDE.md` conventions |

### Configuration via Infrastructure Files

Rather than using environment variables, YAML configs, or JSON schemas, the framework encodes its configuration as natural-language Markdown documents:

| Configuration Concern | Traditional Approach | This Framework's Approach |
|----------------------|---------------------|--------------------------|
| Behavioral rules | Code constants, env vars | `OPERATING_RULES.md` (12 rules in prose) |
| Quality thresholds | Numeric configs, CI gates | `QUALITY_STANDARDS.md` (Q1-Q10 with criteria) |
| Output formatting | Linters, formatters, templates | `OUTPUT_RULES.md` (7 format sections) |
| Execution order | Build system DAGs, Makefiles | `PROMPT_DEPENDENCY_MAP.md` (text DAG) |
| Validation criteria | Test suites, assertions | `VALIDATION_CHECKLISTS.md` (per-phase checklists) |
| Terminology | Code comments, wikis | `GLOSSARY.md` (term definitions) |

### Framework Extensibility

New phases or prompts can be added by:

1. Creating a new `.md` file following the template in `PROMPT_DESIGN_GUIDE.md`
2. Adding the prompt to `PROMPT_DEPENDENCY_MAP.md` with its prerequisites
3. Adding a validation checklist entry to `VALIDATION_CHECKLISTS.md`
4. Updating `MASTER_INDEX.md` for navigation

No code changes, no recompilation, no package version bumps required.

---

## 3. Dependency Analysis

This repository has no `package.json`, `requirements.txt`, `Cargo.toml`, or equivalent manifest. Its "dependencies" are capabilities required of the runtime environment.

### Runtime Dependencies

| Dependency | Minimum Requirement | Purpose | Criticality |
|-----------|-------------------|---------|-------------|
| Large Language Model | 32K+ context window, strong instruction following | Executes all prompts, performs analysis, generates documentation | **Critical** (no alternative) |
| Agentic IDE Environment | Filesystem read/write, file search, directory listing | Provides implicit tool calling capabilities assumed by the framework | **Critical** (no alternative) |
| Mermaid Rendering Engine | Mermaid.js compatible renderer | Renders diagrams produced in Phase 7 (P28) | **Important** (graceful degradation to raw syntax) |
| Git | Standard Git CLI | Version control for generated documentation artifacts | **Optional** (useful but not required for execution) |
| Target Repository | Accessible filesystem path | The subject of reverse engineering analysis | **Critical** (input data) |

### LLM Requirements (Detailed)

| Requirement | Minimum | Recommended | Rationale |
|------------|---------|-------------|-----------|
| Context window | 32,000 tokens | 128,000+ tokens | Framework infrastructure alone consumes ~12K tokens; large repos require additional capacity |
| Instruction following | High fidelity | Highest available | Framework relies on precise compliance with multi-step System Prompts |
| Code comprehension | Multi-language support | All major languages | Target repos may use any language combination |
| Structured output | Markdown generation | Markdown + Mermaid | Documentation and diagrams are the primary output |
| Reasoning capability | Multi-step analysis | Extended thinking/CoT | Deep code analysis (Phase 4) requires complex reasoning chains |

### Agentic IDE Requirements

| Capability | Required For | Example Environments |
|-----------|-------------|---------------------|
| Read File (by path) | All analysis prompts (P01-P36) | Cursor, Claude Code, Opencode, Windsurf |
| List Directory (recursive) | Repository scanning (P01), folder architecture (P04) | Same |
| Search (text/semantic) | Dependency tracing (P05), caller analysis, flow tracing | Same |
| Write File | Output generation (all phases), `_analysis/` notes | Same |
| Multi-file context | Loading infrastructure + active prompt + source files simultaneously | Same |

---

## 4. Tech Stack Summary Table

| Layer | Technology | Role | Viable Alternatives | Trade-offs of Alternative |
|-------|-----------|------|---------------------|--------------------------|
| Implementation Language | Markdown (GFM) | All prompts, configuration, and documentation | ReStructuredText (.rst) | Less universal LLM support; heavier syntax overhead; smaller ecosystem for rendering |
| Implementation Language | Markdown (GFM) | All prompts, configuration, and documentation | YAML + Jinja2 templates | Enables parameterization but adds tooling dependency; less readable as prose; requires preprocessing step |
| Implementation Language | Markdown (GFM) | All prompts, configuration, and documentation | JSON Schema + natural language | Enables machine validation but doubles token consumption; poor human ergonomics |
| Diagramming | Mermaid.js (in fenced code blocks) | Architecture, flow, and state diagrams | PlantUML | Requires Java runtime; not natively rendered by GitHub; less LLM familiarity |
| Diagramming | Mermaid.js (in fenced code blocks) | Architecture, flow, and state diagrams | D2 (Terrastruct) | Newer, less ecosystem support; not natively rendered by most platforms |
| Orchestration | Custom MASTER_PROMPT.md | Prompt sequencing, gating, context management | LangChain / LangGraph (Python) | Programmatic control but removes human-in-loop; adds infrastructure dependency; requires deployment |
| Orchestration | Custom MASTER_PROMPT.md | Prompt sequencing, gating, context management | CrewAI / AutoGen (Python) | Multi-agent coordination but over-engineered for linear pipeline; adds latency and cost |
| Runtime Environment | Human-in-loop agentic IDE | File access, execution monitoring, quality judgment | Fully autonomous script | Removes human oversight; risks undetected hallucination; scales better but quality degrades |
| Runtime Environment | Human-in-loop agentic IDE | File access, execution monitoring, quality judgment | CLI batch runner | Faster execution but no interactive remediation; quality gates become pass/fail without nuance |
| Configuration | Infrastructure Markdown files | Rules, standards, checklists as natural language | YAML/TOML config files | Machine-parseable but not LLM-native; creates translation layer; loses nuance of natural language rules |
| Quality Assurance | Self-audit via VALIDATION_CHECKLISTS.md | Per-phase quality gates | External test harness (pytest, Jest) | Requires code infrastructure; cannot validate prose quality; only checks structural properties |
| Version Control | Git | Track framework and output changes | None | Loss of change history; no collaboration; no rollback capability |

---

## 5. Technology Decisions and Trade-offs

### Why Not a Python/Node Orchestrator?

The framework deliberately avoids a programmatic orchestration layer (LangChain, AutoGen, custom Python scripts) for these reasons:

1. **Zero deployment complexity:** No virtual environments, no dependency conflicts, no Docker builds, no CI/CD pipelines. The framework is immediately usable by loading a single file.

2. **Human-in-loop by default:** The agentic IDE paradigm keeps a human operator in the loop at all times. Quality failures are caught interactively rather than logged to a file for later review.

3. **Transparency:** Every instruction the agent receives is readable Markdown. There is no opaque orchestration logic hidden in compiled code. The framework is auditable by reading it.

4. **Model portability:** The same framework works with any LLM that meets the context window and instruction-following requirements. No API-specific code, no SDK version pinning, no model-specific adapters.

### Why Not Structured Data Formats?

Prompts are written in free-form Markdown rather than structured YAML/JSON because:

1. **LLMs are language models:** They perform best when instructions are expressed in natural language, not configuration syntax.
2. **Nuance preservation:** Rules like "speculative claims must be prefixed with [SPECULATIVE]" require natural language precision that structured formats cannot capture without verbose description fields.
3. **Authoring velocity:** Framework designers can iterate on prompts as quickly as writing prose, without schema validation overhead.

---

## Cross-References

- [System Design](./04-architecture/system-design.md) - Architecture that the tech stack enables
- [AI Agent Workflow](./06-ai-agent-workflow.md) - How the runtime environment executes the framework
- [Component Map](./04-architecture/component-map.md) - Individual file roles in the stack
- [Prompt Template Docs](./03-prompt-template-docs/03-prompt-template-docs.md) - Structure of the Markdown prompts
