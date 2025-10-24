# Domain-Driven Design
## A Conceptual Foundation

*Based on the works of Eric Evans (2003) and Vaughn Vernon (2013)*

---

## About This Book

This book provides a comprehensive conceptual foundation for Domain-Driven Design (DDD), focusing on the principles and patterns that enable you to tackle complexity in software development through deep domain understanding and strategic design.

**Type:** Conceptual reference book (technology-agnostic)

---

## How to Read This Book

**If you're new to DDD:**
1. Start with [Introduction](#introduction) to understand the core philosophy
2. Read [Chapter 1: When to Use DDD](01-when-to-use-ddd.md) to assess applicability
3. Study [Chapter 2: Strategic Design](02-strategic-design.md) before diving into tactical patterns
4. Reference [Chapter 3: Tactical Patterns](03-tactical-patterns.md) as needed

**If you're experienced:**
- Use the [Table of Contents](#table-of-contents) to jump to specific patterns
- Review [Chapter 5: Common Pitfalls](05-common-pitfalls.md) to avoid known issues
- Consult the [Quick Reference](quick-reference.md) for pattern comparisons

---

## Table of Contents

### [Introduction](#introduction)
Understanding the Philosophy Behind DDD

### Core Chapters

1. **[When to Use DDD](01-when-to-use-ddd.md)**
   Applicability and Decision Criteria

2. **[Strategic Design](02-strategic-design.md)**
   - Domain and Subdomain
   - Bounded Context
   - Ubiquitous Language
   - Strategic Classification

3. **[Tactical Patterns](03-tactical-patterns.md)**
   - Entity
   - Value Object
   - Aggregate
   - Domain Events
   - Repository
   - Domain Services

4. **[Integration Patterns](04-integration-patterns.md)**
   - Context Map
   - Shared Kernel
   - Anti-Corruption Layer

5. **[Common Pitfalls](05-common-pitfalls.md)**
   Mistakes to Avoid

### Appendices

- **[Appendix A: Strategic Classification Framework](appendix-a-strategic-classification.md)**
  Detailed framework for classifying subdomains (Core, Supporting, Generic) with e-commerce examples

- **[Quick Reference](quick-reference.md)**
  Pattern comparisons, decision trees, and cheat sheets

### [References and Further Reading](#references-and-further-reading)

---

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

---

## References and Further Reading

### Foundational Books

1. **Evans, Eric (2003)**. *Domain-Driven Design: Tackling Complexity in the Heart of Software*
   Addison-Wesley Professional
   ISBN: 978-0321125215
   - The original DDD book, establishing core concepts and patterns
   - Best for: Deep theoretical understanding
   - [Publisher Link](https://www.informit.com/store/domain-driven-design-tackling-complexity-in-the-heart-9780321125217)

2. **Vernon, Vaughn (2013)**. *Implementing Domain-Driven Design*
   Addison-Wesley Professional
   ISBN: 978-0321834577
   - Practical implementation guide with detailed examples
   - Best for: Hands-on practitioners
   - [Publisher Link](https://www.informit.com/store/implementing-domain-driven-design-9780321834577)

3. **Vernon, Vaughn (2016)**. *Domain-Driven Design Distilled*
   Addison-Wesley Professional
   ISBN: 978-0134434421
   - Condensed overview of key concepts
   - Best for: Quick reference and team onboarding
   - [Publisher Link](https://www.informit.com/store/domain-driven-design-distilled-9780134434421)

### Additional Resources

4. **Nilsson, Jimmy (2006)**. *Applying Domain-Driven Design and Patterns*
   Addison-Wesley Professional
   ISBN: 978-0321268204
   - Practical examples with .NET
   - Combines DDD with design patterns

5. **Khononov, Vlad (2021)**. *Learning Domain-Driven Design*
   O'Reilly Media
   ISBN: 978-1098100131
   - Modern take on DDD with current practices
   - Strategic patterns and Bounded Contexts focus

### Online Resources

- **Domain-Driven Design Community**: [dddcommunity.org](https://dddcommunity.org)
- **Eric Evans' Original Website**: Articles and presentations on DDD concepts
- **Vaughn Vernon's Blog**: Practical DDD implementation guidance

### Recommended Complementary Reading

For practical implementation examples, look for technology-specific DDD cookbooks and hexagonal architecture guides in your framework of choice.

---

## Learning Path

### For Beginners

1. **Read this book** (domain-driven-design/)
   - Start with [Introduction](#introduction)
   - Study [When to Use DDD](01-when-to-use-ddd.md)
   - Learn [Strategic Design](02-strategic-design.md)
   - Understand [Tactical Patterns](03-tactical-patterns.md)

2. **Practice with examples**
   - Build sample projects in your chosen framework
   - Refer back to theory when confused

### For Experienced Developers

- Use [Quick Reference](quick-reference.md) for pattern lookups
- Review [Common Pitfalls](05-common-pitfalls.md) regularly
- Consult [Appendix A](appendix-a-strategic-classification.md) for strategic decisions

---

**Remember:** This is a **conceptual reference book**. For code examples and practical implementation, look for technology-specific cookbooks in your chosen framework.

**Start reading:** [Chapter 1: When to Use DDD →](01-when-to-use-ddd.md)
