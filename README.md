# Qorvari CEO Command Center
**Maintainer:** `@Agent-QA` / `@Agent-Dev-Frontend`

## Overview
The Qorvari CEO Command Center (`ceo_dashboard.html`) is an entirely serverless Kanban integration that tracks the real-time execution states of the AI Agent matrix across 13 repositories continuously. It operates exclusively on Client-Side Rendering (CSR), meaning there is no backend web server hosting it; it queries GitHub directly.

## Addressing Authentication Friction (PAT)
Originally, the CEO had to manually paste the GitHub Personal Access Token (PAT) every time the dashboard was opened. 
Based on UX feedback, the dashboard now uses **Persistent Auto-Uplink**:
1. When you enter the PAT once, it is securely locked into your browser's encrypted `localStorage`.
2. Upon all future refreshes, the Javascript engine immediately intercepts the token and bypasses the login screen entirely. You will never see the login box again on that machine.
***If you wish to clear it, click the "Disconnect" button in the upper right corner to scrub the browser cache.***

## Agile Active Sprints
The dashboard natively supports JIRA-style execution tracking:
- Any ticket labeled **[SPRINT 3]** (or higher than the current active sprint) is mathematically stripped from the main view and collapsed into the Icebox.
- When you click **Complete Sprint ▶**, the active counter rolls forward and the new backlog falls dynamically into scope.

## 1-Click Approve Integration
When a bot resolves an issue (e.g. generating a Dockerfile) it will check out a branch, push the code, open a Pull Request, and set the label to `ceo-approval`. 
Clicking **✅ 1-CLICK APPROVE** on the dashboard fires a direct `PATCH` REST API call to GitHub, which mathematically merges the Pull Request and moves the ticket to the **Sprint History** column to calculate sprint velocity.
