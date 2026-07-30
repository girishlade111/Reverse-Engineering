# Business Logic

This repository does not contain a traditional business domain (like e-commerce or finance). The "business rules" are the prompt engineering constraints that govern the LLM's behavior:

1. **Evidence over Inference Rule:** Every documented behavior must trace to actual code read. If an implementation detail is not visible, it must be tagged as `[UNVERIFIED - needs confirmation]`.
2. **Continuation Rule:** The LLM must not stop mid-table or mid-diagram. If truncation is imminent, it must emit exactly `--- CONTINUING IN NEXT MESSAGE: [next section name] ---` and resume automatically.
3. **Pacing Rule:** The LLM must prioritize depth over speed. A thorough multi-message response for a single phase is preferred over a rushed one-shot summary.
4. **Anti-Hallucination Rules:**
   * Never invent a function's purpose from its name alone.
   * Never assume standard implementations (e.g., standard JWT usage) without verifying.
