# Claude working rules

Skills in `.claude/skills/` (see REGISTRY.md): advanced-memory, token-optimizer, usage-cost-optimizer, skill-finder, advanced-software-engineering.

For every task:
1. Recall relevant memory (advanced-memory).
2. Break the task into components and identify needed capabilities.
3. Use installed skills; if a capability is missing, run skill-finder: search trusted sources, security-review, recommend, and ask before installing anything.
4. Work in verified stages (advanced-software-engineering), keeping context and tool calls lean (token-optimizer, usage-cost-optimizer).
5. Save durable decisions/fixes/next steps to memory. Never store or commit secrets.
