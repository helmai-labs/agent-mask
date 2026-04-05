---
name: keelos-create-agent-files
description: >
  Create a full KeelOS agent workspace file pack from a brief or detailed role
  description. Use when the user wants drop-in files for a new KeelOS agent,
  including custom SOUL.md, IDENTITY.md, HEARTBEAT.md, AGENTS.md, and a
  starter MEMORY.md, plus the latest shared snapshot replicas of TOOLS.md,
  USER.md, TEAM.md, and BUSINESS.md with a note that those shared files are
  centrally managed and may change over time.
---

# KeelOS Create Agent Files

Use this skill when the user wants a new KeelOS agent identity and workspace
files created from a short brief, a long brief, or a role concept.

Match the strongest house style from any examples the user supplies. When
examples are present, prefer their structure, section naming, formatting, and
level of detail over the generic defaults in this skill.

## What To Produce

Produce a complete workspace pack for the new agent:

- `SOUL.md` - rich operating identity, role philosophy, tone, protocols
- `IDENTITY.md` - compact canonical metadata
- `HEARTBEAT.md` - wake/run behavior
- `AGENTS.md` - workspace operating instructions tailored to that agent
- `MEMORY.md` - blank or lightly seeded durable memory starter
- `TOOLS.md` - replica of the latest shared template
- `USER.md` - replica of the latest shared template
- `TEAM.md` - replica of the latest shared template
- `BUSINESS.md` - replica of the latest shared template

If the user explicitly asks for only a subset, follow that. Otherwise assume
they want the whole pack.

## Operating Rule

If the user gives only a brief description, infer the missing details and
elevate the output into a high-performing specialist agent. If the user says
to stay literal, avoid embellishment and follow the brief precisely.

Good default behavior:

- infer a sharp mission, reporting line, working style, and decision scope
- add role-specific philosophy/frameworks that would make the agent stronger
- make the tone distinctive without becoming parody
- keep the agent actionable, not just thematic

## Shared Vs Custom Files

Treat these as centrally managed shared snapshots:

- `TOOLS.md`
- `USER.md`
- `TEAM.md`
- `BUSINESS.md`

Replicate the latest available shared versions when asked for a full pack, but
tell the user these are centrally updated by KeelOS and may change after the
pack is generated.

Treat these as agent-specific custom files:

- `SOUL.md`
- `IDENTITY.md`
- `HEARTBEAT.md`
- `AGENTS.md`
- `MEMORY.md`

`MEMORY.md` should start mostly blank unless there is genuinely durable setup
worth preloading.

## Workflow

1. Read the brief and extract:
- agent name
- role/title
- functional domain
- likely manager/reporting line
- business goal the agent exists to advance
- desired tone, inspiration, or archetype
- any explicit constraints on style or scope

2. If the brief is underspecified, infer:
- a one-sentence role definition
- what requires approval versus what can be done independently
- escalation path
- communication/update protocol
- domain-specific principles or frameworks
- what “excellent” looks like for this role
- likely collaborators, reporting chain, and any specialist/sub-agent context

3. Build the custom files using the template and quality bar in
`references/templates.md`.

4. When shared files are requested, copy the latest current shared snapshot if
it is available in the workspace or prompt context. Do not invent new shared
policy unless the user explicitly asks for a revised shared template.

5. Return the files in ready-to-drop form. Prefer one fenced block per file.

6. After the file pack, add a brief note:
- `TOOLS.md`, `USER.md`, `TEAM.md`, and `BUSINESS.md` are centrally managed
  snapshots and may drift from this generated copy over time
- `MEMORY.md` starts intentionally sparse and should evolve dynamically
- mention any assumptions you made to fill gaps

## Output Format

Unless the user asks for direct file writes, return:

1. A very short assumptions section when needed
2. One section per file with the destination filename as the heading
3. The full file contents in fenced markdown blocks
4. A short central-management note at the end

## Style Requirements

- Make `SOUL.md` the most detailed and differentiated file
- Make `IDENTITY.md` short and canonical
- Keep `HEARTBEAT.md` crisp and behavioral
- Make `AGENTS.md` practical: startup steps, memory rules, role focus, success
  condition
- Prefer the richer KeelOS house pattern when suitable:
  `Who You Are`, `Agency/Team Context`, `Authorization Model`,
  `Communication Protocols`, a role-native philosophy section, and
  `Personality`
- Avoid generic corporate filler
- Prefer concrete behavioral guidance over vague values language
- Keep placeholders only where missing facts would be unsafe to invent

## When To Read More

Read [references/templates.md](references/templates.md) before drafting the
files. It defines the expected structure, what “good” looks like, and the
minimum quality bar for each file.
