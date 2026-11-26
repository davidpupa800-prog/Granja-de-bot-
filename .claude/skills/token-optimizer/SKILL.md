---
name: token-optimizer
description: Keep context and output lean. Apply on every task, and especially for long sessions, large files, big logs, multi-repo work, or when context is getting full.
---

# Token Optimizer

## Reading
- Search before reading: `Grep`/`Glob` (or `rg`) to locate, then read only the needed line range (`offset`/`limit`, `sed -n`).
- Never re-read a file you just edited or already have in context.
- Pipe noisy commands: `| tail -50`, `| head`, `grep -E 'error|fail'`, `--quiet`, `-q`. Never dump lockfiles, minified bundles, binaries or media.
- Broad exploration across many files → delegate to an `Explore` subagent; keep only its conclusion.

## Writing
- Use `Edit` with minimal unique `old_string`; don't rewrite whole files for small changes.
- Reuse existing components/utilities; grep for them before creating new ones.
- No duplicate files (`*-v2`, `*-copy`, `*-new`).

## Answering
- Lead with the result. No restating the request, no narrating options you won't take, no repeating content already shown.
- Code in replies only when the user needs to see it; otherwise reference `path:line`.
- Compact bullet lists over prose; tables for comparisons.

## Prompts & context
- Instructions compact: CLAUDE.md under ~60 lines; details live in skills (loaded on demand) instead of always-loaded files.
- For subagents, write a tight brief (goal, files, what's ruled out) instead of pasting large content.
- In long sessions, summarize decisions into memory (see `advanced-memory`) so later turns and sessions don't need to rediscover them. Use `/compact` with a focus hint when context is heavy; `/clear` between unrelated tasks.
