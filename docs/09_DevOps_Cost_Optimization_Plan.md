# Qorvari: DevOps Infrastructure & Cost Optimization Plan
**Prepared by:** `@Agent-DevOps` & `@Agent-FinOps`
**Status:** DRAFT (Awaiting CEO Approval)

## 1. The Strategy: "Serverless by Default"
Legacy operations scale costs linearly. Qorvari will operate on a "Scale-to-Zero" architecture using serverless and containerized deployments. This ensures we incur costs only when clients are actively running discovery paths or orchestrator loops.

## 2. Infrastructure Tiers

### Tier 1: Open Source (Community Edition)
*   **Deployment Model:** Docker Compose (Local to user).
*   **Cost to Qorvari:** $0.00
*   **Plan:** Provide a public `docker-compose.yml` that spins up the UI, Discovery Initiator, and single-tenant Postgres database entirely on the end-user's local hardware.

### Tier 2: Qorvari Pro (SaaS)
*   **Compute:** AWS Fargate (Serverless ECS) 
    *   *Why:* No EC2 instances to patch. Containers scale down to 0 at night.
*   **Database:** AWS Aurora Serverless v2 (PostgreSQL)
    *   *Why:* Scales storage and compute dynamically based on multi-tenant load.
*   **Storage:** Amazon S3 (for Discovery Reports and Assessment outputs)
*   **Cost Estimate:** ~$150/month base cost to support up to 50 active B2B clients.

### Tier 3: The AI Workforce (Internal)
*   **Task Runners:** GitHub Actions.
*   **LLM Inference:** Anthropic Claude (Opus/Sonnet) / OpenAI (GPT-4o).
*   **Cost Estimate:** GitHub Actions provides 2,000 minutes free. LLM API costs are capped at $250/month across all agents.

## 3. FinOps Guardrails
We are implementing three mandatory guardrails:
1.  **Hard LLM Budget Limit:** Daily API spend across the agent workforce cannot exceed $10.00 without CEO intervention.
2.  **AWS Billing Alarms:** Immediate Slack/Email alert if AWS daily spend exceeds $15.00.
3.  **Auto-Pruning:** All S3 objects (discovery reports) older than 90 days are transitioned to Glacier storage ($0.004/GB).

**[ ] Awaiting CEO Approval to begin Terraform generation for the AWS Fargate layout.**
