# Glossary

Quick reference for Domain-Driven Design terminology. Terms are defined conceptually and linked to detailed explanations in the main chapters.

---

## A

**Aggregate**
A cluster of domain objects (entities and value objects) treated as a single unit for data changes. Has a root entity, enforces consistency boundaries, and ensures invariants are always satisfied.
→ See: [Chapter 3.3: Aggregate](03-tactical-patterns.md#33-aggregate)

**Anti-Corruption Layer (ACL)**
A translation layer that protects your domain model from external systems or legacy code. Converts external models into your domain language to prevent external complexity from leaking into your bounded context.
→ See: [Chapter 4.3: Anti-Corruption Layer](04-integration-patterns.md#43-anti-corruption-layer)

**Application Service**
Also known as **Use Case**. A thin orchestration layer in the application layer that coordinates domain objects, manages transactions, and handles infrastructure concerns. Does not contain business logic.
→ See: [Chapter 3.7: Application Services](03-tactical-patterns.md#37-application-services)

---

## B

**Bounded Context**
An explicit boundary within which a specific domain model applies consistently. Defines where the ubiquitous language has a particular meaning. Different bounded contexts can have different models for the same concept (e.g., "Customer" means different things in Sales vs Support contexts).
→ See: [Chapter 2.2: Bounded Context](02-strategic-design.md#22-bounded-context)

**Business Rules Organization**
Strategy for determining where different types of business logic should live in your architecture (value objects, entities, domain services, or application services).
→ See: [Chapter 3.8: Business Rules Organization](03-tactical-patterns.md#38-business-rules-organization)

---

## C

**Conformist**
An integration pattern where the downstream context conforms entirely to the upstream context's model without negotiation. Common when integrating with external systems you cannot influence (e.g., SaaS identity providers, payment processors).
→ See: [Chapter 4.1.1: Conformist](04-integration-patterns.md#conformist)

**Context Map**
A visual representation showing relationships between bounded contexts, including integration patterns, dependencies, and power dynamics. Documents how contexts interact and which patterns govern their relationships.
→ See: [Chapter 4.1: Context Map](04-integration-patterns.md#41-context-map)

**Core Subdomain**
The part of your domain that provides competitive advantage and differentiates your business. Requires the most investment, best developers, and highest code quality.
→ See: [Chapter 2: Strategic Design](02-strategic-design.md) and [Appendix A: Strategic Classification](appendix-a-strategic-classification.md)

**Customer/Supplier**
An integration pattern where upstream (supplier) provides services to downstream (customer) with negotiated requirements and priorities. Customer influences but doesn't dictate the supplier's roadmap.
→ See: [Chapter 4.1.1: Customer/Supplier](04-integration-patterns.md#customersupplier)

---

## D

**Domain**
The entire business problem space you're working in. For an e-commerce company, the domain is "e-commerce." Contains multiple subdomains that each solve specific aspects of the problem.
→ See: [Chapter 2.1: Domain and Subdomain](02-strategic-design.md#21-domain-and-subdomain)

**Domain Event**
An immutable record of something significant that happened in the domain at a specific point in time. Named in past tense (OrderPlaced, UserRegistered) and contains all information handlers need.
→ See: [Chapter 3.4: Domain Events](03-tactical-patterns.md#34-domain-events)

**Domain Service**
Stateless operation encapsulating business logic that doesn't naturally belong to an entity or value object. Contains only domain logic, no infrastructure dependencies. Operates on multiple domain objects or represents domain concepts that are naturally services (policies, calculations).
→ See: [Chapter 3.6: Domain Services](03-tactical-patterns.md#36-domain-services)

---

## E

**Entity**
A domain object with unique identity that persists over time. Identity remains constant even as attributes change. Two entities with the same data but different IDs are different objects.
→ See: [Chapter 3.1: Entity](03-tactical-patterns.md#31-entity)

**Event Storming**
A collaborative workshop technique for discovering domain events, aggregates, and bounded context boundaries through timeline-based exploration with domain experts.
→ See: [Chapter 3.4.9: Event Storming Technique](03-tactical-patterns.md#349-event-storming-technique)

**Eventual Consistency**
A consistency model where changes across aggregates or bounded contexts are synchronized asynchronously over time using domain events, rather than within a single transaction.
→ See: [Chapter 3.3.1: Aggregate Design Principles](03-tactical-patterns.md#331-aggregate-design-principles) and [Chapter 6.2.3: Eventual Consistency Trade-offs](06-advanced-patterns.md#623-eventual-consistency-trade-offs)

---

## G

**Generic Subdomain**
Supporting functionality that is common across industries and doesn't differentiate your business. Best handled by third-party solutions or low-investment custom code (authentication, email services, payment gateways).
→ See: [Chapter 2: Strategic Design](02-strategic-design.md) and [Appendix A: Strategic Classification](appendix-a-strategic-classification.md)

---

## I

**Inbox Pattern**
Advanced pattern for achieving exactly-once processing of domain events by tracking processed events and skipping duplicates. Prevents duplicate side effects when events are delivered multiple times.
→ See: [Chapter 6.1.3: Inbox Pattern](06-advanced-patterns.md#613-inbox-pattern-idempotency)

**Integration Event**
Event designed for cross-bounded-context communication. More stable than domain events (public contract, versioned). Distinguished from domain events which are internal to a bounded context.
→ See: [Chapter 3.4.7: Domain Events vs Integration Events](03-tactical-patterns.md#347-domain-events-vs-integration-events)

**Invariant**
A business rule that must always be true. Aggregates are responsible for protecting their invariants. If a rule can be temporarily broken (between transactions), it's not an invariant.
→ See: [Chapter 3.3: Aggregate](03-tactical-patterns.md#33-aggregate)

---

## O

**Open Host Service**
An integration pattern where upstream provides a well-documented, stable, generic API designed to serve multiple downstream consumers.
→ See: [Chapter 4.1.1: Open Host Service](04-integration-patterns.md#open-host-service)

**Outbox Pattern**
Advanced pattern for guaranteeing domain event delivery by storing events in the database within the same transaction as aggregate changes, then publishing them asynchronously via a background worker.
→ See: [Chapter 6.1.2: Outbox Pattern](06-advanced-patterns.md#612-outbox-pattern-transactional-guarantee)

---

## P

**Partnership**
An integration pattern where two contexts have mutual dependency with joint planning, synchronized releases, and shared responsibility for integration. High trust and close collaboration required.
→ See: [Chapter 4.1.1: Partnership](04-integration-patterns.md#partnership)

**Published Language**
A shared, well-defined language (often domain events or data formats) used for integration between contexts. Typically involves standardized schemas (JSON, Avro, Protobuf) in event-driven architectures.
→ See: [Chapter 4.1.1: Published Language](04-integration-patterns.md#published-language)

---

## R

**Repository**
Interface for retrieving and persisting aggregates. Provides collection-like interface, hides persistence details, and works only with aggregate roots. Defined in domain layer, implemented in infrastructure.
→ See: [Chapter 3.5: Repository](03-tactical-patterns.md#35-repository)

---

## S

**Saga Pattern**
Pattern for coordinating distributed operations across multiple bounded contexts using a sequence of local transactions with compensating actions. Achieves eventual consistency without distributed transactions.
→ See: [Chapter 6.2.1: Saga Pattern](06-advanced-patterns.md#621-saga-pattern-orchestration)

**Shared Kernel**
Code shared between bounded contexts, typically containing generic value objects and base types. Should be kept minimal as it creates coupling between contexts.
→ See: [Chapter 4.2: Shared Kernel](04-integration-patterns.md#42-shared-kernel)

**Strategic Design**
High-level approach to organizing complex domains by identifying subdomains, defining bounded contexts, and planning integration between them. Done before tactical implementation.
→ See: [Chapter 2: Strategic Design](02-strategic-design.md)

**Subdomain**
A distinct area within a domain with specific responsibilities. Classified as Core (competitive advantage), Supporting (necessary but standard), or Generic (commodity functionality).
→ See: [Chapter 2.1: Domain and Subdomain](02-strategic-design.md#21-domain-and-subdomain)

**Supporting Subdomain**
Functionality necessary for the business but not differentiating. Custom-built but doesn't provide competitive advantage (order processing, inventory management, customer service).
→ See: [Chapter 2: Strategic Design](02-strategic-design.md) and [Appendix A: Strategic Classification](appendix-a-strategic-classification.md)

---

## T

**Tactical Patterns**
Concrete building blocks for implementing domain models: Entity, Value Object, Aggregate, Domain Event, Repository, Domain Service, Application Service. Used after strategic design is complete.
→ See: [Chapter 3: Tactical Patterns](03-tactical-patterns.md)

---

## U

**Ubiquitous Language**
Shared vocabulary between developers and domain experts, directly reflected in code. Terms have precise meaning within a bounded context. Reduces translation errors and ensures the model represents actual business concepts.
→ See: [Chapter 2.3: Ubiquitous Language](02-strategic-design.md#23-ubiquitous-language)

**Use Case**
See **Application Service**. Same concept, different terminology. Thin orchestration layer coordinating domain objects to fulfill user actions.
→ See: [Chapter 3.7.0: Use Cases and Application Services](03-tactical-patterns.md#370-use-cases-and-application-services)

---

## V

**Value Object**
An immutable domain object identified by its values rather than identity. Two instances with the same values are interchangeable. Describes characteristics or measurements (Email, Money, Address, DateRange).
→ See: [Chapter 3.2: Value Object](03-tactical-patterns.md#32-value-object)

---

**Navigation:**
* [← Back to Introduction](./)
* [Table of Contents](./#table-of-contents)
* [Quick Reference](quick-reference.md)
