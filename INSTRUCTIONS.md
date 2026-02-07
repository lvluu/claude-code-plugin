# How to Build a Plugin (Human Guide)

Step-by-step walkthrough. Takes ~10 minutes.

---

## 1. Clone the template

```bash
git clone https://github.com/lvluu/claude-code-plugin.git my-plugin
cd my-plugin
rm -rf .git && git init
```

## 2. Name your plugin

Open `.claude-plugin/plugin.json` and fill in:

```json
{
  "name": "my-plugin",
  "description": "What your plugin does in one sentence",
  "author": { "name": "Your Name" }
}
```

## 3. Pick MCP servers

Browse `examples/mcps/` — each file is a ready-to-paste config:

| File | Server | What it does |
|------|--------|-------------|
| `serena.json` | Serena | Code intelligence via LSP (30+ langs) |
| `chrome-devtools.json` | Chrome DevTools | Inspect, debug, profile via CDP |
| `context7.json` | Context7 | Library/framework docs lookup |
| `shadcn.json` | Shadcn | Official shadcn CLI MCP |

Copy what you need into `.mcp.json`:

```jsonc
{
  "mcpServers": {
    // paste entries from examples/mcps/ here
  }
}
```

Don't need any MCP servers? Leave it empty. You can always add them later.

## 4. Add commands (optional)

Commands are slash commands the user types manually (e.g. `/my-plugin:deploy`).

```bash
cp commands/.template.md commands/deploy.md
```

Edit the file — the frontmatter controls behavior:

```markdown
---
description: Deploy to staging
argument-hint: <environment>
allowed-tools: [Bash]
---

Deploy $ARGUMENTS to staging.

1. Run the deploy script
2. Verify health check
3. Report status
```

## 5. Add skills (optional)

Skills are auto-invoked by Claude based on context — no user action needed.

```bash
cp -r skills/.template skills/my-skill
```

Edit `skills/my-skill/SKILL.md`. The `description` is the trigger — be specific:

```markdown
---
name: my-skill
description: Auto-invoked when user asks about database migrations or schema changes
---
```

## 6. Add agents (optional)

Agents are specialized subagents with their own system prompt.

```bash
cp agents/.template.md agents/reviewer.md
```

Edit the frontmatter and write the system prompt below it.

## 7. Add hooks (optional)

Most plugins don't need hooks initially. If you do, edit `hooks/hooks.json`:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "hooks": [{ "type": "command", "command": "your-script.sh" }],
        "matcher": "Edit|Write"
      }
    ]
  }
}
```

## 8. Clean up

- Delete `.template` files you didn't use (or keep them for later)
- Optionally delete `examples/` for a cleaner deploy
- Update `README.md` to describe your actual plugin

## 9. Test

```bash
claude --plugin-dir .
```

## 10. Install

```bash
# Personal use
claude plugin install . --scope user

# Share with team (version controlled)
claude plugin install . --scope project
```

---

## Cheat Sheet

| I want to... | Do this |
|--------------|---------|
| Add an MCP server | Copy from `examples/mcps/` into `.mcp.json` |
| Add a command | `cp commands/.template.md commands/name.md` |
| Add a skill | `cp -r skills/.template skills/name` |
| Add an agent | `cp agents/.template.md agents/name.md` |
| Add hooks | Edit `hooks/hooks.json` |
| Test locally | `claude --plugin-dir .` |
| Install | `claude plugin install . --scope user` |
