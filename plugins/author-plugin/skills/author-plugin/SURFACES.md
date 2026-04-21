# SURFACES.md — decision guide

Authoritative guide for which Claude Code surface a requirement needs. Skills are the default. Everything else is earned.

## Quick decision table

| Signal in the requirement | Surface |
|---|---|
| User invokes it by name, writes prose, asks for help | **Skill** (default) |
| Work benefits from fresh context (review, audit) | **Subagent** |
| Work needs a strictly limited toolset | **Subagent** |
| Multiple independent spokes run in parallel | **Subagent** (per spoke) |
| Literal `/name args` UX matters (user types the command itself) | **Slash command** |
| Runs reactively on an event (file save, tool call, prompt submit) | **Hook** |
| Bridges an external system over stdio JSON-RPC | **MCP server** |
| Language intelligence (symbols, rename, refs) | **LSP server** |
| Static reference content over ~200 lines | **Uppercase .md** reference doc |

If two surfaces could work, default to the simpler one. A skill that calls a subagent is simpler than a subagent that calls a skill.

---

## Skill (default)

**Use when** the user triggers the capability by asking for it in conversation, and the plugin is mostly prose + workflow steps. Skills can load reference `.md` files lazily.

**Layout**
```
skills/<name>/
├── SKILL.md           # frontmatter + workflow, <500 words
├── PATTERNS.md        # optional reference
├── REFERENCE.md       # optional reference
└── ...
```

**Frontmatter**
```yaml
---
name: <kebab-case>
description: Use when <third-person triggers>...
allowed-tools:
  - Read
  - Write
  - ...
---
```

**Gotchas**
- The `description` is the trigger. Treat it like an index entry, not a summary.
- Do not `@`-link reference files — that force-loads them. Use plain markdown links so they load on demand.
- Keep SKILL.md under 500 words. Heavy content goes to uppercase `.md` siblings.

---

## Subagent

**Use when** (a) fresh context produces a better answer (review, unbiased audit), (b) a restricted toolset prevents a class of mistake, or (c) you need parallel independent spokes.

**Layout**
```
agents/<name>.md
```

**Frontmatter**
```yaml
---
name: <kebab-case>
description: <triggers + what it returns>
tools: Bash, Read, Grep, Glob       # comma-separated, not a YAML list
model: inherit                       # or sonnet / haiku / opus
color: purple
---
```

**Forbidden fields in plugin agents**: `hooks`, `mcpServers`, `permissionMode`. Claude Code rejects plugin agents that declare any of these. Put those settings in the harness (`.claude/settings.json`), not the agent.

**Gotchas**
- `tools` is a comma-separated string, not a YAML list.
- `model: inherit` is the right default for orchestrated sub-agents; name the model when cost matters (Haiku for cheap gates, Sonnet for serious work).
- Keep the body in plain second-person ("You are …") — matches the house style.

---

## Slash command

**Use when** the user types the literal command themselves and the argument shape matters. Prefer a skill if the trigger is conversational ("can you review this?").

**Layout**
```
commands/<name>.md
```

The body is the instruction text; the filename is the command.

**Gotchas**
- No frontmatter beyond optional `description`.
- If you find yourself adding lots of supporting files, you actually want a skill.

---

## Hook

**Use when** behaviour must fire reactively on an event. The harness — not Claude — runs hooks.

**Layout**
```
hooks/hooks.json
```

**Events** (28 total; the ones you will actually use):

| Event | Fires when |
|---|---|
| `PreToolUse` | Before any tool call |
| `PostToolUse` | After any tool call |
| `UserPromptSubmit` | Before the user's message reaches Claude |
| `Stop` | When Claude finishes a turn |
| `SubagentStop` | When a sub-agent finishes |
| `Notification` | On permission prompts |
| `SessionStart` / `SessionEnd` | Session lifecycle |
| `PreCompact` | Before auto-compaction runs |

(Full list is in the Claude Code reference. Use the matcher format `"matcher": "Bash"` to scope to a tool name; `"matcher": ".*"` for wildcard.)

**Hook types**
- `command` — shell command; **exit code 2 blocks** the triggering action.
- `http` — POST JSON to a URL.
- `prompt` — inject text into the conversation.
- `agent` — invoke a named sub-agent.

**Gotchas**
- Hooks see tool I/O and prompt content — treat them as security-sensitive.
- Exit code 2 is a hard block; anything else is advisory.

---

## MCP server

**Use when** the plugin needs to bridge an external system (API, database, CLI) into Claude over stdio JSON-RPC. Not for orchestration — that's a subagent.

**Layout**
```
.mcp.json
```

**Shape (stdio only)**
```json
{
  "mcpServers": {
    "<name>": {
      "command": "node",
      "args": ["./server.js"],
      "env": { "API_TOKEN": "${MY_TOKEN}" },
      "cwd": "./servers/<name>"
    }
  }
}
```

**Gotchas**
- Plugins can only ship **stdio** MCP servers. SSE/HTTP servers live in the user's own config.
- `env` interpolates `${VAR}` at load time.

---

## LSP server

**Use when** you need real language-server-protocol intelligence (go-to-def, rename, hover). Rare in user plugins — usually you want a subagent that shells out to an existing LSP.

---

## Uppercase `.md` reference doc

**Use when** content is over ~200 lines or only needed on specific branches of the workflow. Put it next to `SKILL.md` and link from there. File name is uppercase (`PATTERNS.md`, `SURFACES.md`, `REFERENCE.md`) by convention.

---

## Decision flowchart (terse)

```
requirement
├── reactive on an event?  → hook (exit-code-2 to block)
├── bridges external system via stdio JSON-RPC?  → MCP server
├── user types `/name args` literally and shape matters?  → slash command
├── needs fresh context / restricted tools / parallel spokes?  → subagent
├── conversational trigger, prose + workflow?  → skill (default)
└── supporting reference >200 lines?  → uppercase .md, lazily linked from SKILL.md
```

If unsure, pick skill. You can add a subagent later without restructuring.
