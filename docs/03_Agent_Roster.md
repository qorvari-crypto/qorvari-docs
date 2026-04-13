# The Qorvari Agent Roster

This is the official list of AI "Employees." Each agent has distinct system prompts, constraints, and tool access to ensure they never sabotage each other.

### 1. The Strategy & Planning Agents
*   **`@Agent-RandD` (Market Intelligence):** Scrapes the market, reads competitor product updates, and submits trend reports.
*   **`@Agent-PO` (Product Owner):** Consumes R&D reports, writes detailed Product Requirements Documents (PRDs), and creates formatted User Stories.
*   **`@Agent-ScrumMaster` (Agile Lead):** Takes PO tickets, bundles them into 2-week Sprints on GitHub Projects, and assigns Developer Agents.
*   **`@Agent-HR-Performance` (NEW):** We added this role to monitor *Agent Hallucinations*. It evaluates the API token cost and PR rejection rate of Developer agents, actively tuning their prompts if they are failing.

### 2. The Marketing & Support Agents
*   **`@Agent-SEO-Writer` (Content/Docs):** Reads merged PRs and automatically writes SEO-optimized Technical Tutorial blog posts.
*   **`@Agent-Social-Lead` (Community):** Automatically blasts Twitter/Reddit whenever a massive Open Source update is published.
*   **`@Agent-CustomerSuccess` (L1 Support):** Ingests incoming GitHub Issues/Zendesk tickets, analyzes user logs, and formulates helpful replies. 

### 3. The Execution Agents (Engineering)
*   **`@Agent-Recon` (Auditor):** Reads legacy code files and generates Technical Debt Jira tickets.
*   **`@Agent-Dev-Backend` (Python/Java):** Receives code tickets, rewrites logic, and enforces OpenAPI (Swagger) standards.
*   **`@Agent-Dev-Frontend` (Angular/UI):** Adjusts the visual dashboards and integrates APIs.
*   **`@Agent-DBA` (Database Admin):** Strictly handles PostgreSQL migrations and enforcing `tenant_id` logic for SaaS isolation.
*   **`@Agent-TechWriter` (Documentation):** Authors Architecture Decision Records (ADRs) whenever a structural shift happens.

### 4. The Gatekeeper Agents (Quality & Security)
*   **`@Agent-QA-SDET` (Automation Tester):** Reads Dev Pull Requests, generates localized Pytest routines, and triggers Playwright browser paths. Rejects PRs if tests fail.
*   **`@Agent-SecOps` (Red Team):** Scans all PRs for AWS keys, leaked corporate branding, and vulnerable dependency versions.

### 5. The Infrastructure Agents
*   **`@Agent-DevOps` (SysAdmin):** Takes approved staging code and uses Terraform/Pulumi to deploy temporary cloud clusters for testing.
*   **`@Agent-FinOps` (Accountant):** Watches Amazon AWS & LLM API billing. Cuts the Orchestrator off if we exceed our daily $10 maximum budget.
