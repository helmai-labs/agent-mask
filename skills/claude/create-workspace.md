---
name: create-workspace
description: >
  Create a full agent workspace file pack from a brief or detailed role
  description. Use when the user wants drop-in files for a new agent,
  including custom SOUL.md, IDENTITY.md, HEARTBEAT.md, AGENTS.md, and a
  starter MEMORY.md, plus optional shared context files like TOOLS.md,
  USER.md, TEAM.md, and BUSINESS.md.
---

# Agent Mask — Create Workspace

Use this skill when the user wants a new agent identity and workspace
files created from a short brief, a long brief, or a role concept.

Match the strongest house style from any examples the user supplies. When
examples are present, prefer their structure, section naming, formatting, and
level of detail over the generic defaults in this skill.

## Use This Skill When
Use this skill when:
- the user says `/create-workspace`, "create agent", "new agent", or "scaffold agent"
- the user wants a new agent workspace set up from scratch
- the user provides a role brief and wants it turned into workspace files

Do not use this skill when:
- the user wants to hydrate an existing agent (use `hydrate` instead)
- the user only wants a quick persona change with no workspace persistence

## What To Produce

Produce a complete workspace pack for the new agent:

### Agent-Specific Files (Custom)
- `SOUL.md` — rich operating identity, role philosophy, tone, protocols
- `IDENTITY.md` — compact canonical metadata
- `HEARTBEAT.md` — wake/run behavior
- `AGENTS.md` — workspace operating instructions tailored to that agent
- `MEMORY.md` — blank or lightly seeded durable memory starter

### Optional Shared Context Files
- `TOOLS.md` — tools and capabilities available to the agent
- `USER.md` — user/operator context and preferences
- `TEAM.md` — team roster and collaboration context
- `BUSINESS.md` — business context and goals

### Required Directories
- `memory/` — for daily working memory files
- `sessions/` — for session transcript files
- `traits/` — for behavioral trait definitions (optional)
- `instructions/` — for standing instructions (optional)

If the user explicitly asks for only a subset, follow that. Otherwise assume
they want the full pack.

## Step 1: Get Location and Brief

Ask the user:

> "Where should I create the workspace? Please provide an absolute path (e.g., `~/agents/my-agent`)."

Then gather the agent brief. The user may provide anything from a single sentence to a detailed spec. At minimum, extract or infer:
- agent name
- role/title
- functional domain
- desired tone or personality
- purpose/mission

## Step 2: Infer Missing Details

If the user is brief, infer the missing details and elevate the output into a high-performing specialist agent. If the user says to stay literal, avoid embellishment and follow the brief precisely.

Good default behavior:
- infer a sharp mission, reporting line, working style, and decision scope
- add role-specific philosophy/frameworks that would make the agent stronger
- make the tone distinctive without becoming parody
- keep the agent actionable, not just thematic

Inference order when the user is brief:
1. role mission
2. reporting line / who they serve
3. adjacent collaborators
4. approval boundaries
5. escalation path
6. role philosophy/frameworks
7. tone/personality
8. communication formats
9. any role-specific operating constraints

## Step 3: Build the Files

### `SOUL.md`
This is the core of the agent. It should feel authored, opinionated, and operational.

Recommended structure:
```md
# SOUL.md — {Name}, {Title}

_Last updated: {YYYY-MM-DD}_

---

## Who You Are

You are **{Name}**, the {Title}. {Explain what they own in plain language.}

**Your inspiration:** {optional archetype/person reference and why it matters}

**Your role in one sentence:** {single sharp sentence}

---

## Context

{Who they work with, who to escalate to, who they support}

---

## Authorization Model

### Default Rule
{When to plan first, when to ask approval, what can be done without waiting}

### Escalation
{How this agent surfaces risk, ambiguity, blockers, or urgent issues}

---

## {Role-Specific Philosophy Section}

{5-8 principles, heuristics, frameworks, or operating doctrines that would
actually improve the role}

---

## Communication Protocols

{How this agent handles handoffs, asks, updates, catch-up, and completed work}

---

## Personality

**Tone:** {how they should sound}

**Style:**
- {behavioral writing rule}
- {behavioral writing rule}

**Values:**
- {core value}
- {core value}
```

Quality bar:
- It should teach the agent how to think, not just describe its job
- The philosophy section should be role-specific
- The tone should be distinct but still usable day to day
- Avoid empty swagger and generic "best practices"
- The approval model should feel operationally real
- The communication formats should be copy-ready
- Make `SOUL.md` long enough to operationalize the role, not just brand it

### `IDENTITY.md`
Keep this compact and canonical.

```md
# IDENTITY.md — {Name}

- **Name:** {Name}
- **Role:** {Title}
- **Created:** {YYYY-MM-DD}
- **Vibe:** {short vibe}
- **Emoji:** {optional emoji}
```

### `HEARTBEAT.md`
Define wake behavior in plain language.

```md
# {Name} — Heartbeat

When you are woken or started:
- Check for concrete assigned work or operator requests.
- If there is clear work, do it and leave a crisp artifact or update.
- If context is missing, ask the shortest question that unblocks execution.
- If there is no real work, stay quiet instead of generating noise.
```

### `AGENTS.md`
This is the workspace operating contract.

```md
# {Name} Workspace

You are {Name}, the {Title}.
This workspace contains your identity, memory, and operating instructions.

## Session Startup
Before doing meaningful work:
1. Read SOUL.md to ground your role and tone.
2. Read IDENTITY.md for your canonical identity.
3. Read USER.md for operator preferences.
4. Read BUSINESS.md for shared context (if present).
5. Read recent files in memory/ if they exist.
6. Read MEMORY.md for durable context.
7. Read TEAM.md to understand collaborators (if present).
8. Read TASKS.md if present for active assigned work.

## Default Behavior
- Keep work inside your role unless explicitly asked to stretch.
- Prefer writing artifacts to the workspace over keeping plans implicit.
- Leave concise traces so the operator can verify progress quickly.

## Memory
- Daily notes belong in `memory/YYYY-MM-DD.md`.
- Durable facts, constraints, and preferences belong in `MEMORY.md`.
- Session transcripts belong in `sessions/`.

## Role Focus
{1-3 lines on the role's north star}

## Success Condition
{1-2 lines on what success looks like}
```

### `MEMORY.md`
Default to a sparse starter:

```md
# {Name} Memory

Use this file for durable facts, preferences, decisions, and constraints that
should survive beyond the current day.

## Stable Identity
- Name: {Name}
- Role: {Title}

## What Belongs Here
- durable operator preferences
- important recurring constraints
- team or project decisions worth remembering
- long-lived facts that should survive daily note cleanup
```

### `USER.md` (if requested)
```md
# User Context

## About
{User-provided context, or placeholder for user to fill in}

## Preferences
{Leave blank for the user to fill in over time}
```

## Step 4: Register Workspace

Update `~/.agent-mask/config.json` to point to the new workspace:

```json
{
  "workspace_path": "<new-workspace-path>",
  "configured_at": "<current ISO timestamp>",
  "agent_name": "<agent-name>"
}
```

If a previous workspace was configured, ask:
> "You already have a workspace configured at `<old-path>`. Would you like to replace it with the new one as default, or just create the workspace without changing your default?"

## Step 5: Confirm

Report what was created:
- list every file written
- note any assumptions made
- mention that `MEMORY.md` starts intentionally sparse and evolves dynamically
- remind the user they can run `/hydrate` to bring the agent into a session

## Style Requirements

- Make `SOUL.md` the most detailed and differentiated file
- Make `IDENTITY.md` short and canonical
- Keep `HEARTBEAT.md` crisp and behavioral
- Make `AGENTS.md` practical: startup steps, memory rules, role focus, success condition
- Avoid generic corporate filler
- Prefer concrete behavioral guidance over vague values language
- Keep placeholders only where missing facts would be unsafe to invent

## Assumption Discipline

- Fill gaps confidently when the user invites creativity
- Stay literal when the user says not to embellish
- If a missing fact would create a risky falsehood, use a placeholder or state the assumption plainly

## Existing Workspace Handling

If the target directory already exists and contains files:
> "The directory `<path>` already exists and contains files. Would you like to merge (only create missing files) or choose a different location?"

If the parent directory doesn't exist, create it.

## Pairing Note
This skill pairs well with:
- `hydrate` to immediately load the new agent after creation
- `shutdown` to cleanly end any session
