# Quick Reference

Fast lookup for DDD patterns, comparisons, and decision trees.

## Pattern Comparison Table

| Pattern | Identity | Mutability | Lifespan | Purpose |
|---------|----------|------------|----------|---------|
| **Entity** | Yes (UserId, OrderId) | Mutable | Long-lived | Track objects that change over time |
| **Value Object** | No | Immutable | Short-lived | Describe characteristics |
| **Aggregate** | Yes (Root Entity) | Mutable (as whole) | Long-lived | Enforce consistency boundaries |
| **Domain Event** | Yes (EventId) | Immutable | Permanent | Record facts about the past |

## When to Use Each Tactical Pattern

### Entity

**Use when:**
- Need to track something over time
- Object can change but remains the same thing
- Identity matters more than attributes

**Examples:** `User`, `Order`, `Product`, `Account`

### Value Object

**Use when:**
- Describes a characteristic
- Two instances with same values are interchangeable
- No lifecycle or identity needed

**Examples:** `Email`, `Money`, `Address`, `DateRange`

### Aggregate

**Use when:**
- Need to enforce business rules across multiple objects
- Clear consistency boundary exists
- Transaction boundary needed

**Examples:** `Order` (with `OrderItems`), `ShoppingCart`, `Reservation`

### Domain Service

**Use when:**
- Operation doesn't naturally belong to any entity
- Logic spans multiple aggregates
- Stateless behavior needed

**Examples:** `TransferFunds`, `CalculateShipping`, `ValidatePromotion`

## Strategic vs Tactical Design

| Aspect | Strategic Design | Tactical Design |
|--------|------------------|-----------------|
| **Focus** | Business organization | Code implementation |
| **Scope** | Entire domain | Single bounded context |
| **Patterns** | Subdomain, Bounded Context, Context Map | Entity, VO, Aggregate |
| **When** | Before coding | During coding |
| **Outcome** | Architecture decisions | Working code |
| **Questions** | Which contexts? What's core? | How to model? What patterns? |

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
- User account → Entity (tracked over time, has identity)
- Email address → Value Object (interchangeable if same value)
- Order → Entity (tracked, has lifecycle)
- Money → Value Object (100 USD is always 100 USD)

### Should this be a new Bounded Context?

```
Does this area have different:
├─ Language/terminology? → Likely separate BC
├─ Change frequency? → Likely separate BC
├─ Team ownership? → Likely separate BC
└─ All similar? → Probably same BC
```

**Questions to ask:**
- Does "Customer" mean different things here?
- Do changes happen independently?
- Is there a natural team boundary?
- Are invariants separate?

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
- Calculate order total → Entity method
- Transfer funds between accounts → Domain Service
- Send confirmation email → Application Service
- Load order from database → Repository

## Subdomain Classification Cheat Sheet

| Criteria | Core | Supporting | Generic |
|----------|------|-----------|---------|
| **Competitive advantage?** | ✅ Yes | ❌ No | ❌ No |
| **Complex business rules?** | ✅ Yes | ⚙️ Some | ❌ No |
| **Custom built?** | ✅ Yes | ✅ Yes | ❌ Buy/SaaS |
| **Investment level** | 60-70% | 20-30% | 5-10% |
| **Team quality** | Best (senior) | Good (mid-level) | Minimal (junior) |
| **Test coverage** | 90-95% | 70-80% | 50-60% |
| **Change frequency** | Weekly/Monthly | Quarterly/Yearly | Rarely |

## Integration Pattern Selection

| Pattern | Use When | Coupling | Effort |
|---------|----------|----------|--------|
| **Shared Kernel** | High trust, shared team | High | Low |
| **Published Language** | Multiple consumers | Medium | Medium |
| **Anti-Corruption Layer** | Legacy, unstable API | Low | High |
| **Conformist** | No negotiation power | High | Low |
| **Customer/Supplier** | Negotiated relationship | Medium | Medium |

## Common Anti-Patterns

| Anti-Pattern | Problem | Solution |
|-------------|---------|----------|
| **Anemic Domain Model** | Logic in services, not entities | Move behavior to entities |
| **Database-First Design** | Schema drives model | Model domain first |
| **Bloated Shared Kernel** | Too much shared code | Share only generic concepts |
| **Generic Names** | `DataManager`, `Service` | Use ubiquitous language |
| **Large Aggregates** | Too many objects in one aggregate | Split by invariants |
| **Event Misuse** | Events for orchestration | Events record facts only |

## Testing Strategy by Layer

| Layer | What to Test | How |
|-------|-------------|-----|
| **Domain** | Business rules, invariants | Unit tests (no mocks) |
| **Application** | Use case orchestration | Unit tests (mock ports) |
| **Infrastructure** | Adapters work correctly | Integration tests |
| **Presentation** | Request/response mapping | Unit + E2E tests |

## When to Apply DDD

**✅ Use DDD when you have:**
- Complex business domain ≥4/6 decision criteria
- Long-lived application (>1 year)
- Core competitive advantage
- Access to domain experts

**❌ Skip DDD when you have:**
- Simple CRUD operations
- Data-driven apps (reporting, analytics)
- Technical > domain complexity
- Tight constraints (time, budget)

For details, see [Chapter 1: When to Use DDD](01-when-to-use-ddd.md).

## Aggregate Design Checklist

- [ ] Root entity enforces all invariants
- [ ] External access only through root
- [ ] Transaction boundary = aggregate boundary
- [ ] Small enough to fit in memory
- [ ] References other aggregates by ID only
- [ ] Contains entities and value objects needed for invariants

## Bounded Context Checklist

- [ ] Clear ubiquitous language defined
- [ ] Explicit boundaries documented
- [ ] Integration patterns chosen (Context Map)
- [ ] Team ownership assigned
- [ ] Separate deployable unit (if microservices)

## Repository Interface Checklist

- [ ] Defined in domain layer
- [ ] Returns domain objects (not DTOs)
- [ ] Collection-like interface
- [ ] Hides persistence details
- [ ] Works with aggregate roots only
- [ ] No business logic in repository

---

**Navigation:**
- [← Previous: Appendix A](appendix-a-strategic-classification.md)
- [Back to Introduction](README.md)
- [Table of Contents](README.md#table-of-contents)
