---
name: usage-cost-optimizer
description: Choose the cheapest process that keeps quality - model, effort, subagents, tool calls, CI/deploy loops. Use when planning any non-trivial task, before spawning agents or workflows, before expensive operations (large builds, full test suites, mass file changes, web research), or when the user asks about usage, limits or cost.
---

# Usage & Cost Optimizer

## Match effort to task
| Task | Approach |
|---|---|
| Lookup, rename, one-file fix | Do it inline; no subagents; minimal tool calls |
| Multi-file feature / bug hunt | Plan briefly, work in stages, verify each stage |
| Broad search across repo | One `Explore` subagent, not many manual reads |
| Truly parallel independent work | Several subagents in ONE message; only when it saves real time |
| Huge orchestration (Workflow tool) | Only if the user explicitly opts in |

Model choice: default session model for engineering; suggest a smaller/faster model (e.g. Haiku/Sonnet via `/model`) for bulk mechanical edits or simple Q&A; the most capable model for architecture, security, hard debugging. Don't change model/effort on your own — recommend it.

## Tool-call hygiene
- Batch independent calls in one message (parallel reads/greps/commands).
- Don't verify what a tool already confirmed (Edit errors if it fails).
- Run the narrowest test first (single file/test), full suite once at the end.
- Web: `standard` search first; `extended` only when results are thin.
- Don't poll; use background runs + notifications. Don't install packages already present (`npm ls`, `pip show`, `which`).

## Detect costly operations — flag and propose cheaper option
Full rebuilds in loops, re-running CI to "retry", reading whole large files, scanning `node_modules`, many sequential single-file reads, re-fetching the same URL, spawning agents for 2–3 tool calls.

## Reporting usage
Claude Code commands (when available in the client): `/cost` (session tokens/cost), `/usage` or `/status` (plan limits), `/context` (context window breakdown). Report them when asked; never invent numbers.
