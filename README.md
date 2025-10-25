# Domain-Driven Design

## A Conceptual Foundation

_Based on the works of Eric Evans (2003) and Vaughn Vernon (2013)_

***

## About This Book

This book provides a comprehensive conceptual foundation for Domain-Driven Design (DDD), focusing on the principles and patterns that enable you to tackle complexity in software development through deep domain understanding and strategic design.

**Type:** Conceptual reference book (technology-agnostic)

### Prerequisites

This book assumes you have:

- **Software development experience** (1-2+ years building applications)
- **Basic architecture knowledge** (understanding of layers, separation of concerns)
- **Familiarity with object-oriented programming** (classes, encapsulation, inheritance)
- **Database fundamentals** (entities, relationships, transactions)
- **Experience with business requirements** (translating business needs to code)

**Not required:**
- Framework-specific knowledge (this book is technology-agnostic)
- Prior DDD experience
- Experience with microservices or distributed systems

***

## How to Read This Book

**If you're new to DDD:**

1. Start with [Introduction](./#introduction) to understand the core philosophy
2. Read [Chapter 1: When to Use DDD](01-when-to-use-ddd.md) to assess applicability
3. Study [Chapter 2: Strategic Design](02-strategic-design.md) before diving into tactical patterns
4. Reference [Chapter 3: Tactical Patterns](03-tactical-patterns.md) as needed

**If you're experienced:**

* Use the [Table of Contents](./#table-of-contents) to jump to specific patterns
* Review [Chapter 5: Common Pitfalls](05-common-pitfalls.md) to avoid known issues
* Consult the [Quick Reference](quick-reference.md) for pattern comparisons

***

## Table of Contents

### [Introduction](./#introduction)

Understanding the Philosophy Behind DDD

### Core Chapters

1. [**When to Use DDD**](01-when-to-use-ddd.md) Applicability and Decision Criteria
2. [**Strategic Design**](02-strategic-design.md)
   * 2.1 Domain and Subdomain
   * 2.2 Bounded Context
   * 2.3 Ubiquitous Language
   * 2.4 Strategic Classification
   * 2.5 DDD Discovery Methodologies
     * 2.5.1 Event Storming
     * 2.5.2 Domain Storytelling
     * 2.5.3 Example Mapping
     * 2.5.4 Context Mapping Workshop
     * 2.5.5 Choosing the Right Methodology
3. [**Tactical Patterns**](03-tactical-patterns.md)
   * 3.0 Architecture Overview
     * 3.0.1 Layered Architecture in DDD
     * 3.0.2 Isolating the Domain: Hexagonal Architecture
   * 3.1 Entity
     * 3.1.1 Common Misconceptions About Entities
   * 3.2 Value Object
     * 3.2.1 Common Misconceptions About Value Objects
   * 3.3 Aggregate
     * 3.3.1 Aggregate Design Principles
     * 3.3.2 Finding Aggregate Boundaries
     * 3.3.3 Common Aggregate Design Mistakes
     * 3.3.4 Aggregate Refactoring Process
     * 3.3.5 Aggregate Design Checklist
   * 3.4 Domain Events
     * 3.4.1 When to Use Domain Events
     * 3.4.2 When NOT to Use Domain Events
     * 3.4.3 Domain Event Patterns
     * 3.4.4 Implementation Timing
     * 3.4.5 Error Handling for Event Handlers
     * 3.4.6 Common Domain Event Mistakes
     * 3.4.7 Domain Events vs Integration Events
     * 3.4.8 Event Naming Conventions
     * 3.4.9 Discovery Techniques (see Chapter 2.5)
   * 3.5 Repository
   * 3.6 Domain Services
     * 3.6.1 When to Use Domain Services
     * 3.6.2 What Domain Services Should NOT Do
     * 3.6.3 Domain Service vs Application Service
     * 3.6.4 The Common Anti-Pattern
     * 3.6.5 Decision Matrix
   * 3.7 Application Services
     * 3.7.0 Use Cases and Application Services
     * 3.7.1 Application Service vs Domain Service
     * 3.7.2 Responsibilities of Application Services
     * 3.7.3 What Application Services Should NOT Do
     * 3.7.4 Application Service Patterns
     * 3.7.5 Testing Application Services
     * 3.7.6 Application Service Best Practices
     * 3.7.7 Testing Strategy for DDD Applications
   * 3.8 Business Rules Organization
     * 3.8.1 Types of Business Rules
     * 3.8.2 Where Each Rule Lives
     * 3.8.3 Validation Layering
     * 3.8.4 Organizing Rules: The Big Picture
4. [**Integration Patterns**](04-integration-patterns.md)
   * 4.1 Context Map
     * 4.1.1 Context Relationship Types
       * Partnership
       * Customer/Supplier
       * Conformist
       * Anti-Corruption Layer (ACL)
       * Open Host Service
       * Published Language
     * 4.1.2 Choosing Integration Patterns
     * 4.1.3 Context Map Evolution
   * 4.2 Shared Kernel
   * 4.3 Anti-Corruption Layer
5. [**Common Pitfalls**](05-common-pitfalls.md) Mistakes to Avoid
6. [**Advanced Patterns**](06-advanced-patterns.md)
   * 6.1 Event-Driven Patterns
     * 6.1.1 Event Recording and Publishing Flow
     * 6.1.2 Outbox Pattern (Transactional Guarantee)
     * 6.1.3 Inbox Pattern (Idempotency)
     * 6.1.4 When to Use Each Pattern
   * 6.2 Distributed Coordination
     * 6.2.1 Saga Pattern (Orchestration)
     * 6.2.2 Compensating Transactions
     * 6.2.3 Eventual Consistency Trade-offs
   * 6.3 Event Sourcing
     * 6.3.1 What is Event Sourcing?
     * 6.3.2 Core Concepts
     * 6.3.3 When to Use Event Sourcing
     * 6.3.4 Trade-offs
     * 6.3.5 Event Sourcing vs Traditional Persistence
     * 6.3.6 Common Mistakes
     * 6.3.7 Event Sourcing with DDD Aggregates
   * 6.4 CQRS (Command Query Responsibility Segregation)
     * 6.4.1 What is CQRS?
     * 6.4.2 CQRS Patterns
     * 6.4.3 When to Use CQRS
     * 6.4.4 Trade-offs
     * 6.4.5 CQRS with Eventual Consistency
     * 6.4.6 Common Mistakes
     * 6.4.7 CQRS Decision Matrix
     * 6.4.8 CQRS with DDD

### Appendices

* [**Appendix A: Strategic Classification Framework**](appendix-a-strategic-classification.md) Detailed framework for classifying subdomains (Core, Supporting, Generic) with e-commerce examples
* [**Glossary**](glossary.md) Quick reference definitions for all DDD terms
* [**Quick Reference**](quick-reference.md) Pattern comparisons, decision trees, and cheat sheets

### [References and Further Reading](./#references-and-further-reading)

***

## Introduction

**Domain-Driven Design** is a software development approach that focuses on modeling complex business domains through deep collaboration between technical experts and domain experts. Introduced by Eric Evans in 2003 and refined by Vaughn Vernon in 2013, DDD provides both strategic and tactical tools to tackle complexity at the heart of software.

### The Philosophy

At its core, DDD recognizes that:

1. **The domain is paramount** - The business problem you're solving is more important than the technology you use
2. **Language matters** - The way we talk about the domain shapes how we model it
3. **Complexity is inevitable** - But it can be managed through careful boundaries and strategic design
4. **Collaboration is essential** - Developers and domain experts must speak the same language

### Core Principles

The foundation of DDD rests on four pillars:

1. **Ubiquitous Language**: A shared vocabulary between developers and domain experts, reflected directly in the code
2. **Bounded Contexts**: Explicit boundaries that define where a particular model applies
3. **Strategic Design**: A framework for classifying and prioritizing different parts of your domain
4. **Tactical Patterns**: Concrete building blocks (Entity, Value Object, Aggregate) for implementing domain models

### Why DDD Matters

Traditional software development often starts with database schema design or UI mockups. DDD flips this approach: **start with the business problem, model the domain deeply, and let technical decisions follow from domain understanding**.

This inversion is powerful for complex domains but adds overhead for simple applications. Understanding when to apply DDD is as important as understanding how.

> **Next:** Start with [Chapter 1: When to Use DDD](01-when-to-use-ddd.md) to determine if DDD is right for your project.

***

## References and Further Reading

### Foundational Books

1. **Evans, Eric (2003)**. _Domain-Driven Design: Tackling Complexity in the Heart of Software_ Addison-Wesley Professional ISBN: 978-0321125215
   * The original DDD book, establishing core concepts and patterns
   * Best for: Deep theoretical understanding
   * [Publisher Link](https://www.informit.com/store/domain-driven-design-tackling-complexity-in-the-heart-9780321125217)
2. **Vernon, Vaughn (2013)**. _Implementing Domain-Driven Design_ Addison-Wesley Professional ISBN: 978-0321834577
   * Practical implementation guide with detailed examples
   * Best for: Hands-on practitioners
   * [Publisher Link](https://www.informit.com/store/implementing-domain-driven-design-9780321834577)
3. **Vernon, Vaughn (2016)**. _Domain-Driven Design Distilled_ Addison-Wesley Professional ISBN: 978-0134434421
   * Condensed overview of key concepts
   * Best for: Quick reference and team onboarding
   * [Publisher Link](https://www.informit.com/store/domain-driven-design-distilled-9780134434421)

### Additional Resources

4. **Nilsson, Jimmy (2006)**. _Applying Domain-Driven Design and Patterns_ Addison-Wesley Professional ISBN: 978-0321268204
   * Practical examples with .NET
   * Combines DDD with design patterns
5. **Khononov, Vlad (2021)**. _Learning Domain-Driven Design_ O'Reilly Media ISBN: 978-1098100131
   * Modern take on DDD with current practices
   * Strategic patterns and Bounded Contexts focus

### Online Resources

* **Domain-Driven Design Community**: [dddcommunity.org](https://dddcommunity.org)
* **Eric Evans' Original Website**: Articles and presentations on DDD concepts
* **Vaughn Vernon's Blog**: Practical DDD implementation guidance

### Recommended Complementary Reading

For practical implementation, you will need framework-specific resources that demonstrate how to apply these patterns in real code. The concepts in this book are implementation-agnostic and can be applied in any programming language or framework.

***

## Learning Path

### For Beginners

1. **Read this book** (domain-driven-design/)
   * Start with [Introduction](./#introduction)
   * Study [When to Use DDD](01-when-to-use-ddd.md)
   * Learn [Strategic Design](02-strategic-design.md)
   * Understand [Tactical Patterns](03-tactical-patterns.md)
2. **Practice with examples**
   * Build sample projects in your chosen framework
   * Refer back to theory when confused

### For Experienced Developers

* Use [Quick Reference](quick-reference.md) for pattern lookups
* Review [Common Pitfalls](05-common-pitfalls.md) regularly
* Consult [Appendix A](appendix-a-strategic-classification.md) for strategic decisions

***

**Remember:** This is a **conceptual reference book**. The patterns and principles here are technology-agnostic and require framework-specific knowledge to implement in real projects.

**Start reading:** [Chapter 1: When to Use DDD →](01-when-to-use-ddd.md)
