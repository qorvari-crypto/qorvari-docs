# Qorvari: Sprint 2 Planning (Containerization & True Autonomy)
**Date:** Current
**Attendees:** `@Agent-ProductOwner`, `@Agent-ScrumMaster`, `@Agent-Devops`

## 1. Retrospective Review & Sprint Goal
In Sprint 1, we successfully built the local orchestrator and scrubbed the legacy IP. However, we identified a critical execution gap: the bots write code locally but do not open remote Pull Requests on GitHub. 

**Sprint 2 Goal:**
1. Upgrade the Dev Agents with Git-hook autonomy so they commit their own code.
2. Begin executing the FinOps strategy by wrapping the legacy `matilda` codebase into lightweight, isolated Docker containers to drastically reduce cloud execution costs.

## 2. Epic 1: True Cloud Autonomy (Git Hooks)
*   **Target:** `qorvari-platform`
*   **Assigned Bot:** `@Agent-Dev-Backend`
*   **User Story:** As the CEO, I want the bot to automatically `git commit` and `git push` to a new branch, and open a Pull Request via the GitHub API, so I can review the lines of code before I click the 1-Click Approve button.

## 3. Epic 2: The Infrastructure Containerization
*   **Target:** `qorvari-discovery-core`
*   **Assigned Bot:** `@Agent-Devops`
*   **User Story:** As the FinOps architect, I want the legacy monolithic Java repository packaged into a multi-stage `Dockerfile`. 
*   **Acceptance Criteria:**
    *   Must use generic Alpine base images to minimize footprint.
    *   Must compile via Maven/Gradle entirely within the Docker builder layer to prevent dependency drift on the host machine.
