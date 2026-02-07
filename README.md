# claude-code-plugin-template

Template for building Claude Code plugins. Fork it, fill in the TODOs, ship.

## Quick Start

### Option A: Let AI build it for you

Paste this into Claude Code (or any AI assistant):

```
Fetch https://raw.githubusercontent.com/lvluu/claude-code-plugin-template/main/INSTRUCTIONS.md and follow it to build me a plugin called <your-plugin-name>
```

Claude will clone the template, ask what you need, and scaffold the whole plugin.

### Option B: Manual

```bash
# 1. Fork / clone this template
gh repo create my-plugin --template lvluu/claude-code-plugin-template

# 2. Edit .claude-plugin/plugin.json — set name, description, author

# 3. Pick MCP servers from examples/mcps/ and paste into .mcp.json

# 4. Add your commands, skills, agents, hooks

# 5. Test
claude --plugin-dir .

# 6. Install
claude plugin install . --scope user
```

## Structure

```
your-plugin/
├── .claude-plugin/
│   └── plugin.json             # Plugin manifest (name, description, wiring)
├── .mcp.json                   # MCP servers (starts empty — you fill it)
│
├── commands/                   # Slash commands (user-invoked via /plugin:cmd)
│   └── .template.md            # ← copy & rename to create a command
├── skills/                     # Skills (auto-invoked by Claude based on context)
│   └── .template/
│       └── SKILL.md            # ← copy dir & rename to create a skill
├── agents/                     # Subagents (specialized roles)
│   └── .template.md            # ← copy & rename to create an agent
├── hooks/
│   └── hooks.json              # Event hooks (starts empty — you fill it)
│
├── examples/                   # Reference catalog — don't deploy, just copy from
│   ├── mcps/                   # Ready-to-paste MCP server configs
│   │   ├── README.md           # Server catalog with setup notes
│   │   ├── serena.json         # Code intelligence (LSP, 30+ langs)
│   │   ├── chrome-devtools.json  # DevTools Protocol
│   │   ├── context7.json       # Library docs lookup
│   │   └── shadcn.json         # Shadcn UI components
│   ├── commands/               # Example commands
│   │   └── lint-check.md
│   ├── agents/                 # Example agents
│   │   └── code-reviewer.md
│   ├── skills/                 # Example skills
│   │   └── commit-conventions/
│   │       └── SKILL.md
│   └── hooks/                  # Example hook configs
│       └── pre-edit-warning.json
│
└── .gitignore
```

## How To

### Add MCP Servers

Browse `examples/mcps/`, copy what you need into `.mcp.json`:

```jsonc
// .mcp.json
{
  "mcpServers": {
    // paste from examples/mcps/context7.json
    "context7": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp"]
    }
  }
}
```

See `examples/mcps/README.md` for the full catalog and optional API key setup.

### Add a Command

```bash
cp commands/.template.md commands/my-command.md
```

Edit the frontmatter and instructions. Invoked as `/your-plugin:my-command`.

**Frontmatter options:**

| Field | Description |
|-------|-------------|
| `description` | Shown in `/help` |
| `argument-hint` | Usage hint: `<required> [optional]` |
| `allowed-tools` | Pre-approved tools (skip permission prompts) |
| `model` | Override: `opus`, `sonnet`, `haiku` |

### Add a Skill

```bash
cp -r skills/.template skills/my-skill
```

Edit `skills/my-skill/SKILL.md`. The `description` field is critical — it tells Claude **when** to auto-invoke.

Optional subdirs: `references/`, `examples/`, `scripts/`

### Add an Agent

```bash
cp agents/.template.md agents/my-agent.md
```

Edit frontmatter (`name`, `description`) and write the system prompt below.

**Frontmatter options:**

| Field | Description |
|-------|-------------|
| `name` | Unique identifier |
| `description` | When Claude should invoke this agent |
| `model` | `opus`, `sonnet`, `haiku` |
| `color` | UI indicator: `green`, `blue`, `red` |

### Add Hooks

Edit `hooks/hooks.json` or copy from `examples/hooks/`:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "hooks": [{ "type": "command", "command": "your-script.sh", "timeout": 10 }],
        "matcher": "Edit|Write"
      }
    ]
  }
}
```

**Available events:** `PreToolUse`, `PostToolUse`, `Stop`, `SessionStart`, `SessionEnd`, `UserPromptSubmit`, `SubagentStart`, `SubagentStop`, `PreCompact`

## Testing

```bash
# Load plugin from local directory
claude --plugin-dir .

# Load multiple plugins
claude --plugin-dir ./plugin-a --plugin-dir ./plugin-b
```

## Install Scopes

```bash
claude plugin install . --scope user     # personal (default)
claude plugin install . --scope project  # team (version controlled)
claude plugin install . --scope local    # project-specific (gitignored)
```

## License

MIT
