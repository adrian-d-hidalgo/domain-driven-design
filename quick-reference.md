# Quick Reference

Fast lookup for DDD patterns, comparisons, and decision trees.

## Pattern Comparison Table

| Pattern          | Identity              | Mutability         | Lifespan    | Purpose                             |
| ---------------- | --------------------- | ------------------ | ----------- | ----------------------------------- |
| **Entity**       | Yes (UserId, OrderId) | Mutable            | Long-lived  | Track objects that change over time |
| **Value Object** | No                    | Immutable          | Short-lived | Describe characteristics            |
| **Aggregate**    | Yes (Root Entity)     | Mutable (as whole) | Long-lived  | Enforce consistency boundaries      |
| **Domain Event** | Yes (EventId)         | Immutable          | Permanent   | Record facts about the past         |

## When to Use Each Tactical Pattern

### Entity

**Use when:**

* Need to track something over time
* Object can change but remains the same thing
* Identity matters more than attributes

**Examples:** `User`, `Order`, `Product`, `Account`

**Common Misconceptions:**

* ❌ "Entities always have database IDs" → Entities can have domain-generated IDs (UUID, ULID) before persistence
* ❌ "Entities are just objects with IDs" → They have behavior, enforce invariants, and emit domain events
* ❌ "Entities must be mutable" → Entities should have intention-revealing methods, not generic setters

### Value Object

**Use when:**

* Describes a characteristic
* Two instances with same values are interchangeable
* No lifecycle or identity needed

**Examples:** `Email`, `Money`, `Address`, `DateRange`

**Common Misconceptions:**

* ❌ "Value Objects are just primitives wrapped in classes" → They can be complex with multiple fields and rich behavior
* ❌ "Value Objects can't have methods" → They can have methods for calculations, formatting, and comparisons
* ❌ "Value Objects with same values are literally the same object" → Different instances, same equality semantics
* ❌ "Immutability means no methods" → Methods return new instances instead of mutating

### Aggregate

**Use when:**

* Need to enforce business rules across multiple objects
* Clear consistency boundary exists
* Transaction boundary needed

**Examples:** `Order` (with `OrderItems`), `ShoppingCart`, `Reservation`

**Design Principles:**

1. **Transaction Boundaries Match Aggregate Boundaries** - If objects must change together, they're in the same aggregate
2. **Keep Aggregates Small** - Aim for 1-5 entities, rarely more than 10
3. **Reference Other Aggregates by Identity Only** - Hold IDs, not object references
4. **Use Eventual Consistency Between Aggregates** - Coordinate via domain events

**Common Mistakes:**

* ❌ **God Aggregate** - Including too many unrelated entities (hundreds of objects)
* ❌ **Anemic Aggregate** - All logic in services, no behavior in aggregate
* ❌ **Split Invariants** - Invariant requires multiple objects in separate aggregates (race conditions)
* ❌ **Referencing by Instance** - Holding direct references to other aggregates (tight coupling)

### Domain Service

**Use when:**

* Operation doesn't naturally belong to any entity
* Logic spans multiple aggregates
* Stateless behavior needed
* Pure business logic that coordinates domain objects

**Examples:** `TransferFunds`, `CalculateShipping`, `ValidatePromotion`

### Application Service

**Use when:**

* Orchestrating use cases
* Managing transactions
* Coordinating repositories
* Dispatching domain events
* Enforcing security/authorization
* Translating between DTOs and domain objects

**Examples:** `PlaceOrderUseCase`, `RegisterUserHandler`, `ProcessPaymentCommand`

**Key Difference:**
* **Domain Service** = WHAT (business rules) - Lives in domain layer
* **Application Service** = HOW (orchestration) - Lives in application layer

### Domain Events

**Use when:**

* Decoupling aggregates within bounded context
* Recording audit trail of domain changes
* Enabling eventual consistency between aggregates
* Integrating across bounded contexts

**When NOT to use:**

* Internal aggregate changes (use private methods)
* Synchronous operations requiring immediate feedback

**Examples:** `OrderPlacedEvent`, `PaymentCompletedEvent`, `UserRegisteredEvent`

## Strategic vs Tactical Design

| Aspect        | Strategic Design                        | Tactical Design              |
| ------------- | --------------------------------------- | ---------------------------- |
| **Focus**     | Business organization                   | Code implementation          |
| **Scope**     | Entire domain                           | Single bounded context       |
| **Patterns**  | Subdomain, Bounded Context, Context Map | Entity, VO, Aggregate        |
| **When**      | Before coding                           | During coding                |
| **Outcome**   | Architecture decisions                  | Working code                 |
| **Questions** | Which contexts? What's core?            | How to model? What patterns? |

## Decision Trees

### Should this be an Entity or Value Object?

```
Does it need to be tracked over time?
├─ Yes → Does it change but remain the same thing?
│  ├─ Yes → Entity
│  └─ No → Review: might be Value Object
└─ No → Are two instances with same values interchangeable?
   ├─ Yes → Value Object
   └─ No → Entity
```

**Examples:**

* User account → Entity (tracked over time, has identity)
* Email address → Value Object (interchangeable if same value)
* Order → Entity (tracked, has lifecycle)
* Money → Value Object (100 USD is always 100 USD)

### How to Find Aggregate Boundaries?

**4-Step Process:**

```
Step 1: Identify Invariants
  └─ What business rules must ALWAYS be true?

Step 2: Group Objects That Enforce Same Invariant
  └─ Which objects are needed to verify this rule?

Step 3: Verify Transaction Scope
  └─ Can this invariant be verified in ONE transaction?
     ├─ YES → Same aggregate
     └─ NO → Separate aggregates + eventual consistency

Step 4: Check Aggregate Size
  └─ Will this aggregate load and save efficiently?
     ├─ 1-5 entities → Optimal ✓
     ├─ 6-10 entities → Review carefully
     └─ 10+ entities → Split by invariants
```

**Example Analysis - E-Commerce Order:**

| Invariant | Same Transaction? | Decision |
|-----------|-------------------|----------|
| Order has 1-50 items | ✓ Yes | Same aggregate |
| Total matches item sum | ✓ Yes | Same aggregate |
| Products exist | ✗ No (query catalog) | Separate aggregate |
| Customer has credit | ✗ No (query payment) | Separate aggregate |
| Inventory available | ✗ No (query inventory) | Separate aggregate |

**Result:** Order Aggregate = Order + OrderItems only

### Should this be a new Bounded Context?

```
Does this area have different:
├─ Language/terminology? → Likely separate BC
├─ Change frequency? → Likely separate BC
├─ Team ownership? → Likely separate BC
└─ All similar? → Probably same BC
```

**Questions to ask:**

* Does "Customer" mean different things here?
* Do changes happen independently?
* Is there a natural team boundary?
* Are invariants separate?

### Where does this logic belong?

```
Is this logic about a single entity's state?
├─ Yes → Entity method
│
└─ No → Does it span multiple aggregates?
   ├─ Yes → Domain Service or Application Service
   │  └─ Pure domain logic? → Domain Service
   │     └─ Orchestration? → Application Service
   │
   └─ No → Repository or Infrastructure
```

**Examples:**

* Calculate order total → Entity method
* Transfer funds between accounts → Domain Service
* Send confirmation email → Application Service
* Load order from database → Repository

## Aggregate Size Guidelines

| Aggregate Size | Entity Count | Load Time | Load Pattern | Action |
|----------------|-------------|-----------|--------------|--------|
| **Optimal** | 1-5 | <100ms | Single query | ✅ Keep as-is |
| **Acceptable** | 6-10 | <500ms | 1-2 joins | ⚠️ Review carefully |
| **Too Large** | 11-20 | >500ms | Multiple joins | ❌ Refactor soon |
| **Bloated** | 20+ | >1000ms | Complex loading | ❌ Refactor immediately |

**Refactoring Triggers:**

* Loading requires multiple database queries
* Lazy loading needed to avoid performance issues
* High contention (many operations lock same aggregate)
* Contains entities from different subdomains
* Parts change at different frequencies

## Subdomain Classification Cheat Sheet

| Criteria                    | Core           | Supporting       | Generic          |
| --------------------------- | -------------- | ---------------- | ---------------- |
| **Competitive advantage?**  | ✅ Yes          | ❌ No             | ❌ No             |
| **Complex business rules?** | ✅ Yes          | ⚙️ Some          | ❌ No             |
| **Custom built?**           | ✅ Yes          | ✅ Yes            | ❌ Buy/SaaS       |
| **Investment level**        | 60-70%         | 20-30%           | 5-10%            |
| **Team quality**            | Best (senior)  | Good (mid-level) | Minimal (junior) |
| **Test coverage**           | 90-95%         | 70-80%           | 50-60%           |
| **Change frequency**        | Weekly/Monthly | Quarterly/Yearly | Rarely           |

## Integration Pattern Selection

| Pattern                   | Use When                | Coupling | Effort |
| ------------------------- | ----------------------- | -------- | ------ |
| **Shared Kernel**         | High trust, shared team | High     | Low    |
| **Published Language**    | Multiple consumers      | Medium   | Medium |
| **Anti-Corruption Layer** | Legacy, unstable API    | Low      | High   |
| **Conformist**            | No negotiation power    | High     | Low    |
| **Customer/Supplier**     | Negotiated relationship | Medium   | Medium |

## Common Anti-Patterns

| Anti-Pattern              | Problem                           | Solution                    |
| ------------------------- | --------------------------------- | --------------------------- |
| **Anemic Domain Model**   | Logic in services, not entities   | Move behavior to entities   |
| **Database-First Design** | Schema drives model               | Model domain first          |
| **Bloated Shared Kernel** | Too much shared code              | Share only generic concepts |
| **Generic Names**         | `DataManager`, `Service`          | Use ubiquitous language     |
| **Large Aggregates**      | Too many objects in one aggregate | Split by invariants         |
| **Event Misuse**          | Events for orchestration          | Events record facts only    |

## Testing Strategy by Layer

| Layer | Type | What to Test | Dependencies | Speed | Quantity |
|-------|------|--------------|--------------|-------|----------|
| **Domain** | Unit Tests | Business rules, invariants, VOs | None | ⚡ <100ms | 🔵🔵🔵🔵🔵 |
| **Application** | Acceptance Tests | Use case orchestration | Test Doubles | ⚡ <500ms | 🔵🔵🔵🔵 |
| **Infrastructure** | Integration Tests | Adapters work correctly | Real DB/APIs | 🐌 1-5s | 🔵🔵 |
| **Presentation** | E2E Tests | Complete user flows | Complete App | 🐌 5-30s | 🔵 |

### Testing Pyramid

```
        /\
       /E2E\      ← Few (1-5%)
      /------\
     /  Intg  \   ← Moderate (5-10%)
    /----------\
   / Acceptance \ ← Many (15-20%)
  /--------------\
 /      Unit      \ ← Very Many (70-80%)
/------------------\
```

### Testing Strategy Guidelines

**Unit Tests (Domain Layer):**
- ✅ Test Value Objects, Entities, Aggregates, Domain Services
- ✅ No mocks, no infrastructure
- ✅ Fast (<100ms)
- ❌ Never mock domain objects

**Acceptance Tests (Application Layer):**
- ✅ Test Use Cases with test doubles (in-memory repos, fakes)
- ✅ Verify event publishing
- ✅ Test through port interfaces
- ❌ Don't mock domain logic

**Integration Tests (Infrastructure Layer):**
- ✅ Test with real database/APIs
- ✅ Verify adapter implementations
- ✅ Clean data before/after tests
- ⚠️ Use sparingly (slower)

**E2E Tests (Full Application):**
- ✅ Test critical user journeys
- ✅ Complete stack (all layers)
- ✅ Few tests (only critical paths)
- ❌ Never mock infrastructure in E2E

### Test Execution Schedule

| Stage | Tests | Purpose | Time Limit |
|-------|-------|---------|------------|
| **Pre-commit** | Unit + Acceptance | Fast feedback | <30s |
| **Pre-push** | + Integration | Pre-push confidence | <5min |
| **CI/CD** | + E2E | Full validation | <30min |

## Domain Events Quick Guide

### When to Use Domain Events

| Use Case | Description | Example |
|----------|-------------|---------|
| **Decouple Aggregates** | Avoid direct dependencies between aggregates | OrderPlaced → DecreaseInventory |
| **Audit Trail** | Record important domain occurrences | UserRegistered, PaymentCompleted |
| **Eventual Consistency** | Coordinate changes across aggregates | OrderCancelled → RefundPayment |
| **Cross-Context Integration** | Communicate between bounded contexts | OrderShipped → NotificationContext |

### When NOT to Use Domain Events

* ❌ **Internal Aggregate Changes** - Use private methods instead
* ❌ **Synchronous Immediate Feedback** - Use return values or exceptions

### Implementation Timing

| Timing | When | Risk | Use Case |
|--------|------|------|----------|
| **After Commit** | Events dispatched after transaction commits | ✓ Low | Production (Recommended) |
| **Before Commit** | Events dispatched in same transaction | ⚠️ High | Rarely needed |
| **Immediate** | Events dispatched synchronously | ⚠️ Very High | Testing only |

### Error Handling Strategies

1. **Retry with Exponential Backoff** - Temporary failures (network, timeouts)
2. **Dead Letter Queue (DLQ)** - Persistent failures requiring manual intervention
3. **Ignore Failure** - ⚠️ Only for truly non-critical events

### Event Naming Convention

**Format:** `[Aggregate][ActionInPastTense]Event`

**Examples:**
* ✅ `OrderPlacedEvent`, `PaymentCompletedEvent`, `UserRegisteredEvent`
* ❌ `PlaceOrderEvent` (command, not event), `OrderEvent` (not specific)

### Domain Events vs Integration Events

| Aspect | Domain Events | Integration Events |
|--------|---------------|-------------------|
| **Scope** | Within bounded context | Between bounded contexts |
| **Purpose** | Internal consistency | External integration |
| **Coupling** | Internal implementation | Public contract |
| **Change Frequency** | Can change freely | Must be versioned |
| **Format** | Internal structure | Standardized (JSON, Avro) |

## Application Services vs Domain Services

### Quick Comparison

| Aspect | Application Service | Domain Service |
|--------|-------------------|----------------|
| **Layer** | Application | Domain |
| **Purpose** | Use case orchestration | Business logic |
| **Logic Type** | Technical (HOW) | Business rules (WHAT) |
| **Dependencies** | Infrastructure ports | Only domain objects |
| **Transaction** | Manages transactions | No transaction management |
| **Testing** | Mock infrastructure | Pure unit tests |

### Application Service Responsibilities

1. **Use Case Orchestration** - Coordinate domain objects
2. **Transaction Management** - Begin/commit/rollback
3. **Repository Coordination** - Load/save aggregates
4. **Event Dispatching** - Publish domain events
5. **Security & Authorization** - Check permissions
6. **Input Validation** - Validate before domain
7. **DTO Translation** - Convert to/from domain

### What Application Services Should NOT Do

* ❌ **Business Logic** - Belongs in domain layer
* ❌ **Complex Orchestration** - Break into smaller use cases
* ❌ **Direct Database Queries** - Use repositories

## When to Apply DDD

**✅ Use DDD when you have:**

* Complex business domain ≥4/6 decision criteria
* Long-lived application (>1 year)
* Core competitive advantage
* Access to domain experts

**❌ Skip DDD when you have:**

* Simple CRUD operations
* Data-driven apps (reporting, analytics)
* Technical > domain complexity
* Tight constraints (time, budget)

For details, see [Chapter 1: When to Use DDD](01-when-to-use-ddd.md).

## Aggregate Design Checklist

**Boundary Definition:**
* [ ] Aggregate has clear root entity
* [ ] All invariants can be enforced within aggregate
* [ ] External references use IDs only
* [ ] Transaction boundary matches aggregate boundary
* [ ] External access only through root

**Size and Performance:**
* [ ] Aggregate loads quickly (subsecond)
* [ ] Contains 1-10 entities (typically 1-5)
* [ ] No lazy loading required
* [ ] Fits comfortably in memory
* [ ] Loading doesn't require complex joins

**Consistency:**
* [ ] Immediate consistency within aggregate
* [ ] Eventual consistency to other aggregates
* [ ] Domain events published for external changes
* [ ] No distributed transactions
* [ ] One aggregate per transaction

**Encapsulation:**
* [ ] Business logic in aggregate, not services
* [ ] Private fields with public methods
* [ ] Invariants always protected
* [ ] Cannot be put in invalid state
* [ ] Intention-revealing method names

**Testability:**
* [ ] Aggregate can be tested in isolation
* [ ] No infrastructure dependencies
* [ ] Clear pre/post conditions
* [ ] Behavior testable without database
* [ ] Domain events testable

## Bounded Context Checklist

* [ ] Clear ubiquitous language defined
* [ ] Explicit boundaries documented
* [ ] Integration patterns chosen (Context Map)
* [ ] Team ownership assigned
* [ ] Separate deployable unit (if microservices)

## Repository Interface Checklist

* [ ] Defined in domain layer
* [ ] Returns domain objects (not DTOs)
* [ ] Collection-like interface
* [ ] Hides persistence details
* [ ] Works with aggregate roots only
* [ ] No business logic in repository

## Business Rules Organization

### Where Does Each Rule Live?

| Rule Type | Location | Example | Dependencies |
|-----------|----------|---------|--------------|
| **Single value validation** | Value Object | Email format | None |
| **Entity invariant** | Aggregate | Order has 1-50 items | Internal state |
| **State transition** | Entity method | Order cancellable? | Internal state |
| **Cross-aggregate logic** | Domain Service | Discount calculation | Multiple aggregates |
| **Database check** | Application Service | Email unique? | Repository |
| **Authorization** | Application Service | User can delete? | Permission service |
| **HTTP validation** | Presentation Layer | JSON valid? | DTO validator |

### Decision Tree: Where Does This Logic Belong?

```
Validates single value?
├─ YES → Value Object
└─ NO → Continue

Modifies entity state?
├─ YES → Entity method (if only this entity)
└─ NO → Continue

Involves multiple aggregates (business logic)?
├─ YES → Domain Service
└─ NO → Continue

Needs database/infrastructure?
├─ YES → Application Service
└─ NO → Review requirements
```

## Advanced Patterns

### Event-Driven Patterns

#### Event Recording Flow

**Rule:** Aggregates record events. Use Cases publish them.

| Who | Responsibility | Why |
|-----|---------------|-----|
| **Aggregate** | Records events | Knows WHEN events occur (domain logic) |
| **Use Case** | Publishes events | Controls IF events published (after transaction) |

**Implementation:**
```
1. Aggregate.recordEvent() → Internal list
2. UseCase.save(aggregate) → Transaction commits
3. UseCase.publish(aggregate.pullEvents()) → After successful save
```

---

#### Outbox Pattern

**Problem:** Event publishing can fail (event bus down, transaction rollback)

**Solution:** Store events in database transaction with aggregate state

| Pattern | Guarantees | Complexity | Use When |
|---------|-----------|------------|----------|
| **Outbox** | Delivery guaranteed | Medium | Production systems |
| **In-memory EventBus** | None | Low | MVP/prototype only |

**Flow:**
```
1. Transaction: Save aggregate + Save events to outbox table (atomic)
2. Background worker: Publish events from outbox → Event bus
3. Mark as published
```

---

#### Inbox Pattern

**Problem:** Events delivered multiple times (network retries, at-least-once delivery)

**Solution:** Track processed events, skip duplicates

**Implementation:**
```
1. Check: Event already processed?
   ├─ YES → Skip
   └─ NO → Continue
2. Record: event_id in inbox table (processing)
3. Process: Business logic
4. Mark: processed
```

**Use when:** Handlers must be exactly-once (inventory updates, payments)

---

#### Pattern Selection Matrix

| Scenario | Patterns | Reason |
|----------|---------|--------|
| **MVP** | In-memory EventBus | Simple, sufficient |
| **Production (single BC)** | Outbox + Inbox | Guaranteed delivery + idempotency |
| **Production (multi-BC)** | Outbox + Inbox + Saga | Full distributed guarantees |
| **High-scale** | Outbox + Message Broker + Inbox | Scalable event streaming |

---

### Distributed Coordination

#### Saga Pattern

**Problem:** Distributed operations can fail partially (Order created, payment charged, but inventory unavailable)

**Solution:** Sequence of local transactions with compensating actions

**Flow:**
```
Step 1: CreateOrder → Success (compensation: CancelOrder)
Step 2: ChargePayment → Success (compensation: RefundPayment)
Step 3: ReserveInventory → FAIL
  → Run compensations in reverse: RefundPayment, CancelOrder
```

**Key Principles:**
1. **LIFO Compensations** - Last successful step compensated first
2. **Idempotent Compensations** - Safe to run multiple times
3. **Best-Effort** - May not fully restore original state
4. **Eventual Consistency** - Accept brief inconsistency

---

#### Saga vs Distributed Transaction

| Aspect | Saga Pattern | 2PC (Distributed TX) |
|--------|--------------|---------------------|
| **Consistency** | Eventual | Immediate (ACID) |
| **Availability** | High (no locks) | Low (locks held) |
| **Scalability** | Scales well | Poor |
| **Complexity** | High (compensations) | Lower |
| **Use when** | Cross-BC, microservices | Single DB, monolith |

---

#### When to Use Saga

**✅ Use Saga when:**
- Cross-bounded-context operations
- Microservices architecture
- Long-running processes (hours/days)
- High availability required
- Accept eventual consistency

**❌ Don't use Saga when:**
- Operations within same aggregate
- Require immediate consistency
- Simple CRUD
- Single database/monolith

---

### Event Pattern Complexity Levels

| Level | Patterns | Use When | Complexity |
|-------|----------|----------|------------|
| **1. Simple** | In-memory EventBus | MVP, learning | Low |
| **2. Reliable** | Outbox | Production, single BC | Medium |
| **3. Idempotent** | Inbox | Handlers need dedup | Medium |
| **4. Production** | Outbox + Inbox | Production critical ops | High |
| **5. Distributed** | Outbox + Kafka + Inbox | Multi-BC microservices | Very High |

---

### Advanced Patterns Summary

| Pattern | Problem Solved | Key Benefit | Trade-off |
|---------|---------------|-------------|-----------|
| **Event Recording Flow** | Who records/publishes? | Clear responsibilities | Foundational (always use) |
| **Outbox** | Event publishing fails | Guaranteed delivery | Slight delay (eventual) |
| **Inbox** | Duplicate events | Exactly-once processing | Extra DB table + checks |
| **Saga** | Distributed TX don't scale | High availability | Complex compensations |

***

**Navigation:**

* [← Previous: Appendix A](appendix-a-strategic-classification.md)
* [Back to Introduction](./)
* [Table of Contents](./#table-of-contents)
