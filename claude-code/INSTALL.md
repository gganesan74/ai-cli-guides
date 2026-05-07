# Claude Code — Install & Configuration (Linux)

Anthropic's terminal AI coding agent. The native installer is now the **recommended** install method; npm still works but is officially deprecated.

## 1. Install

### Recommended: native installer

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

This drops a self-contained binary onto your PATH and configures background auto-updates. **No Node.js required.**


## 2. Verify

```bash
claude --version
claude doctor   # diagnostic — reports any setup or configuration problems
```

## 3. Set the config files. 

Edit the `~/.claude/settings.json`  (Example provided in this directory).
This is a local install with a user specified LLM. So edit the settings.json file to point to the right LLM. 

Project settings (for example ask Claude to use uv for running code) live in the CLAUDE.md file (one per root project directory). An example is placed in this directory. Copy and modify this for every project directory that you will use. 

Where each config lives is shown in the next section. 

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

## 5. Run the code
Run `claude` from any directory. 

## 6. Uninstall

```bash

claude uninstall

```
