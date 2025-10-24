# Chapter 1: When to Use DDD

DDD is a powerful approach, but it comes with significant overhead. Knowing when to apply it is crucial for project success.

## ✅ Use DDD When

Apply Domain-Driven Design when you have:

### 1. Complex Business Domain

* Rich business logic with many rules and edge cases
* Domain experts needed to understand the problem space
* Business rules that change frequently based on market conditions

### 2. Core Competitive Advantage

* Your domain logic differentiates you from competitors
* The business model is unique or innovative
* Getting the domain model right directly impacts revenue

### 3. Long-Lived Application

* Expected lifespan > 1 year
* Will evolve and grow over time
* Needs to adapt to changing business requirements

### 4. Team Collaboration Possible

* Access to domain experts (business analysts, product owners, end users)
* Time allocated for domain modeling sessions
* Stakeholder buy-in for iterative refinement

## ❌ Do NOT Use DDD When

Avoid DDD in these scenarios:

### 1. Simple CRUD Operations

* Mostly data entry and retrieval
* Minimal business logic
* Database-centric applications

### 2. Data-Driven Applications

* Reporting tools
* Analytics dashboards
* ETL pipelines

### 3. Technical Complexity > Domain Complexity

* The challenge is infrastructure, not business rules
* High-performance computing problems
* Protocol implementations

### 4. Tight Constraints

* Prototypes or proof-of-concepts
* Very small projects (<3 months)
* Limited budget or resources

## Decision Framework

Ask yourself:

1. Is the **business logic** complex? → If yes, continue
2. Does the domain **differentiate** us from competitors? → If yes, continue
3. Will this application **live for years**? → If yes, continue
4. Do we have **domain expert access**? → If yes, DDD is a good fit

**If you answered "no" to 2 or more questions, consider simpler approaches first.**

***

**Navigation:**

* [← Back to Introduction](../../#introduction)
* [Next: Strategic Design →](02-strategic-design.md)
* [Table of Contents](../../#table-of-contents)
