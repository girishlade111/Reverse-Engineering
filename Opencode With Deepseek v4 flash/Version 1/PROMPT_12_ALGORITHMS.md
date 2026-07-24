# PROMPT_12 — Phase 11: Algorithm Extraction

## PHASE CLASS: Logic Analysis
## DEPENDENCIES: PROMPT_11 (Features) — complete
## OUTPUT DIRECTORY: `re-docs/11-algorithms/`

---

## OBJECTIVE

Extract, document, and analyze every significant algorithm in the system. Document business logic, mathematical models, search/sort algorithms, data processing pipelines, heuristics, and decision procedures.

## PREREQUISITES

- [ ] PROMPT_11 completed
- [ ] Features are mapped
- [ ] Functions are cataloged
- [ ] Control flow is analyzed

## INPUTS

- `re-docs/06-deep-read/03-function-catalog.md`
- `re-docs/09-call-graph/02-control-flow-analysis.md`
- Full source code

## ANALYSIS STEPS

### Step 1: Algorithm Identification

Identify all significant algorithms in the codebase:

| Category | Examples | Where to Look |
|----------|----------|---------------|
| **Search** | Binary search, linear scan, graph search | Search services, finder functions |
| **Sort** | Custom sort, multi-key sort | List ordering, ranking |
| **Transform** | Data mapping, format conversion | Data processing pipelines |
| **Validation** | Business rule validation | Service layer, validation layer |
| **Calculation** | Math, statistics, scoring | Pricing, scoring, analytics |
| **Optimization** | Resource allocation, scheduling | Background jobs, planners |
| **Machine Learning** | Model inference, feature extraction | ML services, prediction |
| **Crypto** | Encryption, hashing, signing | Auth, security modules |
| **Parsing** | Text parsing, code parsing, DSL parsing | Input processors |
| **Matching** | Pattern matching, fuzzy matching | Search, recommendations |
| **State Machine** | State transitions, guards | Workflow engines |
| **Rate Limiting** | Token bucket, leaky bucket | Middleware |
| **Caching** | Eviction policies, cache strategies | Cache layer |

### Step 2: Algorithm Documentation

For each significant algorithm, produce this documentation:

```markdown
## Algorithm: Password Strength Scoring

### Location
src/auth/password.ts:15-80

### Purpose
Score password strength on a scale of 0-100 for password validation.

### Input
- password: string

### Output
- score: number (0-100)
- feedback: string[] (list of improvement suggestions)

### Algorithm

1. Start with score = 0
2. Length check: +10 points per character over 8 (max 40)
3. Uppercase check: +10 if contains uppercase
4. Lowercase check: +10 if contains lowercase
5. Number check: +10 if contains number
6. Special character check: +10 if contains special char
7. Repeated character penalty: -5 per repeated sequence
8. Common pattern check: -20 if matches common password list
9. Clamp to 0-100

### Pseudocode

```
function scorePassword(password):
    score = 0
    if length(password) >= 8:
        score += min(40, (length(password) - 8) * 10)
    if containsUppercase(password): score += 10
    if containsLowercase(password): score += 10
    if containsNumber(password): score += 10
    if containsSpecialChar(password): score += 10
    score -= countRepeatedSequences(password) * 5
    if isCommonPassword(password): score -= 20
    return clamp(score, 0, 100)
```

### Complexity
Time: O(n) where n is password length
Space: O(1)

### Edge Cases
- Empty password: returns 0
- Very long password: max length capped at 128 characters
- Unicode characters: treated as special characters

### Test Coverage
src/__tests__/password.test.ts covers: normal passwords, edge cases,
common passwords, empty input
```

### Step 3: Business Rule Extraction

Extract all business rules from the code:

```markdown
## Business Rules

### Rule: Order Discount Calculation

- **Rule**: Orders over $100 get 10% discount
- **Location**: src/orders/discount.ts:25
- **Logic**: 
  ```
  if order.total >= 100:
      order.discount = order.total * 0.10
  ```
- **Exceptions**: VIP customers always get 15% regardless of order total
- **Evidence**: `src/orders/discount.ts:25-42`

### Rule: Account Lockout

- **Rule**: Account locks after 5 failed login attempts
- **Location**: src/auth/lockout.ts:30
- **Logic**: 
  ```
  if failedAttempts >= 5:
      lockAccount(24 hours)
  ```
- **Exception**: Admin accounts never lock
- **Evidence**: `src/auth/lockout.ts:30-55`
```

### Step 4: Mathematical Model Extraction

If the system contains mathematical algorithms:

- Extract the mathematical model
- Document input variables
- Document formulas (using LaTeX notation)
- Document assumptions
- Document edge cases

### Step 5: Heuristics and Decision Logic

Document all heuristic algorithms and decision procedures:

```markdown
## Heuristic: Product Recommendation

### Location
src/recommendations/engine.ts:40-120

### Approach
Collaborative filtering with popularity fallback.

### Decision Logic
1. If user has purchase history:
   a. Find similar users (same categories purchased)
   b. Recommend products similar users bought
   c. Rank by similarity score * popularity score
2. If user has no purchase history:
   a. Recommend popular products in viewed categories
   b. Rank by popularity
3. If no data at all:
   a. Recommend trending products globally
```

### Step 6: Complexity Analysis

For each algorithm, determine:

- **Time complexity**: Big-O notation
- **Space complexity**: Big-O notation
- **Best case**: When is it fastest?
- **Worst case**: When is it slowest?
- **Performance concerns**: Are there any?

## OUTPUT SPECIFICATION

### File 1: `01-algorithm-catalog.md`

Catalog of all identified algorithms.

### File 2: `02-algorithm-details.md`

Detailed documentation of each algorithm.

### File 3: `03-business-rules.md`

All extracted business rules.

### File 4: `04-mathematical-models.md` (if applicable)

Mathematical model documentation.

### File 5: `05-heuristics.md`

Heuristic and decision logic documentation.

### File 6: `06-complexity-analysis.md`

Complexity analysis for all algorithms.

### File 7: `07-algorithms-summary.md`

Summary including:
- Total algorithms documented
- Complexity distribution
- Performance concerns
- Optimization opportunities

## REQUIRED DIAGRAMS

### Flowchart: Algorithm Flow

```mermaid
flowchart TD
    A[Input: password] --> B{Length check}
    B -->|>= 8 chars| C[Calculate length score]
    B -->|< 8 chars| D[score = 0]
    C --> E{Uppercase?}
    D --> E
    E -->|Yes| F[+10 points]
    E -->|No| G{Lowercase?}
    F --> G
    G --> ...rest of checks...
    ... --> Z[Output: score 0-100]
```

## VALIDATION CHECKS

- [ ] All algorithms of significance are identified
- [ ] Each algorithm has documented input, output, and logic
- [ ] Business rules are extracted and documented
- [ ] Each algorithm has complexity analysis
- [ ] Edge cases are documented for each algorithm
- [ ] Heuristics are documented (if applicable)

## COMPLETION CHECKLIST

- [ ] All 7 output files generated
- [ ] Algorithm catalog complete
- [ ] Algorithm details documented
- [ ] Business rules extracted
- [ ] Complexity analysis performed
- [ ] Edge cases documented
- [ ] All outputs saved to `re-docs/11-algorithms/`
- [ ] Phase validation checks passed

---

*Proceed to PROMPT_13_DESIGN_PATTERNS.md only after all checklist items are complete.*
