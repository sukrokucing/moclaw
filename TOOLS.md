_This space is yours. Append your own notes below to make it your own._

## Browser

- **Always use `agent-browser`** for ALL browser tasks — opening pages, clicking, filling forms, screenshots, scraping, etc.
- Binary: `~/.npm-global/bin/agent-browser` (v0.27.0)
- Chrome: `~/.agent-browser/browsers/chrome-148.0.7778.167`
- **Never use** sandbox-mcp browser_* tools or camoufox-cli when agent-browser is available
- PATH: `export PATH="$HOME/.npm-global/bin:$HOME/.local/bin:$PATH"`

### Quick ref
```bash
agent-browser open <url>
agent-browser snapshot -i          # get element refs
agent-browser click @e1
agent-browser fill @e2 "text"
agent-browser screenshot page.png
agent-browser close
```

## Pi Coding Agent (RoboLuau)

- **Binary:** `~/.npm-global/bin/pi` (v0.74.0)
- **PATH:** `export PATH="$HOME/.npm-global/bin:$PATH"`
- **Install:** `npm install -g @earendil-works/pi-coding-agent`
- **Docs:** https://pi.dev / https://github.com/earendil-works/pi

### Global config
- **AGENTS.md:** `~/.pi/agent/AGENTS.md` — RoboLuau identity + all skill references
- **Settings:** `~/.pi/agent/settings.json` — provider: anthropic, model: claude-opus-4-5
- **Skills:** `~/.pi/agent/skills/` — 14 skills (all Roblox + game design + dev workflow)
- **Prompts:** `~/.pi/agent/prompts/` — bugfix, review, newfeature, security-audit

### Project config
- **Project AGENTS.md:** `projects/roblox-startup/AGENTS.md` — Harvest RNG layout + patterns
- Pi auto-loads AGENTS.md from current dir and parent dirs on startup

### Launching RoboLuau
```bash
export PATH="$HOME/.npm-global/bin:$PATH"
export ANTHROPIC_API_KEY="<key>"

# Interactive session in project dir
cd /home/user/.workspace/projects/roblox-startup
pi

# One-shot print mode
pi -p "Review all remote handlers for security issues"

# Load specific skill
pi "/skill:roblox-security" "audit all remote handlers"

# Use prompt template
pi "/bugfix" "VIP luck stacks on rejoin"
pi "/review" "src/server/GameManager.server.lua"
pi "/security-audit" "all remote handlers"
```

### Available skills (auto-discovered from ~/.pi/agent/skills/)
| Skill | When to use |
|---|---|
| `roblox-security` | anti-exploit, rate limiting, server-side validation |
| `roblox-remote-events` | RemoteEvent/RemoteFunction patterns |
| `roblox-datastores` | DataStore, auto-save, BindToClose, ordered stores |
| `roblox-performance` | StreamingEnabled, object pooling, loop optimization |
| `roblox-game-development` | full Luau scripting, architecture, monetization |
| `game-design-core` | MDA framework, core loops, player psychology |
| `game-design-theory` | balance, progression, reward systems |
| `level-design` | flow pillars, pacing, discovery, spatial design |
| `systematic-debugging` | root cause investigation before fixes |
| `code-review-quality` | structured code review with severity levels |
| `writing-plans` | implementation plans for multi-step tasks |
| `brainstorming` | design exploration before implementation |
| `deep-research` | multi-source research and analysis |
| `planning-with-files` | file-based planning for complex tasks |

### Prompt templates
| Template | Use |
|---|---|
| `/bugfix` | Fix a specific bug with root cause investigation |
| `/review` | Code review with severity-ranked findings |
| `/newfeature` | Design + implement a new feature |
| `/security-audit` | Full security audit of remote handlers |
