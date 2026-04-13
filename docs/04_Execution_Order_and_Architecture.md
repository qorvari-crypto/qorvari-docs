# Qorvari Execution Order: What We Build First

## The Golden Rule
> **Infrastructure → Orchestrator → Agents**
>
> You NEVER deploy soldiers before building the command center.

---

## Phase-by-Phase Build Order

### PHASE 1: The Communication & Tracking Layer (The Plumbing)
**WHY FIRST:** If you build agents before a tracking system, they will run blind. You won't know what they did, what failed, or what they spent. This is like hiring 20 employees with no Slack, no email, and no JIRA.

**What We Build:**
1. **GitHub Repository Setup** — Create the `qorvari` GitHub org with repos for each module.
2. **GitHub Projects Board** — Our free JIRA. Configure the Kanban columns:
   - `Backlog` → `Sprint Planned` → `Agent In Progress` → `QA Review` → `Staging` → `CEO Approval` → `Done`
3. **Agent Communication Protocol** — Define HOW agents talk to each other:
   - **Method:** File-based message passing via a shared `/Qorvari/agent_comms/` directory.
   - Each agent writes structured JSON messages (task assignments, status updates, PR links).
   - The Orchestrator reads this directory on a loop to decide what to trigger next.
4. **Agent Activity Log** — A single append-only Markdown log (`/Qorvari/agent_logs/activity.md`) where every agent writes what it did, when, and how many tokens it consumed.

```
Example Communication Flow:
┌─────────────┐    JSON Message     ┌──────────────┐    JSON Message    ┌──────────────┐
│  PO Agent   │ ──────────────────> │ Orchestrator │ ─────────────────> │  Dev Agent   │
│ (writes PRD)│  {type: "new_task"} │   (Router)   │ {type: "assign"}  │ (writes code)│
└─────────────┘                     └──────────────┘                    └──────────────┘
                                          │
                                          │ Reads agent_logs/
                                          │ Checks token budget
                                          ▼
                                    ┌──────────────┐
                                    │ FinOps Agent │
                                    │ (cost guard) │
                                    └──────────────┘
```

---

### PHASE 2: The Orchestrator (The Brain)
**WHY SECOND:** The Orchestrator is the ONLY agent that talks to ALL other agents. It is the central nervous system. Without it, agents don't know when to wake up or sleep.

**What We Build:**
1. **Orchestrator Core Logic** — A Python script that:
   - Polls `/agent_comms/` for new messages every N seconds.
   - Reads the GitHub Projects board state via GitHub API.
   - Decides which agent to invoke next based on ticket state.
   - Enforces the FinOps daily budget cap ($10/day default).
2. **Agent Registry** — A config file (`/Qorvari/config/agent_registry.yaml`) listing every agent, its system prompt path, its allowed tools, and its token budget.

```yaml
# Example: agent_registry.yaml
agents:
  - id: "agent-recon"
    name: "Reconnaissance Agent"
    role: "Code Auditor"
    system_prompt: "./prompts/recon_agent.md"
    allowed_tools: ["read_file", "grep", "list_dir"]
    daily_token_budget: 50000
    can_write_code: false

  - id: "agent-dev-backend"
    name: "Backend Developer Agent"
    role: "Python/Java Engineer"
    system_prompt: "./prompts/dev_backend_agent.md"
    allowed_tools: ["read_file", "write_file", "run_tests", "git_commit"]
    daily_token_budget: 100000
    can_write_code: true

  - id: "agent-qa"
    name: "QA Automation Agent"
    role: "SDET / Tester"
    system_prompt: "./prompts/qa_agent.md"
    allowed_tools: ["read_file", "write_file", "run_tests", "browser_test"]
    daily_token_budget: 75000
    can_write_code: true
```

---

### PHASE 3: The First Agent (Recon)
**WHY THIRD:** The Recon Agent is the simplest and safest agent to deploy first. It only READS code — it never writes or modifies anything. This makes it the perfect "test pilot" to validate that our Orchestrator and Communication layers are working correctly.

**What We Build:**
1. **Recon Agent System Prompt** — A detailed Markdown file defining its personality and constraints.
2. **Recon Agent Script** — A Python wrapper that:
   - Receives a module path from the Orchestrator.
   - Reads every file in the module.
   - Generates a structured audit report (dependencies, technical debt, branding issues, security flags).
   - Writes its findings back to `/agent_comms/` for the PO Agent to pick up later.

---

### PHASE 4: Scale Up Remaining Agents (Iterative)
Once the Recon Agent proves the pipeline works end-to-end:
1. **PO Agent** → Reads Recon reports, writes PRDs and User Stories.
2. **Scrum Master Agent** → Bundles stories into Sprints.
3. **Dev Agents** → Write code against Sprint tickets.
4. **QA Agent** → Reviews Dev PRs.
5. **DevOps Agent** → Deploys approved code.
6. **Marketing Agents** → Announce releases.

Each agent is added ONE AT A TIME and validated before deploying the next.

---

## Summary: The Exact First 5 Steps

| Step | What We Do | Output |
|:-----|:-----------|:-------|
| **Step 1** | Create `/Qorvari/agent_comms/` and `/Qorvari/agent_logs/` directories | Communication backbone |
| **Step 2** | Create `/Qorvari/config/agent_registry.yaml` | Agent definitions and budget caps |
| **Step 3** | Write the Orchestrator script (`orchestrator.py`) | The central brain |
| **Step 4** | Write the Recon Agent system prompt + script | First working agent |
| **Step 5** | Point Recon Agent at `discoveryinitiator` and RUN IT | First real output |
