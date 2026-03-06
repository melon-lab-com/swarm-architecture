# Proposal: The Melon Lab Autonomous Agent Loop

## Objective
To achieve **Level 5 Autonomy** where the Swarm manages its own development lifecycle (Feature -> PR -> Review -> Fix -> Merge) without human intervention.

## Phase 1: Automated Peer Review (POC)
Implement a GitHub Action that triggers the `code-reviewer` Claude Code sub-agent whenever a Pull Request is opened.
- **Workflow:** `workflows/auto-reviewer.yml`
- **Agent:** `swarm-agents/claude-code-agents/code-reviewer.md`
- **Output:** A structured, adversarial review posted as a PR comment.

## Phase 2: The Self-Healing Fixer
Implement a GitHub Action that listens for "Requested Changes" from our Code Reviewer.
- **Trigger:** Comment posted with verdict `Request Changes`.
- **Agent:** `fixer-agent`
- **Action:** Spawns a Claude Code session to read the review comments, apply the requested fixes, and push a new commit to the PR branch.

## Phase 3: Auto-Merge & Mission Control
Implement the final handshake.
- **Trigger:** CI passes AND Code Review verdict is `Approve`.
- **Action:** Auto-merge the PR into `main`.
- **Daily Mission Control:** A heartbeat task that summarizes the Swarm's activity and highlights any blockers for the human human (Yusong).

## Security & Guardrails
- Agents run in isolated GitHub Action runners.
- Reviewer agent is strictly read-only.
- Fixer agent is limited to the current branch.
- Critical secrets (API Keys) are managed via GitHub Secrets.
