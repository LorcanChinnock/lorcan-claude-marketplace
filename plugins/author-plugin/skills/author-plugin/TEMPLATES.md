# TEMPLATES.md — copy-paste skeletons

Per-surface scaffolding. Copy the fenced block, fill placeholders, stay within `PATTERNS.md` rules.

## plugin.json

```json
{
  "name": "<kebab-name>",
  "version": "0.1.0",
  "description": "<one-line triggers + outcome, no workflow narration>",
  "author": {
    "name": "Lorcan Chinnock",
    "email": "ljchinnock@gmail.com"
  }
}
```

## SKILL.md

```markdown
---
name: <kebab-name>
description: Use when <third-person triggers>. <One line of outcome>.
allowed-tools:
  - Read
  - Write
  - Edit
  - Grep
  - Glob
  - AskUserQuestion
---

You are <name>. <One paragraph: what you produce, what you do not>.

## Process

### 1. <Resolve scope / inputs>

<What you need from the user. Use AskUserQuestion only when ambiguity cannot be resolved from the input.>

### 2. <Do the work>

<Constraints: preserve X, never Y, limit to Z.>

### 3. Self-check

<Deterministic checks. Fix silently and re-scan.>

### 4. Output

<Fixed output contract. Describe exactly what the user sees.>

## What this skill is not

- Not a <nearest-miss capability>.
- Not a <common wrong-shape confusion>.
- Not a <thing it could be mistaken for>.
```

## agent.md

```markdown
---
name: <kebab-name>
description: <triggers + what it returns; third-person>
tools: Read, Grep, Glob, Bash
model: inherit
color: purple
---

You are <Name>, a <role>. <What you output; what you never do>.

Methodology:

- <Principle 1>
- <Principle 2>
- No emojis. Direct tone. No sycophancy.

---

## Inputs

<Describe the argument shapes the agent accepts.>

## Pipeline

### 1. <Step>

<What runs, what it returns.>

### 2. <Step>

...

## Output

<Fixed format the caller parses.>

## What not to do

- <Out-of-scope behaviour 1>
- <Out-of-scope behaviour 2>
```

**Never** add `hooks`, `mcpServers`, or `permissionMode` to agent frontmatter — Claude Code rejects these in plugin agents. Settings that would use them belong in `.claude/settings.json` in the harness, not in the plugin.

## hooks/hooks.json

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "path/to/guard.sh",
            "timeout": 5
          }
        ]
      }
    ]
  }
}
```

Event options: `PreToolUse`, `PostToolUse`, `UserPromptSubmit`, `Stop`, `SubagentStop`, `Notification`, `SessionStart`, `SessionEnd`, `PreCompact`, and the rest of the 28-event list.

Hook `type` options: `command` (exit 2 blocks), `http`, `prompt`, `agent`.

## .mcp.json

```json
{
  "mcpServers": {
    "<server-name>": {
      "command": "node",
      "args": ["./servers/<server-name>/index.js"],
      "env": {
        "API_TOKEN": "${MY_API_TOKEN}"
      },
      "cwd": "./servers/<server-name>"
    }
  }
}
```

Stdio only in plugins. SSE/HTTP servers belong in user config.

## REFERENCE.md (uppercase reference doc)

```markdown
# REFERENCE.md — <topic>

<One-paragraph scope: what this file covers and when SKILL.md should link in.>

## <Section>

<Content. Keep each section under ~200 words if it is loaded frequently; heavy refs may exceed.>

## <Section>

...
```

Link from SKILL.md with `[REFERENCE.md](REFERENCE.md)` — never `@REFERENCE.md`.

## README.md (plugin root)

```markdown
# <kebab-name>

<One-line purpose, matching plugin.json description>.

## Install

\`\`\`
/plugin install <kebab-name>@lorcan-claude-marketplace
\`\`\`

## Invoke

Ask for it in conversation (the skill's `description` drives auto-match), or invoke explicitly:

\`\`\`
/<plugin-name>:<skill-name>
\`\`\`

<One paragraph on argument shapes if relevant.>

## What it does

- <Concrete behaviour 1>
- <Concrete behaviour 2>
- <Concrete behaviour 3>

## What it does not do

- <Boundary 1>
- <Boundary 2>
```

## marketplace.json plugins[] entry

```json
{
  "name": "<kebab-name>",
  "source": "./plugins/<kebab-name>",
  "description": "<same shape as plugin.json description>",
  "author": {
    "name": "Lorcan Chinnock",
    "email": "ljchinnock@gmail.com"
  },
  "category": "productivity",
  "homepage": "https://github.com/LorcanChinnock/lorcan-claude-marketplace/tree/main/plugins/<kebab-name>"
}
```

Append inside the existing `plugins` array. Match field order exactly.
