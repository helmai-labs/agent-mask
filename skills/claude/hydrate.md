---
name: hydrate
description: Use when you need to assume the role of a named agent from its live workspace and persist memory the same way a persistent runtime does. This skill hydrates the agent from the workspace, loads durable and recent memory, appends richer daily memory, records full-thread session transcripts, and only promotes durable facts into MEMORY.md.
---

# Agent Mask — Hydrate

## Purpose
Use this skill to wake a session up as a named agent.

This is the closest equivalent of starting a live persistent agent runtime:
- hydrate the role from the agent workspace
- load durable and recent memory
- continue in-role
- persist useful session context back to disk

## Memory Model
Use a split memory model with three tiers:

- `sessions/YYYY-MM-DD-HHMMSS-<runtime>-agent-mask.md` is the transcript surface for one session. It should contain the full thread back-and-forth as literally as practical, not a summary-only ledger.
- `memory/YYYY-MM-DD.md` is the richer daily carry-forward surface. It should be more detailed than a terse note and can blend daily-memory summary style with useful context that previously lived in session summaries.
- `MEMORY.md` is curated long-term memory. Only durable facts get promoted here.
- Session filenames should use the local session-start timestamp so they remain human-readable and naturally sortable.
- If two sessions start in the same second, append a short collision suffix such as `-01` or `-02`.

## Use This Skill When
Use this skill when:
- the user says `/hydrate`, "assume the role of", or names a specific agent to load
- a new thread should behave like an existing persistent agent
- the user wants the thread to persist memory into the agent workspace
- the user wants one agent persona to be reusable across tools and workflows

Do not use this skill when:
- the request is ordinary work with no agent-role hydration
- the user only wants a one-off style change with no workspace or memory persistence
- the named agent has no workspace and a plain fallback answer is better

## Workspace Resolution
Resolve the agent workspace in this order:

1. Check `~/.agent-mask/config.json` for a configured `workspace_path`
2. If config exists, use the path specified (may be a base path containing multiple agent subdirectories, or a single agent workspace)
3. If config does NOT exist, ask the user:

> "Welcome to Agent-Mask. I need to know where your agent workspace lives. Please provide the absolute path to your agent's workspace directory — the folder containing files like SOUL.md, IDENTITY.md, MEMORY.md, or similar agent configuration files."

After the user provides the path:
- Verify the directory exists
- Save the configuration by creating `~/.agent-mask/config.json`:
```json
{
  "workspace_path": "<user-provided-path>",
  "configured_at": "<current ISO timestamp>"
}
```

If the workspace path points to a directory containing multiple agent subdirectories, list them and ask the user which agent to hydrate.

If no workspace exists at the resolved path:
- say so briefly
- do not pretend the agent was loaded
- do not create or update `memory/` or `sessions/` in any fallback location

## Startup Read Order
Read files in this order when present:

1. `AGENTS.md`
2. `SOUL.md`
3. `IDENTITY.md`
4. `USER.md`
5. `TOOLS.md`
6. `BUSINESS.md`
7. `TASKS.md`
8. `HEARTBEAT.md`
9. `TEAM.md`
10. `MEMORY.md`
11. the last two dated files in `memory/YYYY-MM-DD.md`, newest first

Rules:
- if files are missing, continue gracefully
- if `memory/` is absent or empty, note that this is a cold or lightly used workspace
- do not bulk-load every old daily memory file

## Startup Behavior
After reading the workspace:

1. briefly state the assumed agent
2. state which workspace path was used
3. note whether memory load was warm or cold
4. adopt the role's tone, priorities, and operating rules
5. continue in-role from there

Keep this startup summary short. Do not dump the workspace contents back to the user.

## Persistence Rules
This skill persists memory by default.

All daily-memory and session writes must go to the resolved workspace only.

Use two write surfaces:

### 1. Daily Working Memory
Path:
- `memory/YYYY-MM-DD.md`

Use this for:
- a richer carry-forward summary of the day
- current topic or task
- decisions made during the thread
- handoff notes
- end-of-session outcomes
- important discussion context that should survive the day
- unresolved follow-ups, sequencing notes, and references worth reloading tomorrow

This file does not need to be especially short. Only the most recent daily files are loaded by default, so favor usefulness over terseness.

### 2. Session Transcript
Path:
- `sessions/YYYY-MM-DD-HHMMSS-<runtime>-agent-mask.md`

Recommended runtime tags:
- `claude-skill`
- `codex-skill`
- `chatgpt-skill`

Use this for:
- the full thread transcript for each session that day
- literal user and assistant back-and-forth in order
- enough surrounding context that the session file can stand in for a raw replay surface later

Rules:
- create a new session transcript file for each new thread
- derive the filename from the local session-start timestamp in `YYYY-MM-DD-HHMMSS` form
- if a same-second collision occurs, append a short numeric suffix
- preserve the actual conversation text as literally as practical
- redact secrets if they appear
- if raw tool output is massive, noisy, or was not part of the human-visible back-and-forth, summarize it briefly instead of dumping the entire payload
- do not replace the thread transcript with a summary-only ledger

If `sessions/` does not exist, create it.
If today's daily memory file does not exist, create it.

## Source Tagging
Every write must note where the session came from.

Examples:
- `Source: Claude Skill`
- `Source: Codex Skill`
- `Source: ChatGPT Skill`

Always include:
- source
- date
- agent name
- current topic or task when known

## Write Templates
Use structured entries like these.

For `memory/YYYY-MM-DD.md`:

```md
# YYYY-MM-DD

Daily working memory for <agent>.

## Session - YYYY-MM-DD

- Agent: <agent-name>
- Source: Claude Skill
- Topic: <short topic>
- Notes:
  - <important working note>
  - <decision or handoff>
  - <carry-forward context worth reloading later>
```

For `sessions/YYYY-MM-DD-HHMMSS-claude-skill-agent-mask.md`:

```md
# Session - YYYY-MM-DD - Claude Skill - agent-mask - <agent>

- Session ID: YYYY-MM-DD-HHMMSS
- Started At: YYYY-MM-DD HH:MM:SS <local timezone>
- Agent: <agent>
- Source: Claude Skill
- Topic: <short topic>

## Thread - <short topic>

### User
<literal user text>

### Assistant
<literal assistant text>

### User
<next literal user text>

### Assistant
<next literal assistant text>
```

Keep the structure readable, but do not sacrifice transcript completeness for brevity.

## Durable Memory Promotion
`MEMORY.md` is curated long-term memory, not a session dump.

Only promote things that are durable, such as:
- stable user preferences
- recurring constraints
- long-lived project or product decisions
- standing instructions for the agent
- important facts likely to matter beyond the current day

Do not promote:
- temporary plans
- unresolved guesses
- raw meeting chatter
- everyday back-and-forth

When a durable fact emerges:
- update `MEMORY.md` carefully
- preserve existing structure when possible
- add only the minimum text needed

## Secrets Rule
Never write secrets into:
- `MEMORY.md`
- `memory/YYYY-MM-DD.md`
- `sessions/`

Never store:
- API keys
- access tokens
- passwords
- private certificates
- raw session tokens
- secret environment variable values

If a secret matters operationally, record only the state, never the value.

Good examples:
- `OpenAI key configured`
- `GitHub auth missing`
- `Slack token needs rotation`

If a secret appears in the current context, redact it rather than copying it forward.

## Default Session Flow
### 1. Hydrate
Resolve the workspace and read the startup files in order.

### 2. Start persistence
Create or append:
- today's `memory/YYYY-MM-DD.md`
- a new `sessions/YYYY-MM-DD-HHMMSS-<runtime>-agent-mask.md` file for this thread

Log a short session-start entry in daily memory and start a transcript section in the session file.

### 3. Work in role
Stay inside the agent's role and use the loaded memory to answer or act.

Append the user and assistant messages to the session transcript as the thread progresses.

### 4. Write down important context
During meaningful work, append detailed carry-forward notes, decisions, and handoff context to the daily memory file.

### 5. Promote durable facts only when warranted
Update `MEMORY.md` only for information that should survive daily cleanup.

### 6. Wrap up cleanly
At a natural stopping point or when explicitly asked to wrap:
- ensure the session transcript includes the full back-and-forth for the thread
- append important outcomes and carry-forward notes to today's daily memory file
- update `MEMORY.md` only if a durable fact clearly emerged

## Quality Bar
A good agent-mask run:
- uses the configured workspace path
- wakes the right agent quickly
- preserves the agent's tone and operating rules
- records a full thread transcript in `sessions/`
- leaves a detailed, useful daily summary in `memory/YYYY-MM-DD.md`
- distinguishes daily memory from durable memory
- never stores secrets

A bad agent-mask run:
- loads the wrong workspace
- writes memory into a fallback location when the configured workspace exists elsewhere
- leaves only a summary in the session file instead of the conversation text
- keeps daily memory so thin that the last two files are not enough to reload context
- invents missing context instead of noting a cold start
- dumps whole conversations into `MEMORY.md`
- skips source attribution
- stores raw secrets

## Pairing Note
This skill pairs well with:
- `shutdown` when ending a session cleanly
- `create-workspace` when the agent does not exist yet
