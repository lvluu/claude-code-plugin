# Claude Code Plugin — AI Build Instructions

You are scaffolding a new Claude Code plugin using a template. Follow these steps exactly.

## Step 1: Clone the Template

```bash
git clone https://github.com/lvluu/claude-code-plugin.git <plugin-name>
cd <plugin-name>
rm -rf .git
git init
```

Ask the user for `<plugin-name>` if not already provided.

## Step 2: Gather Requirements

Ask the user (skip anything already answered):

1. **Plugin name** — kebab-case identifier (e.g. `my-awesome-plugin`)
2. **Description** — one sentence: what does this plugin do?
3. **Author name**
4. **Which MCP servers?** — show this catalog and let them pick:
   | Server | What it does |
   |--------|-------------|
   | `serena` | Semantic code intelligence via LSP (30+ langs) |
   | `chrome-devtools` | Chrome DevTools Protocol (inspect, debug, profile) |
   | `context7` | Up-to-date library/framework documentation lookup |
   | `shadcn` | Official shadcn CLI with MCP support |
   | _(custom)_ | User can provide their own MCP server config |
5. **Commands** — any slash commands? Ask for name + what it should do
6. **Skills** — any auto-invoked skills? Ask for name + trigger conditions + instructions
7. **Agents** — any subagents? Ask for name + role + system prompt summary
8. **Hooks** — any event hooks? (most users won't need these initially)

## Step 3: Fill in plugin.json

Edit `.claude-plugin/plugin.json`:

```json
{
  "name": "<plugin-name>",
  "version": "0.1.0",
  "description": "<user's description>",
  "author": {
    "name": "<user's name>"
  },
  "license": "MIT",
  "keywords": [<relevant keywords>],
  "mcpServers": "./.mcp.json",
  "agents": "./agents/",
  "skills": "./skills/",
  "hooks": "./hooks/hooks.json"
}
```

## Step 4: Configure MCP Servers

For each server the user selected, read the corresponding file from `examples/mcps/<name>.json` and merge the entry into `.mcp.json` under `"mcpServers"`.

Example — if user picks `context7` and `shadcn`:

```json
{
  "mcpServers": {
    "context7": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp"]
    },
    "shadcn": {
      "command": "npx",
      "args": ["shadcn@latest", "mcp"]
    }
  }
}
```

If the user provides a custom MCP config, add it as-is.

## Step 5: Create Commands

For each command the user requested:

1. Copy `commands/.template.md` to `commands/<command-name>.md`
2. Fill in the frontmatter:
   - `description`: what the command does
   - `argument-hint`: expected arguments (if any)
   - `allowed-tools`: tools this command needs (default: `[Read, Glob, Grep, Bash]`)
3. Write the command instructions in markdown below the frontmatter
4. The variable `$ARGUMENTS` captures everything the user types after the command name

## Step 6: Create Skills

For each skill the user requested:

1. Copy `skills/.template/` to `skills/<skill-name>/`
2. Edit `skills/<skill-name>/SKILL.md`:
   - `name`: skill identifier
   - `description`: **critical** — this tells Claude WHEN to auto-invoke. Be specific with trigger phrases.
   - `version`: `0.1.0`
3. Write the skill instructions: what Claude should do when the skill activates

## Step 7: Create Agents

For each agent the user requested:

1. Copy `agents/.template.md` to `agents/<agent-name>.md`
2. Fill in the frontmatter:
   - `name`: agent identifier
   - `description`: when to invoke this agent (trigger conditions)
   - `model`: `opus` (complex), `sonnet` (balanced), `haiku` (fast)
   - `color`: UI color (`green`, `blue`, `red`, `yellow`, `purple`)
3. Write the agent's full system prompt below the frontmatter

## Step 8: Configure Hooks (if needed)

Edit `hooks/hooks.json`. Available events:

| Event | When it fires |
|-------|---------------|
| `PreToolUse` | Before Claude uses any tool |
| `PostToolUse` | After tool execution succeeds |
| `Stop` | When Claude attempts to stop |
| `SessionStart` | At session start |
| `SessionEnd` | At session end |
| `UserPromptSubmit` | When user submits a prompt |
| `SubagentStart` | When a subagent starts |
| `SubagentStop` | When a subagent stops |
| `PreCompact` | Before conversation compaction |

Hook format:

```json
{
  "hooks": {
    "<Event>": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "path/to/script.sh",
            "timeout": 10
          }
        ],
        "matcher": "Edit|Write"
      }
    ]
  }
}
```

Use `${CLAUDE_PLUGIN_ROOT}` for plugin-relative paths in hook commands.

## Step 9: Clean Up

- Delete any `.template` files that weren't used (leave them if user might add more later)
- Delete `examples/` directory if the user wants a clean deploy (optional — it's harmless to keep)
- Update `README.md` to describe the actual plugin, not the template

## Step 10: Test & Verify

```bash
# Test the plugin
claude --plugin-dir .

# Verify structure
find . -type f -not -path './.git/*' | sort
```

Confirm with the user that:
- MCP servers connect successfully
- Commands appear in `/help`
- Skills/agents are listed

## Rules

- NEVER leave TODO placeholders in shipped files
- NEVER invent MCP configs — use `examples/mcps/` or ask the user
- NEVER add MCP servers the user didn't request
- ALWAYS use the exact JSON format from the example files
- ALWAYS ask before adding hooks (most plugins don't need them initially)
- Keep command/skill/agent content focused and concise — no filler prose
