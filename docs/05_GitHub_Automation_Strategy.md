# Qorvari: GitHub Automation Strategy

## The Short Answer
**Yes.** You sign in once, generate a Personal Access Token (PAT), and every agent uses that token to autonomously create repos, open Issues, move Kanban cards, open PRs, and trigger CI/CD — without you ever logging into GitHub again.

---

## How It Works (One-Time Setup)

### Step 1: You Sign In Once (5 minutes)
1. Go to `github.com` → Settings → Developer Settings → **Personal Access Tokens (Fine-Grained)**.
2. Create a token with these scopes:
   - `repo` — Full control of repositories (create, read, write, delete).
   - `project` — Read/write access to GitHub Projects (our JIRA board).
   - `workflow` — Trigger GitHub Actions (CI/CD pipelines).
3. Copy the token. **This is the ONLY manual step you ever do.**

### Step 2: Store the Token Securely
```bash
# Store in a local .env file that the Orchestrator reads
echo "GITHUB_TOKEN=ghp_xxxxxxxxxxxxxxxxxxxx" > /Qorvari/config/.env
echo "GITHUB_ORG=qorvari" >> /Qorvari/config/.env
```
The `.env` file is added to `.gitignore` so it never leaks.

### Step 3: Everything After This Is Automated
The Orchestrator and all agents use the GitHub REST API (via the `PyGithub` or `requests` library) with your token. Here is exactly what each agent can do autonomously:

---

## What Gets Automated (Per Agent)

| Agent | GitHub Action (Automated) | How |
|:------|:--------------------------|:----|
| **PO Agent** | Creates GitHub Issues with labels like `feature`, `bug`, `tech-debt` | `POST /repos/{owner}/{repo}/issues` |
| **Scrum Master** | Moves Issues into Sprint columns on the Projects board | `PATCH /projects/columns/cards/{card_id}` |
| **Dev Agent** | Creates a branch, commits code, opens a Pull Request | `git push` + `POST /repos/{owner}/{repo}/pulls` |
| **QA Agent** | Adds review comments on PRs, approves or rejects | `POST /repos/{owner}/{repo}/pulls/{pull}/reviews` |
| **DevOps Agent** | Triggers GitHub Actions workflows for deployment | `POST /repos/{owner}/{repo}/actions/workflows/{id}/dispatches` |
| **Orchestrator** | Reads board state, assigns agents, tracks progress | `GET /projects/{id}/columns` |
| **FinOps Agent** | Reads Actions usage minutes to track CI/CD costs | `GET /orgs/{org}/settings/billing/actions` |

---

## What You (The CEO) See

Once this is wired up, you simply open `github.com/orgs/qorvari/projects/1` in your browser and you see a live Kanban board:

```
┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│   BACKLOG    │  │  IN SPRINT   │  │ AGENT ACTIVE │  │  QA REVIEW   │  │ CEO APPROVAL │
│              │  │              │  │              │  │              │  │              │
│ #12 Add SAML │  │ #8 De-brand  │  │ #5 Multi-    │  │ #3 Docker    │  │ #1 Open      │
│ #13 Cost API │  │    discovery │  │   tenant DB  │  │   compose    │  │   Source Drop│
│ #14 Pulumi   │  │ Assigned:    │  │ Assigned:    │  │ Assigned:    │  │ WAITING FOR  │
│              │  │ @Dev-Backend │  │ @Agent-DBA   │  │ @Agent-QA    │  │ YOU ✋       │
└──────────────┘  └──────────────┘  └──────────────┘  └──────────────┘  └──────────────┘
```

Cards move automatically as agents complete work. The ONLY column that requires YOUR click is `CEO Approval` → `Done`.

---

## Cost: $0

- GitHub Organizations: **Free** (public repos).
- GitHub Projects V2: **Free** (unlimited).
- GitHub Actions: **Free** (2,000 minutes/month for free tier).
- Personal Access Token: **Free**.

**Total recurring cost for the tracking + automation layer: $0/month.**
