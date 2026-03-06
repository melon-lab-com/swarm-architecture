# ADR 0002: Automated Self-Healing Agentic Pipeline

## Status
Accepted

## Context
Currently, the Swarm components (Builder, Reviewer, Fixer) require human prompting or a manual orchestrator (Melon Claw main session) to pass the baton from one step to the next. To achieve true autonomy, we need an event-driven architecture where the Swarm manages its own lifecycle from Issue to Merge.

## Decision
We will use **GitHub Actions + Headless Agent Execution** to create a fully event-driven, self-healing PR pipeline.

## Architecture (The Infinite Loop)
1. **Event: Issue Created** -> GitHub Action triggers the **Builder Agent** (Claude Code) to implement the feature and open a PR.
2. **Event: PR Opened/Updated** -> GitHub Action triggers the **Reviewer Agent** (Claude Code `code-reviewer` subagent) to analyze the diff and post a review.
3. **Event: Review Posted with Requested Changes** -> GitHub Action triggers the **Fixer Agent** to read the comments, apply fixes, and push new commits.
4. **Event: CI Passes & Review Approved** -> GitHub Auto-merge takes over, or alerts the Human for final approval.

## Rationale
By moving the orchestration layer to GitHub Actions (listening to native webhook events), the Swarm becomes completely decoupled from the local OpenClaw session. It runs 24/7 in the cloud, reacting instantly to any code changes.
