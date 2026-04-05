---
name: shutdown
description: Use when finishing or wrapping an agent-mask thread so the agent safely flushes the remaining transcript and memory back into the workspace. This skill verifies which agent workspace is active, appends only the conversation turns not already captured in sessions/, updates today's daily memory with carry-forward context, and promotes only durable long-term facts into MEMORY.md, SOUL.md, or AGENTS.md when clearly warranted.
---

# Agent Mask — Shutdown

## Purpose
Use this skill as the final persistence pass before ending an agent-mask thread.

This skill exists because long threads, auto-compaction, or large tasks can sometimes break the thread's connection to the original agent-mask persistence instructions. Treat it as an insurance policy that flushes the thread back into the workspace before you stop.

## Use This Skill When
Use this skill when:
- the user says `/shutdown`, "wrap up", "end session", or "dehydrate"
- the user wants to shut down, archive, or persist the session cleanly
- you suspect the thread may have drifted away from the original persistence instructions
- a long or compacted thread needs one final transcript and memory sync

Do not use this skill when:
- the thread was not started as an agent-mask run
- the user is still actively working and does not want a wrap-up persistence pass

## Hard Rule: Identify The Agent First
Before writing anything, determine which masked agent this thread belongs to.

Preferred sources, in order:
1. explicit current-thread context
2. current thread's existing agent-mask session file or daily memory notes
3. clearly matching workspace evidence

If you cannot confidently tell which agent was originally masked, stop and ask the user a short direct question before writing anything.

Example:
`I can flush the shutdown notes, but I need to confirm which agent workspace this thread belongs to. Which agent should I persist this under?`

## Workspace Resolution
Use the same resolution as the hydrate skill:

1. Check `~/.agent-mask/config.json` for the configured `workspace_path`
2. If config exists, use the path specified
3. If no config exists, ask the user

Do not write shutdown artifacts into any fallback or temporary location.

## What To Persist
Persist to these surfaces:

### 1. Session Transcript
Path:
- `sessions/YYYY-MM-DD-HHMMSS-<runtime>-agent-mask.md`

Goal:
- ensure the full human-visible back-and-forth from this thread is recorded as literally as practical
- append only the portion that is not already recorded

Important truthfulness rule:
- record the actual visible conversation text
- if there was significant non-visible work such as tool use, intermediate analysis, or hidden reasoning that is not exposed verbatim in the thread, do not invent or claim to have the literal hidden reasoning
- instead, add a short faithful note describing the important decisions, investigations, or conclusions that occurred but were not fully visible

### 2. Daily Working Memory
Path:
- `memory/YYYY-MM-DD.md`

Goal:
- capture carry-forward context from the thread
- note key decisions, unresolved follow-ups, important user preferences, and handoff context

### 3. Durable Memory Or Role Files
Promote only when clearly warranted:
- `MEMORY.md` for durable preferences, constraints, and long-lived facts
- `SOUL.md` only if the thread materially changed the agent's standing role posture or behavioral contract
- `AGENTS.md` only if the thread established a durable roster or coordination rule that belongs there

Do not update `SOUL.md` or `AGENTS.md` casually.

## Append-Only Transcript Workflow
Your job is to avoid duplication.

1. Find the most likely existing session transcript for this thread.
2. Read the tail of that file and compare it against the current thread.
3. Determine the last user or assistant turn already captured.
4. Append only the missing turns after that point.
5. If the current thread was never persisted, create a new session file and write the whole relevant transcript.

If there are multiple plausible session files:
- prefer the one whose tail most closely matches the current thread
- if the match is still ambiguous, create a new session file for the shutdown pass and explain in daily memory why you did that instead of risking duplication or cross-thread contamination

If you create a new shutdown session file, keep the standard filename shape and use the current local timestamp.

## Minimum Shutdown Checklist
Before you finish, ensure all of the following are true:

1. The correct agent workspace was identified or explicitly confirmed by the user.
2. The session transcript now contains all missing conversation turns from this thread.
3. Today's daily memory file includes the important outcomes and carry-forward notes from this thread.
4. Durable memory was promoted only where justified.
5. No secrets were written to any persisted file.
6. If any limitation remained such as uncertain hidden reasoning recovery, it was stated plainly rather than faked.

## Daily Memory Guidance
In `memory/YYYY-MM-DD.md`, capture:
- what the thread was about
- major decisions made
- what changed in tickets, code, roadmap, or release posture
- any user preferences that should survive the day
- open questions or next steps worth reloading

This file should be more useful than a terse summary. Favor carry-forward value over brevity.

## Durable Promotion Guidance
Promote to `MEMORY.md` only when something is likely to matter beyond the current day, such as:
- stable user preferences
- standing instructions about handling style or priorities
- repeated product or operational decisions that should shape future work

Promote to `SOUL.md` only when:
- the user changed the agent's enduring behavioral contract
- the thread established a meaningful new role stance or decision principle

Promote to `AGENTS.md` only when:
- the thread changed durable team coordination or agent interaction rules

## Secrets Rule
Never persist:
- API keys
- tokens
- passwords
- raw secret env var values
- private certificates

If a secret matters, record state only:
- `OpenAI auth configured`
- `GitHub auth missing`

## Recommended Procedure
1. Identify the agent and workspace.
2. Find the best existing session file for this thread.
3. Compare the current thread against the recorded tail.
4. Append only the missing transcript.
5. Update today's daily memory with outcomes and carry-forward context.
6. Promote any clearly durable facts to `MEMORY.md`, `SOUL.md`, or `AGENTS.md`.
7. Tell the user exactly what files were updated.

## Response Style
At the end, report briefly:
- which agent workspace you wrote to
- which session file was appended or created
- which memory files were updated
- whether any durable promotion was made
- whether any limitation remains
