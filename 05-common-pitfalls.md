# Chapter 5: Common Pitfalls

Mistakes to avoid when implementing Domain-Driven Design.

## 1. Anemic Domain Model

**Problem:** Entities as data bags with all logic in services.

```typescript
// ❌ Anemic - Entity is just data
class Order {
  id: OrderId;
  items: OrderItem[];
  status: OrderStatus;
  total: Money;
}

class OrderService {
  placeOrder(order: Order): void {
    // All logic in service
    if (order.items.length === 0) {
      throw new Error("Empty order");
    }
    order.status = OrderStatus.Placed;
  }
}
```

```typescript
// ✅ Rich Domain Model - Entity has behavior
class Order {
  private constructor(
    private readonly id: OrderId,
    private items: OrderItem[],
    private status: OrderStatus
  ) {}

  place(): void {
    if (this.items.length === 0) {
      throw new EmptyOrderError();
    }
    this.status = OrderStatus.Placed;
  }

  calculateTotal(): Money {
    return this.items.reduce(
      (sum, item) => sum.add(item.subtotal()),
      Money.zero()
    );
  }
}
```

**Why it's bad:**
- Business logic scattered across services
- Entities become dumb data containers
- Breaks encapsulation
- Harder to maintain and test

## 2. Database Design First

**Problem:** Starting with database schema before domain model.

```typescript
// ❌ Database-first thinking
@Entity()
class OrderEntity {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  customer_id: number;

  @Column('decimal')
  total: number;

  // Getters and setters
}
```

```typescript
// ✅ Domain-first thinking
class Order {
  private constructor(
    private readonly id: OrderId,
    private readonly customer: CustomerId,
    private items: OrderItem[]
  ) {}

  total(): Money {
    return this.items.reduce(
      (sum, item) => sum.add(item.price()),
      Money.zero()
    );
  }
}

// Infrastructure layer handles persistence
```

**Why it's bad:**
- Database concerns leak into domain
- Focus on storage instead of behavior
- Domain model constrained by DB capabilities
- Harder to refactor

**Do instead:**
- Model the domain first
- Let infrastructure adapt to domain model
- Use mappers/adapters between domain and persistence

## 3. Bloated Shared Kernel

**Problem:** Sharing too much code between Bounded Contexts.

```typescript
// ❌ Sharing too much
// shared-kernel/
//   - User.ts (complete entity)
//   - Order.ts (complete entity)
//   - OrderService.ts (business logic)
//   - PaymentGateway.ts (infrastructure)
```

```typescript
// ✅ Minimal shared kernel
// shared-kernel/
//   - Email.ts (generic value object)
//   - Money.ts (generic value object)
//   - DomainEvent.ts (base class)
//   - Result.ts (common type)
```

**Why it's bad:**
- High coupling between contexts
- Changes in one context break others
- Difficult to evolve contexts independently
- Defeats purpose of bounded contexts

**Do instead:**
- Share only truly generic concepts
- Prefer duplication over wrong abstraction
- Use integration patterns (ACL, Published Language)

## 4. Ignoring Ubiquitous Language

**Problem:** Using generic technical names instead of business terms.

```typescript
// ❌ Generic names
class DataManager {
  processData(data: Record): void {
    this.repository.save(data);
  }
}

// ❌ Technical jargon
class OrderEntity {
  insert(): void {}
  delete(): void {}
  update(): void {}
}
```

```typescript
// ✅ Business language
class Order {
  place(): void {}
  cancel(reason: CancellationReason): void {}
  ship(): void {}
}

class InventoryManager {
  reserveStock(product: Product, quantity: Quantity): void {}
  releaseStock(product: Product, quantity: Quantity): void {}
}
```

**Why it's bad:**
- Disconnect between code and business
- Domain experts can't read the code
- Knowledge crunching is lost
- Harder to communicate

**Do instead:**
- Use exact terms from domain experts
- Rename when language evolves
- Keep glossary in sync with code

## 5. Large Aggregates

**Problem:** Creating aggregates that are too large.

```typescript
// ❌ Aggregate too large
class Order {
  private items: OrderItem[];
  private shipments: Shipment[];
  private payments: Payment[];
  private invoices: Invoice[];
  private refunds: Refund[];
  private reviews: Review[];
  // ... 15 more collections
}
```

```typescript
// ✅ Smaller, focused aggregates
class Order {
  private items: OrderItem[];
  // Reference other aggregates by ID
  private paymentId?: PaymentId;
  private shipmentId?: ShipmentId;
}

class Payment {
  private orderId: OrderId; // Reference, not containment
  private transactions: Transaction[];
}
```

**Why it's bad:**
- Performance issues (loading too much data)
- Concurrency conflicts (more contention)
- Harder to reason about invariants
- Difficult to test

**Do instead:**
- Keep aggregates focused on invariants
- Reference other aggregates by ID
- Use domain events for coordination

## 6. Misusing Domain Events

**Problem:** Using domain events for everything, including workflow orchestration.

```typescript
// ❌ Event-driven workflow in domain
class Order {
  place(): void {
    this.status = OrderStatus.Placed;

    // ❌ Orchestrating workflow via events
    this.addEvent(new OrderPlaced());
    this.addEvent(new ChargePayment());
    this.addEvent(new SendConfirmationEmail());
    this.addEvent(new UpdateInventory());
  }
}
```

```typescript
// ✅ Events record facts, orchestration elsewhere
class Order {
  place(): void {
    if (!this.canBePlaced()) {
      throw new OrderCannotBePlacedError();
    }
    this.status = OrderStatus.Placed;
    this.addEvent(new OrderPlaced(this.id, this.customerId));
  }
}

// Application service orchestrates workflow
class PlaceOrderUseCase {
  async execute(command: PlaceOrderCommand): Promise<void> {
    const order = await this.orderRepo.findById(command.orderId);
    order.place();
    await this.orderRepo.save(order);

    // Orchestrate follow-up actions
    await this.paymentService.charge(order);
    await this.emailService.sendConfirmation(order);
  }
}
```

**Why it's bad:**
- Domain becomes coupled to workflow
- Hard to reason about side effects
- Testing becomes difficult
- Violates single responsibility

**Do instead:**
- Domain events record facts
- Application layer orchestrates workflows
- Use saga/process manager for complex flows

## 7. Over-Engineering Simple Domains

**Problem:** Applying full DDD to simple CRUD applications.

**When DDD is overkill:**
- Simple data entry forms
- Basic reporting tools
- Straightforward admin panels
- Minimal business logic

**Do instead:**
- Use simpler patterns (Active Record, Transaction Script)
- Apply DDD only to complex subdomains
- Start simple, refactor to DDD when complexity justifies it

See [Chapter 1: When to Use DDD](01-when-to-use-ddd.md) for decision criteria.

---

**Navigation:**
- [← Previous: Integration Patterns](04-integration-patterns.md)
- [Next: Appendix A →](appendix-a-strategic-classification.md)
- [Table of Contents](README.md#table-of-contents)
