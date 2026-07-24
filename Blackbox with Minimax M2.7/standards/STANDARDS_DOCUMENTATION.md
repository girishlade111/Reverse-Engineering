# Documentation Standards

> **Document:** standards/STANDARDS_DOCUMENTATION.md  
> **Version:** 1.0.0  
> **Purpose:** Define standards for all generated documentation  
> **When to Use:** During Phase 9 when generating documentation

---

## 📐 STANDARD D1: DOCUMENT STRUCTURE

### D1.1 Required Front Matter
Every documentation file MUST begin with:

```markdown
# [Document Title]

> **Document:** [filename.md]  
> **Phase:** [Phase Name]  
> **Last Updated:** [YYYY-MM-DD]  
> **Purpose:** [Brief description of this document's purpose]

---
```

### D1.2 Standard Section Order
1. **Title** (H1)
2. **Front Matter** (blockquote metadata)
3. **Purpose/Overview** (2-3 sentences)
4. **Main Content** (structured with H2, H3 headings)
5. **Confidence Assessment** (statement of confidence)
6. **Cross-References** (related documents)
7. **Changelog** (optional, for updates)

### D1.3 Heading Hierarchy
- H1: Document title (one per file)
- H2: Major sections
- H3: Sub-sections
- H4: Sub-sub-sections (use sparingly)
- **No skipping levels** (H1 → H3 is not allowed)

---

## 📐 STANDARD D2: WRITING STYLE

### D2.1 Voice and Tone
- **Active voice:** "The service processes requests" NOT "Requests are processed by the service"
- **Technical precision:** Use exact terminology from the codebase
- **Objectivity:** Describe what the code does, not what it should do
- **Professionalism:** Avoid slang, humor, or casual language

### D2.2 Terminology Rules
1. Define acronyms on first use: "REST (Representational State Transfer)"
2. Use consistent terminology throughout all documents
3. Match codebase terminology (if the code uses "Account," don't call it "User")
4. Avoid synonyms for the same concept (don't use "User" and "Account" interchangeably)
5. Use code font (\`code\`) for: file names, function names, variable names, class names

### D2.3 Prohibited Language
- ❌ "It seems to..." / "It appears to..." → Use "The code indicates..." or state confidence
- ❌ "Obviously" / "Clearly" → If it's obvious, no need to say so
- ❌ "Just" / "Simply" → Minimizes complexity
- ❌ "Basically" / "Essentially" → Vague filler

---

## 📐 STANDARD D3: CODE INCLUSION

### D3.1 Code Block Standards
- Always specify language: \```python
- Include relevant imports for context
- Show 3-5 lines of context before/after key code
- Never truncate with "..." or "// rest of code"
- Verify all code examples compile/run correctly

### D3.2 When to Include Code
Include code when:
- Documenting an API endpoint (show request/response)
- Documenting a complex algorithm
- Showing usage examples
- Illustrating a design pattern implementation
- Showing configuration examples

### D3.3 Code Commentary
- Annotate code with comments to explain non-obvious parts.
- Keep annotations concise.
- Use code comments (//, #, /* */) for inline annotations.

---

## 📐 STANDARD D4: CROSS-REFERENCES

### D4.1 Cross-Reference Format
```markdown
**See Also:**
- [Document Name](path/to/document.md) — Brief description of relationship
- [Related Concept](../05_ARCHITECTURE/SYSTEM_ARCHITECTURE.md#section)
```

### D4.2 Cross-Reference Requirements
- Every document must reference at least one related document.
- Every component must reference its parent module.
- Every workflow must reference the components it involves.
- Cross-references should include a brief description of the relationship.

### D4.3 Bidirectional References
- If Document A references Document B, Document B should reference Document A.
- Exception: Reference documents (INDEX, glossary) that are referenced by many.

---

## 📐 STANDARD D5: TABLES

### D5.1 Table Format
```markdown
| Column 1 | Column 2 | Column 3 |
|----------|----------|----------|
| Value 1  | Value 2  | Value 3  |
```

### D5.2 Table Guidelines
- Always include a header row.
- Use alignment dashes (---, :---, ---:, :---:) as appropriate.
- Keep tables scannable (5-10 rows maximum; use multiple tables if needed).
- Include a caption/description before the table.

---

## 📐 STANDARD D6: LISTS

### D6.1 Bullet Lists
- Use for unordered items where order doesn't matter.
- Keep items parallel in structure (all nouns, all verbs, etc.).
- Avoid more than 2 levels of nesting.

### D6.2 Numbered Lists
- Use for sequential steps or ranked items.
- Each step should be actionable.
- Keep steps atomic (one action per step).

---

## 📐 STANDARD D7: FILE NAMING

### D7.1 Naming Convention
- Use UPPER_SNAKE_CASE for documentation files.
- Use descriptive names: `SYSTEM_ARCHITECTURE.md`, not `ARCH.md`.
- Prefix with phase number for phase-specific files.

### D7.2 Directory Structure
Follow the output directory structure defined in OUTPUT_RULES.md exactly.

---

## 📐 STANDARD D8: METADATA

### D8.1 File Level Metadata
Every file must include in the front matter:
- Title
- Document identifier (relative path)
- Phase
- Last updated date
- Purpose statement

### D8.2 Code Element Metadata
Documented functions/classes should include:
- File path and line number
- Purpose
- Parameters (name, type, description)
- Return value (type, description)
- Errors thrown
- Dependencies (called by, calls)
- Complexity (for algorithms)

---

## ✅ COMPLIANCE CHECK

- [ ] Front matter present in all documents
- [ ] Writing style follows active voice, technical precision
- [ ] Terminology consistent across documents
- [ ] Code examples verified and properly formatted
- [ ] Cross-references present and bidirectional
- [ ] Tables properly formatted
- [ ] File naming follows convention
- [ ] Metadata complete for all documented elements

