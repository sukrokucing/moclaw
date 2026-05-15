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
