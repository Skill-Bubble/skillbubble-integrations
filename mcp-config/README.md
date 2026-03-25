# MCP Config Snippets

Copy the config for your AI assistant and drop it into the right file.

## Before you start

1. Sign in to SkillBubble → [Settings → Integrations](https://www.skillbubble.com/settings/integrations)
2. Generate an API key
3. Replace `sk-sb-your-api-key-here` in the config below with your key

---

## Kiro

**File**: `~/.kiro/settings/mcp.json`

```json
// kiro.json
```

Uses Kiro's native `streamableHttp` transport — no extra packages needed.

---

## Claude Desktop / Claude Code

**File**: `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS)  
**File**: `%APPDATA%\Claude\claude_desktop_config.json` (Windows)

```json
// claude.json
```

Uses `mcp-remote` to bridge HTTP → stdio. Requires Node.js installed.

---

## GitHub Copilot (VS Code)

**File**: `.vscode/mcp.json` in your workspace, or user-level settings

```json
// copilot.json
```

Uses `mcp-remote` — same as Claude. Requires Node.js installed.

---

## Other MCP clients

Any client that supports `streamableHttp` can connect directly:

```
URL:     https://mcp.skillbubble.com/mcp
Header:  Authorization: sk-sb-your-api-key-here
```

For clients that only support stdio, use `mcp-remote`:

```bash
npx -y mcp-remote@latest https://mcp.skillbubble.com/mcp \
  --header "Authorization:sk-sb-your-api-key-here"
```
