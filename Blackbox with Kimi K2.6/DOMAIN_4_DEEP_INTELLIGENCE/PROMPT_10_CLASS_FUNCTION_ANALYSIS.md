# PROMPT_10: Class & Function Deep Analysis

## Classification
- **Domain:** Deep Code Intelligence
- **Phase:** 3 — Detailed Code Analysis
- **Prerequisites:** PROMPT_06, PROMPT_07 (Component Analysis)
- **Dependencies:** Component interfaces, module decomposition
- **Estimated Effort:** Very High (requires analyzing every class and function)

---

## Objective

Perform a complete, deep analysis of every class and significant function in the repository. Document signatures, behavior, internal logic, edge cases, and relationships to produce a comprehensive catalog of all code-level artifacts.

---

## Input Requirements

### Required Context
- Component inventory and interfaces from PROMPT_07
- Module decomposition from PROMPT_06
- Data flow mappings from PROMPT_08

### Required Files
- All source files containing class definitions
- All source files containing function definitions

---

## Pre-Analysis Checklist

- [ ] PROMPT_06, PROMPT_07, PROMPT_08 completed and context loaded
- [ ] Component interfaces documented
- [ ] Module boundaries understood

---

## Analysis Tasks

### Task 1: Class Deep Analysis

**Purpose:** Document every class in the repository with complete detail.

**Instructions:**
1. For each class, document:

   - **Identity:** Class name, full path, line numbers
   - **Classification:** Entity, Value Object, Service, Controller, Repository, Factory, Builder, Utility, Configuration, Exception, DTO, Model, ViewModel, Singleton, Abstract class, Interface/Protocol, Mixin
   - **Inheritance:** Parent class, implemented interfaces, mixins
   - **Lifecycle:** Who creates instances, when, how disposed
   - **State:** Instance variables with types and purposes
   - **Public API:** All public methods with complete signatures
   - **Protected/Private API:** Internal methods with purposes
   - **Dependencies:** External services, repositories, utilities injected
   - **Thread Safety:** Immutable, synchronized, thread-local, not thread-safe
   - **Design Pattern:** Singleton, Factory, Strategy, Observer, etc.

2. Analyze internal behavior:

   - Constructor/initialization logic
   - Key method algorithms
   - State management
   - Error handling patterns
   - Resource management (open/close, acquire/release)

**Output Format:**

```
markdown
## Class Analysis

### Class: `UserService`
| Aspect | Detail |
|--------|--------|
| **Path** | src/users/services/user_service.py:10-150 |
| **Type** | Service |
| **Pattern** | None (plain class) |
| **Parent** | BaseService (abstract) |
| **Interfaces** | IUserService (Protocol) |
| **Lifecycle** | Singleton (injected by DI container) |
| **Thread Safety** | Not thread-safe (uses shared session) |

#### State
| Variable | Type | Purpose | Initialized |
|----------|------|---------|-------------|
| user_repository | UserRepository | Data access | Constructor |
| auth_service | AuthService | Authentication | Constructor |
| notification_service | NotificationService | Notifications | Constructor |
| _cache | dict | Local cache | Constructor |

#### Public API
| Method | Signature | Returns | Side Effects |
|--------|-----------|---------|--------------|
| create_user | (data: CreateUserDTO) -> User | User | DB insert, email notification |
| get_user | (user_id: UUID) -> User | User | None |
| update_user | (user_id: UUID, data: UpdateUserDTO) -> User | User | DB update |
| delete_user | (user_id: UUID) -> None | None | DB delete, cleanup |
| find_by_email | (email: str) -> Optional[User] | Optional[User] | None |

#### Private API
| Method | Purpose |
|--------|---------|
| _validate_email_uniqueness | Check email not taken |
| _send_welcome_notification | Send async welcome |
| _invalidate_cache | Clear cached entries |

#### Algorithm: create_user
```
1. Validate input data (delegate to UserValidator)
2. Check email uniqueness
3. Hash password (delegate to PasswordHasher)
4. Create User entity
5. Save via UserRepository
6. Send welcome notification (async, fire-and-forget)
7. Return created User
```

#### Error Handling
| Error | Where Handled | Action |
|-------|---------------|--------|
| DuplicateEmailError | create_user step 2 | Raise with field details |
| ValidationError | create_user step 1 | Return 400 with errors |
| DatabaseError | create_user step 5 | Log, raise ServiceError |
```

---

### Task 2: Function Deep Analysis

**Purpose:** Document every significant function in the repository.

**Instructions:**
1. For each significant function (public API, complex internal, critical path), document:
   - **Signature:** Name, parameters with types, return type
   - **Purpose:** One-sentence description
   - **Algorithm:** Step-by-step logic
   - **Control Flow:** Conditions, loops, recursion
   - **Error Handling:** Exceptions raised, error returns
   - **Edge Cases:** Empty input, null values, boundary conditions
   - **Performance:** Time complexity, space complexity
   - **Dependencies:** Other functions called
   - **Side Effects:** State changes, I/O, network calls

**Output Format:**

```
markdown
## Function Analysis

### Function: `process_order_payment(order_id: UUID, payment_method: PaymentMethod) -> PaymentResult`
| Aspect | Detail |
|--------|--------|
| **Path** | src/orders/services/payment_processor.py:30-85 |
| **Purpose** | Process payment for an order |
| **Visibility** | Public |

#### Signature
```python
async def process_order_payment(
    order_id: UUID,
    payment_method: PaymentMethod,
    idempotency_key: Optional[str] = None
) -> PaymentResult:
```

#### Algorithm
```
1. Validate order_id exists and is in 'pending_payment' status
2. Check idempotency_key (if provided) to prevent duplicate processing
3. Calculate total from order items
4. Choose payment gateway based on payment_method.type
5. Call gateway.charge(amount, currency, payment_details)
6. On success:
   a. Update order status to 'paid'
   b. Create payment transaction record
   c. Emit 'payment.completed' event
   d. Return PaymentResult(success=True, transaction_id)
7. On failure:
   a. Log failure reason
   b. Update order status to 'payment_failed'
   c. Emit 'payment.failed' event
   d. Return PaymentResult(success
