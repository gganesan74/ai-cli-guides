# Claude Code — Install & Configuration (Linux)

Anthropic's terminal AI coding agent. The native installer is now the **recommended** install method; npm still works but is officially deprecated.

## 1. Install

### Recommended: native installer

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

This drops a self-contained binary onto your PATH and configures background auto-updates. **No Node.js required.**

### Alternative: npm

If you need version pinning or already manage everything through npm:

```bash
npm install -g @anthropic-ai/claude-code
```

> **Don't use `sudo`** for the npm method — it creates permission and security issues. If you hit `EACCES`, configure a user-owned npm prefix (see the top-level `README.md`).

## 2. Verify

```bash
claude --version
claude doctor   # diagnostic — reports any setup or configuration problems
```

## 3. Authenticate

Run `claude` from any directory. It will open your browser to sign in to your Anthropic account (Claude Pro, Max, Team, Enterprise, or Console/API). Credentials are stored locally and persist between sessions — you won't need to re-authenticate unless you log out or your credentials expire.

## 4. Where the config files live

Claude Code uses a **hierarchical settings system** in JSON. Settings are merged top-down, with later layers overriding earlier ones for conflicting keys:

1. **User settings** (global defaults)
2. **Project shared settings** (team-wide, checked into Git)
3. **Project local settings** (your personal overrides, gitignored)

### Directory layout

```
~/                                     # your home directory
└── .claude/
    ├── settings.json                  # user-level config (global)
    └── ...                            # auth state, cache, etc.

<your-project>/
├── .claude/
│   ├── settings.json                  # shared project settings (commit to Git)
│   └── settings.local.json            # your personal project overrides (gitignore!)
└── CLAUDE.md                          # project guidance Claude reads each session
```

### What goes where

| File | Purpose | In Git? |
|------|---------|---------|
| `~/.claude/settings.json` | Global defaults: env vars, auto-update channel, default model, etc. | N/A (personal machine) |
| `<project>/.claude/settings.json` | Team-wide project rules and permissions | ✅ Yes |
| `<project>/.claude/settings.local.json` | Personal API keys, local-only tweaks | ❌ No (add to `.gitignore`) |
| `<project>/CLAUDE.md` | Project description, conventions, code style guidance | ✅ Yes |

### Minimal `~/.claude/settings.json` example

```json
{
  "env": {
    "DISABLE_AUTOUPDATER": "0"
  },
  "autoUpdatesChannel": "stable"
}
```

`autoUpdatesChannel` accepts `"latest"` (new features as soon as released) or `"stable"` (~1 week behind, skips releases with major regressions).

## 5. Troubleshooting

- **Settings changes not taking effect** — Close all Claude Code windows and start a fresh terminal. If still broken, validate your JSON (a stray comma silently breaks the whole file).
- **`command not found: claude`** — If you used npm, your global npm bin isn't on PATH. Run `npm config get prefix` and add `/bin` to your PATH.
- **Permission errors during install** — Don't use `sudo`. See the user-owned npm prefix setup in the top-level `README.md`.
- **Anything else** — Run `claude doctor`. It auto-detects most configuration issues and suggests fixes.

## 6. Uninstall

```bash
# If installed via native installer:
claude uninstall

# If installed via npm:
npm uninstall -g @anthropic-ai/claude-code
rm -rf ~/.claude
```
