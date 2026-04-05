# Agent-Mask

**Hydrate and use your agents anywhere, any tool, any time.**

Agent-Mask is a skill pack that lets you carry your AI agent's persistent memory, identity, and traits across any tool or workspace.

## Why Agent-Mask?

LLM tools store your agent's memory in opaque temp files — proprietary JSONL blobs, vendor-specific databases, formats only one tool can read. Auto-compaction silently rewrites your context mid-session. Session files get pruned on schedules you don't control. Your agent's entire history exists at the mercy of a cleanup process you can't even find.

Agent-Mask puts you in control. Every session transcript, every daily memory file, every long-term fact your agent learns is a plain markdown file in a folder on your machine. Human-readable, version-controllable, portable across any tool. No one else gets to decide what your agent remembers.

## Skills

| Skill | Description |
|-------|-------------|
| `/hydrate` | Load an agent's identity, memory, and traits into the current session |
| `/shutdown` | Gracefully end a session, persisting any state changes |
| `/create-workspace` | Scaffold a new agent workspace at a target location |

## How it works

Your agent lives in a folder. Plain markdown files — SOUL.md, IDENTITY.md, MEMORY.md, session transcripts, daily working memory. No databases, no proprietary formats, no vendor lock-in.

Run `/hydrate` in Claude Code, ChatGPT, or Codex and your agent wakes up with its full identity, memory, and behavior intact. Run `/shutdown` and everything gets persisted back to disk. Switch tools tomorrow and pick up where you left off.

## Supported Platforms

- **Claude Code** (Anthropic) — `skills/claude/`
- **ChatGPT / Codex** (OpenAI) — `skills/codex/`

## Quick Start

1. Copy the skill files for your platform into your tool's skills directory
2. Run `/hydrate` and point it at your agent workspace
3. Your agent is live

No account needed. No runtime. No auth. Just markdown files.

## License

MIT
