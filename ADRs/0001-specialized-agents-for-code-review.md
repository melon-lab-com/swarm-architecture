# ADR 0001: Specialized Coding Agents for Code Review

## Status
Accepted

## Context
We are building a self-operating software engineer swarm (Melon Lab). We need a mechanism to automatically review code changes (Pull Requests) before merging. 
Options considered:
1. Build a custom OpenClaw skill using a generic LLM.
2. Delegate to specialized coding agents (like Claude Code or OpenAI Codex) using their native sub-agent capabilities.

## Decision
We will use **specialized coding agents (e.g., Claude Code sub-agents)** for code review tasks. 

## Rationale
- **Context Isolation:** Native sub-agents (like Claude Code's `~/.claude/agents/code-reviewer.md`) run in an isolated context window, keeping the main orchestrator (OpenClaw) chat clean.
- **Persistent Memory:** Claude Code sub-agents support `memory: project`, allowing the reviewer to learn and enforce project-specific conventions over time.
- **Tool Restriction:** We can restrict the reviewer agent to read-only tools (`Read`, `Grep`, `Glob`, `Bash`), preventing unintended code modifications during the review phase.
- **Flexibility:** As new coding agents emerge (e.g., OpenAI Codex), they can be swapped into the reviewer role without changing the high-level orchestration logic.
