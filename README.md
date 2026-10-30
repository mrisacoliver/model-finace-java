# model-finace-java
Financial systems review, IT auditing, and digital forensics.

# Hi there, I'm Isaque Oliveira 👋

### Senior Software Engineer | Financial Systems & Digital Forensics Specialist

I am a Senior Software Engineer with over 7 years of experience building high-throughput distributed systems, event-driven architectures, and ACID-compliant financial engines. My core differentiator lies at the intersection of **scalable software development, financial IT audit, tax compliance, and forensic data integrity**.

Former **Senior Tax Consultant at Michelin** and **Court-Appointed Judicial Expert**, I combine deep hands-on engineering execution with an auditor's analytical mindset to build bulletproof, transparent, and audit-ready enterprise software.

---

### 🛠️ Core Tech Stack & Ecosystem
--
```mermaid
sequenceDiagram
    autonumber
    Client->>API: POST /api/v1/ledger/transaction
    API->>Auth: Validate JWT Token
    Auth-->>API: Token Valid
    API->>TaxService: Get Tax Rate (ICMS/PIS)
    TaxService-->>API: Return Rate (18%)
    API->>DB: Save Ledger Record (ACID)
    DB-->>API: Transaction Committed
    API-->>Client: 201 Created (Transaction ID)
```

Microservice

```mermaid
graph TD
    %% Entradas do Sistema
    Client[Client Apps / ERP / Payment Gateway] -->|HTTPS / REST API| APIGateway[Spring Cloud Gateway]

    %% Camada de Autenticação e Entrada
    subgraph Edge Layer
        APIGateway -->|Validate Token| Security[Spring Security / OAuth2 JWT]
    end

    %% Core Application Layer (Java Microservice)
    subgraph Financial Calculation Microservice - Java 21
        APIGateway -->|REST Requests| Controller[REST API Controllers]
        
        subgraph Clean Architecture / DDD Core
            Controller -->|DTOs| StrategyEngine[Tax & Calculation Strategy Engine]
            StrategyEngine -->|Rules Execution| LedgerService[Ledger & Accounting Service]
            LedgerService -->|Domain Events| EventPublisher[Event Publisher]
        end
    end

    %% Camada de Processamento Assíncrono
    subgraph Messaging & Async Processing
        EventPublisher -->|Publish Transaction Events| Kafka[Apache Kafka Broker]
        Kafka -->|Consume Events| AuditWorker[Audit & Log Consumer Worker]
    end

    %% Camada de Persistência e Cache
    subgraph Data & Storage Layer
        StrategyEngine -->|Cache Tax Rates| Redis[(Redis Cache)]
        LedgerService -->|ACID Transactions| Postgres[(PostgreSQL - Double-Entry Ledger DB)]
        AuditWorker -->|Immutable Logs| NoSQL[(MongoDB - Financial Audit Trail)]
    end
```

graph TD
    %% Clientes e Entrada
    ClientApp[ERP / Banking Services / Payment Gateway] -->|POST /api/v1/ledger/entries| Gateway[Spring Cloud Gateway]

    %% Camada de Segurança e Roteamento
    subgraph Edge & Security Layer
        Gateway -->|Validate JWT| OAuthServer[OAuth2 / OIDC Server]
    end

    %% Core Application Layer (Financial Ledger Engine)
    subgraph Financial Ledger Engine - Java 21 / Spring Boot
        Gateway -->|HTTPS/REST| Controller[Ledger Entry Controller]
        
        subgraph Domain Model & Validation
            Controller -->|Validate Balance| AccountingService[Double-Entry Ledger Service]
            AccountingService -->|Enforce Balancing| BalanceValidator[Debit/Credit Equality Engine]
        end

        subgraph Event Generation
            AccountingService -->|Publish Domain Events| EventPublisher[Spring Event Publisher]
        end
    end

    %% Camada de Eventos Assíncronos
    subgraph Event Broker & Messaging
        EventPublisher -->|Produce Message| Kafka[Apache Kafka - Ledger Events Topic]
        Kafka -->|Consume Transaction Logs| AuditWorker[Audit & Compliance Worker - Go]
        Kafka -->|Consume for Analytics| AnalyticsWorker[Financial Reporting Worker]
    end

    %% Camada de Persistência e Cache
    subgraph Persistence & Audit Layer
        AccountingService -->|ACID Ledger Entries| Postgres[(PostgreSQL - Relational Ledger DB)]
        AccountingService -->|Cache Balance State| Redis[(Redis - Fast Balance Cache)]
        AuditWorker -->|Store Immutable Log| MongoAudit[(MongoDB - Forensic Audit Trail)]
    end
