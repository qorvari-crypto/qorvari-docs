# Qorvari: R&D Feature Roadmap (Matilda vs. Qorvari)
**Prepared by:** `@Agent-R&D`

## 1. Legacy Analysis ("Matilda")
The original Matida architecture is heavily focused on basic, rules-based infrastructure discovery via SSH/WMI. While robust, its data output is static—it dumps large JSON documents and Excel reports of what it finds.

## 2. Qorvari Future Features (SaaS & AI Pivot)
To make Qorvari an enterprise winner, we must pivot from "static reporting" to "Interactive AI Automation." 

**Feature 1: Conversational Architecture (Chat-with-your-infra)**
*   *Idea:* Instead of parsing Excel sheets, engineers type: `"Show me all EC2 instances running vulnerable Java versions that connect to the Oracle DB."`
*   *Implementation:* Hook the output of the Discovery Engine into a Vector Database (Pinecone) and use LLMs to perform Retrieval-Augmented Generation (RAG).

**Feature 2: Automated Cost-Optimization Actions**
*   *Idea:* Matilda tells you what you have. Qorvari will tell you how much money you are wasting and offer a one-click fix.
*   *Implementation:* Integrate AWS Pricing APIs against discovered hardware, map underutilized CPUs, and trigger Terraform to dynamically downsize them.

**Feature 3: Continuous Drift Detection**
*   *Idea:* Move from point-in-time scans to continuous state management.
*   *Implementation:* Move from polling to event-driven webhooks, alerting you instantly if a developer provisions an unauthorized S3 bucket.
