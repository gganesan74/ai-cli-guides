# Codex — Install & Configuration (Linux)

OpenAI's terminal AI coding agent. Installs via npm and uses **TOML** for configuration (not JSON).

## 1. Install

```bash
npm install -g @openai/codex
```

> **Don't use `sudo`**. If you hit `EACCES`, configure a user-owned npm prefix (see the top-level `README.md`).

## 2. Verify

```bash
codex --version
```

## 3. Authenticate

Run `codex` in any terminal. On first launch you'll be prompted to sign in with either:

- Your **ChatGPT account** (OAuth flow opens in browser), or
- An **API key** (`printenv OPENAI_API_KEY | codex login --with-api-key`)

Credentials are stored at `~/.codex/auth.json` by default. For better security, you can opt into the system keyring by setting `cli_auth_credentials_store = "keyring"` in your config.

## 4. Where the config files live

Codex stores everything under `CODEX_HOME`, which defaults to `~/.codex/`. Configs are resolved in this precedence order (highest first):

1. `--config` CLI overrides
2. **Project config** files: `.codex/config.toml`, walked from project root down to your current working directory (closest wins; **trusted projects only**)
3. **User config**: `~/.codex/config.toml`

### Directory layout

```
~/                                     # your home directory
└── .codex/
    ├── config.toml                    # user-level config
    ├── auth.json                      # credentials (unless using keyring)
    ├── history.jsonl                  # command history
    ├── sessions/                      # full conversation transcripts
    │   └── ...
    ├── log/
    │   └── codex-tui.log              # TUI logs
    └── AGENTS.md                      # optional global instructions

<your-project>/
├── .codex/
│   └── config.toml                    # project-specific overrides (trusted projects only)
└── AGENTS.md                          # project-level instructions Codex reads each session
```

### What goes where

| File | Purpose | In Git? |
|------|---------|---------|
| `~/.codex/config.toml` | Default model, sandbox/approval policy, MCP servers, feature flags | N/A (personal machine) |
| `~/.codex/auth.json` | Your credentials | ❌ Never |
| `<project>/.codex/config.toml` | Project model/permission overrides | ✅ Usually yes |
| `<project>/AGENTS.md` (root) | Project-wide guidance and conventions | ✅ Yes |
| `<subfolder>/AGENTS.md` | Subfolder-specific guidance (e.g., per-package) | ✅ Yes |

> **Important**: Codex only loads project-level `.codex/` configs from projects you have **trusted**. You'll be prompted to trust a project on first use.

### Minimal `~/.codex/config.toml` example

```toml
# Default model for all sessions
model = "gpt-5.4"

# Approval policy: untrusted | on-request | never
approval_policy = "on-request"

# Built-in permission profile: :read-only | :workspace | :danger-no-sandbox
default_permissions = ":workspace"

# Where to open file citations (vscode | cursor | windsurf | none)
file_opener = "vscode"

[features]
shell_snapshot = true   # speed up repeated commands
```

### Per-run overrides

You can override any key for a single run without editing `config.toml`:

```bash
# Dedicated flag
codex --model gpt-5.4

# Generic key=value override (value is parsed as TOML)
codex --config model='"gpt-5.4"'
codex --config sandbox_workspace_write.network_access=true
codex --config 'mcp_servers.context7.enabled=false'
```

## 5. Troubleshooting

- **`command not found: codex`** — Your global npm bin isn't on PATH. Run `npm config get prefix` and add `/bin` to your PATH.
- **Project `.codex/config.toml` is ignored** — You haven't trusted the project yet. Codex will prompt; accept it.
- **Want to relocate the config directory** — Set `CODEX_HOME=/your/path` in your shell.
- **Permission errors during install** — Don't use `sudo`. See the user-owned npm prefix setup in the top-level `README.md`.

## 6. Uninstall

```bash
npm uninstall -g @openai/codex
rm -rf ~/.codex
```
