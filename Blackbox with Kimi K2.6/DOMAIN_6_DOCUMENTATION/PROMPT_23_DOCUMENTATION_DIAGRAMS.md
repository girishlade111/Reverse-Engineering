# PROMPT_23: Diagram & Visual Documentation Generation

## Classification
- **Domain:** Documentation Generation
- **Phase:** 5 — Documentation Production
- **Prerequisites:** All Phase 1-4 prompts (01-19)
- **Dependencies:** Complete analysis context
- **Estimated Effort:** High

---

## Objective

Generate all visual documentation artifacts including architecture diagrams, sequence diagrams, flowcharts, UML diagrams, state machine diagrams, and dependency graphs using Mermaid.js and other diagramming standards.

---

## Input Requirements

### Required Context
- All architectural findings from Phase 2-4
- Data flow mappings from PROMPT_08
- State machines from PROMPT_14
- Execution pipelines from PROMPT_15
- Event workflows from PROMPT_16
- Dependency graph from PROMPT_09

---

## Analysis Tasks

### Task 1: Architecture Diagrams
**Purpose:** Generate all architecture-level diagrams.

**Instructions:**
1. Create the following diagrams:
   - System context diagram (C4 Level 1)
   - Container diagram (C4 Level 2)
   - Component diagram (C4 Level 3)
   - Deployment diagram
   - Network architecture diagram

**Output Format:**

```markdown
## Architecture Diagrams

### System Context Diagram (C4 Level 1)
```mermaid
graph TB
    subgraph "Users"
        CU[Customer]
        AU[Admin]
    end
    
    subgraph "E-Commerce System"
        ES[E-Commerce Platform]
    end
    
    subgraph "External Systems"
        PS[Payment System<br/>Stripe]
        ESys[Email System<br/>SendGrid]
        FS[File Storage<br/>AWS S3]
    end
    
    CU -->|"Browse products<br/>Place orders"| ES
    AU -->|"Manage products<br/>View analytics"| ES
    ES -->|"Process payments"| PS
    ES -->|"Send notifications"| ESys
    ES -->|"Store files"| FS
```

### Container Diagram (C4 Level 2)
```mermaid
graph TB
    subgraph "Single Server"
        subgraph "Web Container"
            FE[React SPA<br/>TypeScript]
        end
        subgraph "API Container"
            API[FastAPI Server<br/>Python]
        end
        subgraph "Worker Container"
            CW[Celery Worker<br/>Python]
        end
    end
    
    subgraph "Data Stores"
        DB[(PostgreSQL<br/>Primary Database)]
        RD[(Redis<br/>Cache & Queue)]
    end
    
    FE -->|"HTTPS/REST"| API
    API -->|"SQL"| DB
    API -->|"Commands"| RD
    CW -->|"Tasks"| RD
    CW -->|"SQL"| DB
```
---

### Task 2: Behavioral Diagrams
**Purpose:** Generate all behavioral and process diagrams.

**Instructions:**
1. Create the following diagrams:
   - Sequence diagrams for key flows
   - State machine diagrams
   - Activity diagrams for complex processes
   - Event flow diagrams

**Output Format:**

```markdown
## Behavioral Diagrams

### Sequence Diagram: Order Placement
```mermaid
sequenceDiagram
    participant C as Client
    participant A as API
    participant O as OrderService
    participant P as PaymentService
    participant I as InventoryService
    participant N as NotificationService
    participant DB as Database
    
    C->>A: POST /orders
    A->>O: create_order()
    O->>DB: Save order (status: pending)
    O->>I: reserve_inventory()
    I-->>O: Inventory reserved
    O->>P: process_payment()
    P-->>O: Payment successful
    O->>DB: Update order (status: confirmed)
    O->>N: send_confirmation()
    N-->>O: Confirmation sent
    O-->>A: Order created
    A-->>C: 201 Created
```

### State Machine Diagram: Order Lifecycle
```mermaid
stateDiagram-v2
    [*] --> Pending
    Pending --> Confirmed: Payment received
    Pending --> Cancelled: Payment failed
    Confirmed --> Processing: Inventory reserved
    Processing --> Shipped: Dispatched
    Shipped --> Delivered: Confirmed
    Shipped --> Returned: Return requested
    Delivered --> Returned: Return requested
    Returned --> Refunded: Refund processed
    Cancelled --> [*]
    Refunded --> [*]
```
---

### Task 3: Structural Diagrams
**Purpose:** Generate all structural diagrams.

**Instructions:**
1. Create the following diagrams:
   - Class diagrams for key domain models
   - Component diagrams for module structure
   - Package/dependency diagrams
   - Database ER diagrams

**Output Format:**

```markdown
## Structural Diagrams

### Class Diagram: Domain Models
```mermaid
classDiagram
    class User {
        +UUID id
        +String email
        +String name
        +String role
        +DateTime created_at
        +create_order() Order
        +get_profile() Profile
    }
    
    class Order {
        +UUID id
        +UUID user_id
        +OrderStatus status
        +Decimal total
        +DateTime created_at
        +process_payment() bool
        +cancel() void
    }
    
    class OrderItem {
        +UUID id
        +UUID order_id
        +UUID product_id
        +Integer quantity
        +Decimal price
    }
    
    class Product {
        +UUID id
        +String name
        +Decimal price
        +Integer stock
        +String category
    }
    
    User "1" --> "*" Order
    Order "1" --> "*" OrderItem
    OrderItem "*" --> "1" Product
```
---

### Task 4: Infrastructure & Deployment Diagrams
**Purpose:** Generate infrastructure and deployment diagrams.

**Instructions:**
1. Create the following diagrams:
   - Deployment architecture diagram
   - Network topology diagram
   - CI/CD pipeline diagram
   - Data flow diagram

**Output Format:**

```markdown
## Infrastructure Diagrams

### Deployment Architecture
```mermaid
graph TB
    subgraph "Production Environment"
        subgraph "Load Balancer"
            LB[Nginx<br/>Load Balancer]
        end
        
        subgraph "Application Servers"
            API1[FastAPI Instance 1]
            API2[FastAPI Instance 2]
            API3[FastAPI Instance 3]
        end
        
        subgraph "Worker Pool"
            W1[Celery Worker 1]
            W2[Celery Worker 2]
        end
        
        subgraph "Data Layer"
            DB[(PostgreSQL<br/>Primary)]
            DB_R[(PostgreSQL<br/>Read Replica)]
            RD[(Redis<br/>Cluster)]
        end
        
        subgraph "External"
            S3[AWS S3]
            CDN[CloudFront CDN]
        end
    end
    
    LB --> API1
    LB --> API2
    LB --> API3
    API1 --> DB
    API2 --> DB
    API3 --> DB_R
    API1 --> RD
    W1 --> RD
    W1 --> DB
    API1 --> S3
    S3 --> CDN
```
---

## Output Requirements
### Required Deliverables
1. Architecture diagrams (C4 model levels 1-3)
2. Behavioral diagrams (sequence, state, activity)
3. Structural diagrams (class, component, package)
4. Infrastructure and deployment diagrams

### Output Structure
```
DOCUMENTATION_DIAGRAMS/
├── architecture_diagrams.md
├── behavioral_diagrams.md
├── structural_diagrams.md
└── infrastructure_diagrams.md
```

---

## Quality Checks
- [ ] All diagrams use Mermaid.js syntax
- [ ] Diagrams are focused (max 15 nodes per diagram)
- [ ] All diagrams have descriptive captions
- [ ] Complex systems use multiple focused diagrams
- [ ] Diagrams render correctly in standard Markdown viewers
- [ ] All abbreviations are defined in captions

---

## Cross-References
- **Previous Prompt:** PROMPT_22_DOCUMENTATION_DEVELOPER.md
- **Next Prompt:** PROMPT_24_DOCUMENTATION_QUALITY.md
- **Shared Context Key:** documentation.diagrams
