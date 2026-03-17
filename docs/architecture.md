# Architecture

## Overview
<!-- One paragraph: what kind of system is this? Monolith, microservices, modular monolith? -->
[TODO: High-level architecture description]

## System Context
<!-- How does this system fit into the larger landscape? External systems, APIs, users -->
[TODO: System context — what connects to what]

## Software Layers

### Presentation Layer
<!-- Controllers, REST endpoints, UI serving -->
[TODO: Define]

### Business Layer
<!-- Services, domain logic, orchestration -->
[TODO: Define]

### Persistence Layer
<!-- Repositories, database access, caching -->
[TODO: Define]

### Cross-Cutting Concerns
<!-- Security, logging, monitoring, error handling -->
[TODO: Define]

## Package / Module Structure
```
src/main/java/com/cgsit/[project]/
├── config/          ← Spring/Quarkus configuration
├── [module]/
│   ├── api/         ← REST controllers + DTOs
│   ├── domain/      ← Entities, value objects, domain services
│   └── persistence/ ← Repositories, DB mappers
├── common/          ← Shared utilities, base classes
└── security/        ← Authentication, authorization
```

### Class Naming Conventions

| Layer | Suffix | Example | File Pattern |
|-------|--------|---------|--------------|
| **API** | `*Resource.java` | `PortfolioResource`, `UserResource` | `**/api/*Resource.java` |
| **API DTOs** | `*Dto.java`, `*Request.java` | `PortfolioDto`, `CreateUserRequest` | `**/api/dto/*.java` |
| **Service** | `*Service.java` | `PortfolioService`, `AuthService` | `**/service/*Service.java` |
| **Domain** | Entity name (no suffix) | `Portfolio`, `User`, `Security` | `**/domain/*.java` |
| **Persistence** | `*Repository.java` | `PortfolioRepository`, `UserRepository` | `**/persistence/*Repository.java` |
| **Flyway** | `V*__*.sql` | `V1.0.0__initial_schema.sql` | `**/db/migration/**/*.sql` |

### Frontend Naming Conventions

| Type | Pattern | Example |
|------|---------|---------|
| **Component** | `kebab-case.component.ts/html/scss` | `portfolio-detail.component.ts` |
| **Service** | `kebab-case.service.ts` | `portfolio.service.ts` |
| **Models** | `api.models.ts` | Shared interfaces/types |
| **E2E Tests** | `*.spec.ts` in `e2e/` | `smoke.spec.ts` |

## Cloud / Infrastructure
<!-- AWS, Azure, Docker, Kubernetes — what runs where -->
[TODO: Define deployment architecture]

### Environments
| Environment | Purpose              | Infrastructure       |
|-------------|----------------------|----------------------|
| Local       | Development          | [TODO: Docker Compose?] |
| Dev/Test    | Integration testing  | [TODO: AWS/Azure?]  |
| Staging     | Pre-production       | [TODO]               |
| Production  | Live system          | [TODO]               |

## Data Architecture
<!-- Database(s), schemas, data flow -->
- **Primary DB**: [TODO: PostgreSQL? Version?]
- **Caching**: [TODO: Redis? None?]
- **Search**: [TODO: Elasticsearch? None?]
- **Message Broker**: [TODO: ActiveMQ? RabbitMQ? None?]

## Key Architecture Decisions
<!-- Important decisions and their rationale — brief, link to RFCs for details -->
1. [TODO: Decision 1 — e.g., "Modular monolith over microservices because..."]
2. [TODO: Decision 2]
