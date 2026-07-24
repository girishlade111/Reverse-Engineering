# PROMPT_11: Algorithm Extraction & Documentation

## Classification
- **Domain:** Deep Code Intelligence
- **Phase:** 3 — Detailed Code Analysis
- **Prerequisites:** PROMPT_10 (Class & Function Analysis)
- **Dependencies:** Class/function catalog
- **Estimated Effort:** High (identifying and documenting all algorithms)

---

## Objective

Identify, extract, and document every distinct algorithm implemented in the repository. For each algorithm, document its purpose, logic, complexity, inputs, outputs, and usage context.

---

## Input Requirements

### Required Context
- Class and function analysis from PROMPT_10
- Data flow mappings from PROMPT_08
- Component interfaces from PROMPT_07

---

## Pre-Analysis Checklist
- [ ] PROMPT_10 completed and class/function catalog available
- [ ] Key functions identified for algorithm analysis

---

## Analysis Tasks

### Task 1: Algorithm Identification

**Purpose:** Identify all distinct algorithms in the codebase.

**Instructions:**
1. Scan all significant functions for algorithmic patterns:
   - **Search algorithms:** Binary search, linear search, hash-based lookup
   - **Sort algorithms:** Quicksort, mergesort, heapsort, built-in sort
   - **Graph algorithms:** BFS, DFS, shortest path, topological sort
   - **String algorithms:** Pattern matching, parsing, regex
   - **Math algorithms:** Statistical calculations, financial calculations, cryptography
   - **Optimization algorithms:** Dynamic programming, greedy algorithms
   - **Machine learning:** Model inference, feature extraction
   - **Data processing:** Aggregation, filtering, transformation pipelines
2. Classify each algorithm by type and complexity

**Output Format:**

```markdown
## Algorithm Inventory

| Algorithm | Type | Location | Complexity | Used By |
|-----------|------|----------|------------|---------|
| Password hashing | Cryptographic | src/auth/password.py | O(1) | AuthService |
| Email validation | String/Pattern | src/validators/email.py | O(n) | UserValidator |
| Order total calculation | Math/Aggregation | src/orders/services/pricing.py | O(n) | OrderService |
| Route optimization | Graph/Shortest Path | src/logistics/router.py | O(V+E log V) | LogisticsService |
| Fraud detection | ML/Classification | src/ml/fraud_detector.py | O(n) | PaymentService |
| Rate limiting | Algorithm/Sliding Window | src/middleware/rate_limit.py | O(1) | APIGateway |
```

---

### Task 2: Algorithm Deep Documentation

**Purpose:** Document each algorithm with complete detail.

**Instructions:**
1. For each identified algorithm, document:
   - **Purpose:** What problem does it solve?
   - **Input:** Parameters with types and constraints
   - **Output:** Return value with type
   - **Logic:** Step-by-step pseudocode or detailed description
   - **Complexity:** Time and space complexity (best, average, worst case)
   - **Edge Cases:** How it handles empty input, duplicates, boundary values
   - **Correctness:** Invariants, pre/post conditions
   - **Implementation Notes:** Any optimizations, trade-offs, or nuances

**Output Format:**

```markdown
## Algorithm: Route Optimization

### Purpose
Find the optimal delivery route that minimizes total distance while respecting time windows and vehicle capacity constraints.

### Classification
- **Type:** Graph/Shortest Path with Constraints (VRPTW variant)
- **Pattern:** Modified Dijkstra with constraint checking

### Input
```python
def optimize_route(
    deliveries: List[Delivery],
    vehicles: List[Vehicle],
    depot: Location,
    time_windows: Dict[str, TimeWindow],
    max_route_time: int = 480  # minutes
) -> List[Route]:
```

### Algorithm Logic (Pseudocode)
```
1. Build distance matrix between all delivery locations and depot
2. Sort deliveries by time window start (earliest first)
3. For each vehicle:
   a. Initialize route at depot
   b. Find nearest unassigned delivery within time window
   c. Check capacity constraint
   d. If within constraints, assign to route
   e. Update current time and location
   f. Repeat until no more deliveries fit or time exceeded
4. If unassigned deliveries remain, try next vehicle
5. Return all routes

Optimization: Apply 2-opt improvement to each route
```

### Complexity
| Case | Time Complexity | Space Complexity |
|------|----------------|------------------|
| Best | O(n log n) | O(n^2) |
| Average | O(n^2 log n) | O(n^2) |
| Worst | O(n^3) | O(n^2) |

### Edge Cases
| Case | Behavior |
|------|----------|
| Empty delivery list | Return empty routes |
| Single delivery | Direct route depot -> delivery -> depot |
| All time windows overlap | Sort by distance, ignore time |
| No feasible route | Return partial assignment, flag unassigned |
| Vehicle capacity exceeded | Skip delivery, try next vehicle |

### Correctness Invariants
- Every delivery assigned to at most one route
- Vehicle capacity never exceeded
- Time windows respected for all deliveries
- All routes start and end at depot
```

---

### Task 3: Algorithm Usage & Context Mapping

**Purpose:** Map where and how each algorithm is used in the system.

**Instructions:**
1. For each algorithm, document:
   - All call sites (where it's invoked)
   - Context of invocation (what module, what flow)
   - How results are used
   - Alternative algorithms considered (if detectable from comments)

**Output Format:**

```markdown
## Algorithm Usage Map

| Algorithm | Call Sites | Flow | Context |
|-----------|------------|------|---------|
| Password hashing | AuthService.register(), AuthService.change_password() | User registration, password change | Security |
| Route optimization | LogisticsService.plan_deliveries() | Daily delivery planning | Operations |
| Fraud detection | PaymentService.process_payment() | Payment processing | Risk management |
```

---

## Synthesis
**Purpose:** Create a comprehensive algorithm reference.

**Output Format:**

```markdown
## Algorithm Catalog Summary

| Algorithm | Type | Complexity | Criticality | Usage Count |
|-----------|------|------------|-------------|-------------|
| Password hashing | Cryptographic | O(1) | CRITICAL | 3 |
| Route optimization | Graph | O(n^2 log n) | HIGH | 1 |
| Fraud detection | ML | O(n) | HIGH | 1 |
| Rate limiting | Sliding Window | O(1) | MEDIUM | 1 |
```

---

## Output Requirements
### Required Deliverables
1. Complete algorithm inventory
2. Deep documentation for each algorithm
3. Algorithm usage and context map

---

## Cross-References
- **Previous Prompt:** PROMPT_10_CLASS_FUNCTION_ANALYSIS.md
- **Next Prompt:** PROMPT_12_DESIGN_PATTERN_DETECTION.md
- **Shared Context Key:** algorithms.inventory, algorithms.documentation
