# High-Level Architecture (C4 Model)
**Architect:** `@Agent-ChiefArchitect`

This document defines the physical hardware boundaries and microservice layers required to launch Qorvari into production on a serverless AWS infrastructure. By shedding the legacy monolithic EC2 allocations, the SaaS operates on a 100% elastic finOps model—costing almost $0 when inactive.

```mermaid
graph TD
    %% Define Styles
    classDef AWS fill:#FF9900,stroke:#232F3E,stroke-width:2px,color:white,font-weight:bold;
    classDef Backend fill:#326ce5,stroke:#fff,stroke-width:2px,color:white,font-weight:bold;
    classDef Database fill:#47A248,stroke:#fff,stroke-width:2px,color:white,font-weight:bold;
    classDef Analytics fill:#6366f1,stroke:#fff,stroke-width:2px,color:white,font-weight:bold;
    
    subgraph "External Entry"
      A[CEO / Enterprise Clients] -->|HTTPS 443| B(AWS Application Load Balancer):::AWS
    end
    
    subgraph "AWS Fargate (Serverless Compute)"
      B --> C{Nginx API Gateway proxy}
      C -->|/api/v1/discovery| D[Qorvari Discovery Core - Python FastAPI]:::Backend
      C -->|/api/v1/assess| E[Assessment Engine - Spring Boot GraalVM]:::Backend
      C -->|/api/v1/ui| U[Qorvari Frontend - React]:::Backend
    end
    
    subgraph "Data Persistence & AI RAG Tier"
      D --> F[(MongoDB Relational DB)]:::Database
      E --> F
      D --> G[(Pinecone Vector DB)]:::Analytics
      G -->|Conversational Integration| H[LLM OpenAI Router]:::Analytics
    end
```
