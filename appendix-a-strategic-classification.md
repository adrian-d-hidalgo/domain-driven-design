# Appendix A: Strategic Classification Framework

This appendix provides a detailed framework for classifying subdomains and making investment decisions. We use an **e-commerce platform** as the reference example throughout.

## Strategic Classification Overview

| Type | Investment | Strategy | Purpose & Guidance |
|------|-----------|----------|----------------------|
| **Core** | 60-70% | Custom-built, best team, full DDD & tests | Encodes unique business advantage. High change rate, tight quality bar, explicit boundaries, event-driven where useful. |
| **Supporting** | 20-30% | Build in-house (templates ok), modular & reliable | Enables Core to operate. Has business semantics but doesn't differentiate on its own. Focus on stability, clear contracts, and integration. |
| **Generic** | 5-10% | Buy/SaaS/OSS, minimal customization | Commodity capabilities. Prefer managed services or libraries; keep adapters thin and swappable. |

## Core Subdomain (60-70% investment)

**Definition:** The strategic heart—what makes your e-commerce platform unique and competitive.

### Investment Philosophy

> "Put the best people on the Core Domain." — Evans (2003)

### Characteristics

- ✅ Business differentiator (competitive advantage)
- ✅ Complex, frequently-changing business rules
- ✅ Proprietary knowledge and algorithms
- ✅ Primary revenue generator
- ✅ No adequate third-party solution exists

### Team & Quality

- Best talent (senior developers + domain experts)
- Highest quality standards (90%+ test coverage)
- Full DDD tactical patterns (Aggregates, Events, etc.)
- Continuous innovation and iteration
- Event-driven architecture where beneficial
- Strategic IP protection

### E-Commerce Core Examples

| Subdomain | Module Path | Why It's Core | Key Capabilities |
|-----------|------------|---------------|------------------|
| **Product Recommendation Engine** | `recommendation/` | Proprietary ML algorithms drive conversions | Personalized suggestions, collaborative filtering, real-time behavioral analysis |
| **Dynamic Pricing** | `pricing-optimizer/` | Competitive advantage through intelligent pricing | Demand forecasting, competitor analysis, margin optimization, A/B testing |
| **Inventory Allocation** | `inventory-optimizer/` | Multi-warehouse optimization is unique to business | Smart allocation across warehouses, demand prediction, stock rebalancing |
| **Personalized Search** | `search-personalization/` | Custom ranking algorithms improve discovery | User-intent analysis, query expansion, personalized ranking, learning to rank |
| **Checkout Optimization** | `checkout-optimizer/` | Proprietary funnel optimization reduces cart abandonment | One-click checkout, smart field pre-filling, payment method optimization |
| **Fraud Detection** | `fraud-detection/` | Custom ML models protect revenue | Real-time risk scoring, pattern detection, behavioral analysis |

### Decision Criteria: Core if ≥4 met

- [ ] Directly generates revenue or prevents loss
- [ ] Differentiates from competitors
- [ ] Requires deep domain expertise
- [ ] Changes frequently (weekly/monthly)
- [ ] No adequate third-party solution
- [ ] Customers notice and value it

## Supporting Subdomain (20-30% investment)

**Definition:** Necessary for operations but not a competitive differentiator. Enables Core to function.

### Characteristics

- ⚙️ Business necessity (Core can't work without it)
- ⚙️ Medium complexity with business-specific logic
- ⚙️ Stable requirements (changes quarterly/yearly)
- ⚙️ Some customization needed, but templates work
- ⚙️ "Build vs Buy" is debatable

### Team & Quality

- Competent mid-level developers
- Good engineering quality (70-80% test coverage)
- Modular, well-documented, reliable
- Clear contracts and integration points
- Focus on stability over innovation

### E-Commerce Supporting Examples

| Subdomain | Module Path | Why It's Supporting | Key Capabilities |
|-----------|------------|---------------------|------------------|
| **Account Management** | `account/` | Standard but with business rules | User profiles, organization/tenancy, role hierarchy, account settings |
| **Order Management** | `order/` | Core workflows but standard patterns | Order creation, status tracking, cancellation, refund processing |
| **Catalog Management** | `catalog/` | Product data with business semantics | Product CRUD, categories, attributes, variants, SKU management |
| **Permissions** | `permissions/` | RBAC/ABAC with custom rules | Role management, permission policies, resource-based access control |
| **Billing** | `billing/` | Subscription/quota logic | Plan management, usage tracking, invoicing, payment scheduling |
| **Notification** | `notification/` | Multi-channel orchestration | Template management, channel selection, delivery scheduling, preferences |
| **Shipping** | `shipping/` | Carrier integration + business rules | Rate calculation, label generation, tracking, return management |
| **Promotion** | `promotion/` | Discount rules engine | Coupon management, rule evaluation, eligibility checking, redemption tracking |
| **Review System** | `review/` | Rating + moderation workflow | Review submission, moderation queue, verified purchase badges, aggregation |
| **Wishlist** | `wishlist/` | User collections + sharing | List management, item tracking, privacy controls, social sharing |

### Decision Criteria: Supporting if

- [ ] Necessary for business operations
- [ ] Has some business-specific logic
- [ ] Not a competitive differentiator
- [ ] Could build or buy, but building gives more control
- [ ] Integrates deeply with Core

## Generic Subdomain (5-10% investment)

**Definition:** Commodity solutions available off-the-shelf. Zero competitive differentiation.

### Characteristics

- 📦 Standardized, solved problems
- 📦 Buy, use SaaS, or adopt open-source
- 📦 Minimal or no customization
- 📦 Thin adapter layer only
- 📦 Swappable implementations

### Team & Quality

- Junior developers for adapter maintenance
- Minimal custom code
- Focus on integration, not implementation
- Keep adapters thin and technology-agnostic

### E-Commerce Generic Examples

| Subdomain | Module Path | Solution Strategy | Typical Solutions |
|-----------|------------|-------------------|-------------------|
| **Email Delivery** | `mail-driver/` | SaaS adapter | Transactional email services |
| **Identity Provider** | `idp-adapter/` | SaaS/OSS integration | OAuth/OIDC providers, SSO solutions |
| **Payment Gateway** | `payment-adapter/` | Third-party integration | Payment processors, billing platforms |
| **File Storage** | `file-storage/` | Cloud storage driver | Object storage services |
| **CDN** | `cdn-adapter/` | Managed service | Content delivery networks |
| **Observability** | `observability/` | SaaS/OSS tooling | Logging, metrics, tracing platforms |
| **Feature Flags** | `feature-flags-adapter/` | SaaS integration | Feature management platforms |
| **PDF Generation** | `pdf-renderer-adapter/` | Library wrapper | PDF generation libraries |
| **Image Processing** | `image-processor/` | SaaS/library | Image transformation services |
| **SMS Delivery** | `sms-driver/` | SaaS adapter | SMS gateway services |
| **Video Streaming** | `video-adapter/` | SaaS platform | Video hosting platforms |
| **Cache** | `cache-adapter/` | Infrastructure service | In-memory data stores |

### Decision Criteria: Generic if

- [ ] Multiple vendors provide nearly identical solutions
- [ ] No business logic required
- [ ] Commoditized technology
- [ ] Building it provides zero competitive advantage
- [ ] Easy to swap providers

## Strategic Decision Framework

Use this flowchart to classify any subdomain:

```
Is it a competitive differentiator?
├─ Yes → Is it proprietary/unique to our business?
│  ├─ Yes → CORE (invest 60-70%)
│  └─ No → Re-evaluate: might be Supporting
│
└─ No → Can we buy/use SaaS/OSS solution?
   ├─ Yes → GENERIC (invest 5-10%)
   │
   └─ No → Does it have business-specific logic?
      ├─ Yes → SUPPORTING (invest 20-30%)
      └─ No → Re-evaluate: might be Generic with adapter
```

## Investment Allocation Guide

### Budget Distribution (100% total)

- **Core**: 60-70% of engineering resources
- **Supporting**: 20-30% of engineering resources
- **Generic**: 5-10% of engineering resources

### Time Allocation Example (10-person team)

- 6-7 developers → Core subdomains
- 2-3 developers → Supporting subdomains
- 1 developer → Generic integrations & adapters

### Quality Standards

| Type | Test Coverage | Code Review | Documentation | DDD Patterns |
|------|--------------|-------------|---------------|--------------|
| Core | 90-95% | Required (2+ reviewers) | Extensive (architecture docs) | Full tactical patterns |
| Supporting | 70-80% | Required (1+ reviewer) | Good (API contracts) | Selective (where valuable) |
| Generic | 50-60% | Optional | Minimal (integration guide) | Adapters only |

## Classification Evolution

**Important:** Classifications are not permanent. Market changes, competition, and business strategy can shift a subdomain's classification.

### Example Evolution Scenarios

| Scenario | Original | New Classification | Reason |
|----------|---------|-------------------|--------|
| Recommendation engine becomes commoditized | Core | Supporting → Generic | Third-party ML platforms now handle this well |
| Custom checkout flow becomes competitive advantage | Supporting | Core | New ML-powered optimization drives 20% conversion increase |
| Email delivery needs custom retry logic | Generic | Supporting | Business requirements exceed standard SaaS capabilities |
| Fraud detection can use third-party ML | Core | Generic | Specialized fraud platforms now provide superior solutions |

### Re-evaluation Triggers

- Quarterly strategy reviews
- Major competitive landscape shifts
- New SaaS products entering market
- Significant business model changes
- Technology maturation (e.g., ML as a service)

### Action Items

- [ ] Schedule quarterly subdomain classification review
- [ ] Track competitor capabilities in Core areas
- [ ] Monitor SaaS market for Generic alternatives
- [ ] Measure business impact of each Core subdomain

---

**Navigation:**
- [← Previous: Common Pitfalls](05-common-pitfalls.md)
- [Next: Quick Reference →](quick-reference.md)
- [Table of Contents](README.md#table-of-contents)
