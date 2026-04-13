# Qorvari: 24/7 Cloud Operations Plan

If the agents only run when your MacBook is open, you are not a SaaS company. To operate globally autonomously, we must migrate the "Brain" to the cloud.

## Part 1: Making the CEO Dashboard Publicly Available
You need to be able to check on your AI employees from your phone, anywhere in the world.
1.  **The Solution:** We will push `ceo_dashboard.html` to the `qorvari-docs` repository and enable **GitHub Pages**.
2.  **How it works seamlessly:** You navigate to `https://qorvari-crypto.github.io/qorvari-docs/ceo_dashboard.html` on your phone. Because the token is only stored in your device's browser, the dashboard remains completely secure. No one else can see your agents' work even though the HTML file is hosted.

## Part 2: How the Agents Run 24/7 (The Serverless Workforce)
We could rent an AWS EC2 instance to run the agents, but we want to remain **Budget-Friendly**. The best solution is **GitHub Actions**.

### The Cloud Orchestrator Workflow
Instead of running `orchestrator.py` on your laptop, we will move it to: `.github/workflows/ai-orchestrator.yml`.

1.  **The Cron Trigger:** The GitHub Action is programmed to run every hour (`cron: '0 * * * *'`).
2.  **The Sweep:** Every hour, GitHub spins up a free Ubuntu server (the Action Runner). It runs the Orchestrator Python script.
3.  **The Agent Execution:** 
    *   The script reads the backlogs of all 13 repos.
    *   It identifies that `@Agent-Dev-Backend` has an open ticket.
    *   The GitHub Runner contacts the LLM API (OpenAI/Gemini/Anthropic) using an API key stored securely in **GitHub Secrets**.
    *   The LLM writes the code. The GitHub Runner commits the code and opens a Pull Request automatically.
4.  **The Sleep:** The script finishes, and the runner shuts down, costing you $0.

### Real-Time Event Triggers (Webhooks)
We don't just have to wait for the cron schedule. If YOU (the CEO) leave a comment on a PR saying "Fix this bug", GitHub Webhooks will instantly "wake up" the Orchestrator GitHub Action, letting the Developer Agent respond and submit a hotfix within 2 minutes.

With this setup, your AI company works 24 hours a day, 7 days a week, pushing code while you sleep, and requires zero servers from you.
