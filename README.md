# claude-code-plugin

Template for building Claude Code plugins. Fork it, fill in the TODOs, ship.

## Get Started

### Let AI do it

Paste this into Claude Code, Cursor, or any AI assistant:

```
Fetch https://raw.githubusercontent.com/lvluu/claude-code-plugin/main/INSTRUCTIONS_AI.md and follow it to build me a plugin called <your-plugin-name>
```

It will clone the template, ask what you need (MCPs, commands, skills, agents), and scaffold everything.

### Do it yourself

Read **[INSTRUCTIONS.md](./INSTRUCTIONS.md)** — human-friendly, step-by-step, ~10 minutes.

```bash
git clone https://github.com/lvluu/claude-code-plugin.git my-plugin
cd my-plugin
rm -rf .git && git init
# follow INSTRUCTIONS.md from here
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
│   │   ├── serena.json         # Code intelligence (LSP, 30+ langs)
│   │   ├── chrome-devtools.json  # DevTools Protocol
│   │   ├── context7.json       # Library docs lookup
│   │   └── shadcn.json         # Shadcn UI components
│   ├── commands/               # Example commands
│   ├── agents/                 # Example agents
│   ├── skills/                 # Example skills
│   └── hooks/                  # Example hook configs
│
├── INSTRUCTIONS.md             # Human guide
├── INSTRUCTIONS_AI.md          # AI guide (machine-readable)
└── .gitignore
```

## Quick Reference

| I want to... | Do this |
|--------------|---------|
| Add an MCP server | Copy from `examples/mcps/` into `.mcp.json` |
| Add a command | `cp commands/.template.md commands/name.md` |
| Add a skill | `cp -r skills/.template skills/name` |
| Add an agent | `cp agents/.template.md agents/name.md` |
| Add hooks | Edit `hooks/hooks.json` |
| Test locally | `claude --plugin-dir .` |
| Install | `claude plugin install . --scope user` |

## License

MIT
