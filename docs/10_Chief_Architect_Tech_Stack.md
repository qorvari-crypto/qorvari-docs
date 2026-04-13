# Qorvari: Chief Architect Technical Strategy
**Prepared by:** `@Agent-ChiefArchitect`
**Status:** DRAFT (Awaiting CEO Approval)

## 1. Technical Optimization Goal
The original `matilda` codebase relies heavily on traditional, somewhat monolithic patterns (large Spring Boot modules, dense Flask applications, hardcoded systemd files). 
Our primary architectural goal is **Decoupling and Modernization** without initiating a total rewrite.

## 2. Target Tech Stack (Industrial Standard)

| Domain | Current Legacy Tech | Target Modern Tech | Rationale |
| :--- | :--- | :--- | :--- |
| **Frontend UI** | Angular (Monolithic) | Angular with Nx Monorepo | Enables micro-frontends and faster, independent compilation. (Already partially in place). |
| **Core Discovery Backend** | Python Flask (systemd) | FastAPI (Dockerized) | FastAPI provides auto-generated OpenAPI (Swagger) docs natively, handles async better, and is the industry SaaS standard over Flask. |
| **Enterprise Services** | Java Spring Boot | Spring Boot + GraalVM | Converting Spring Boot apps to Native GraalVM images reduces memory consumption from 1GB+ down to ~50MB per service. Critical for cost-efficient scaling. |
| **Database** | Shared schema | Multi-tenant PostgreSQL (Row-Level Security) | Absolute requirement for B2B SaaS. RLS ensures client A cannot see client B's discovery data without spinning up 100 different databases. |

## 3. Communication Protocol standardization
1.  **Synchronous (UI -> Backend):** RESTful HTTP/2 APIs using pure JSON. Every endpoint must be documented via OpenAPI v3.
2.  **Asynchronous (Microservice -> Microservice):** Moving away from shared database polling to event-driven architectures using **RabbitMQ** or **AWS SQS**.

## 4. Immediate Architectural Refactoring Targets
*   *Target 1:* Rip out all references to `/etc/systemd/...` in Python apps, replace with `Dockerfile` entrypoints.
*   *Target 2:* Implement a unified API Gateway layer (Kong or NGINX) so the front-end only talks to one secure endpoint, rather than managing 13 different microservice ports.

**[ ] Awaiting CEO Approval to assign refactoring targets to the Developer Agents.**
