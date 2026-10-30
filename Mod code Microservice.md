```mermaid
graph TD
    %% Definição dos Clientes e Entrada
    Client[Web / Mobile Client] -->|HTTPS / REST| API Gateway[API Gateway / Spring Cloud Gateway]
    
    %% Camada de Segurança e Roteamento
    subgraph Security & Routing
        API Gateway --> Auth[OAuth2 / JWT Server]
    end

    %% Microsserviços Principais
    subgraph Core Architecture
        API Gateway -->|Async Dispatch| LedgerService[Ledger Engine Service - Java 21]
        API Gateway -->|REST| TaxService[Tax Calculation Microservice - Quarkus]
    end

    %% Processamento Assíncrono e Mensageria
    subgraph Event Stream
        LedgerService -->|Publish Events| Kafka[Apache Kafka Cluster]
        Kafka -->|Consume Events| AuditWorker[Audit & Reporting Worker - Go]
    end

    %% Banco de Dados e Cache
    subgraph Data & Storage
        LedgerService -->|ACID Transactions| Postgres[(PostgreSQL - Ledger DB)]
        TaxService -->|Read Tax Rates| Redis[(Redis Cache)]
        AuditWorker -->|Store Logs| NoSQL[(MongoDB - Audit Trail)]
    end
```