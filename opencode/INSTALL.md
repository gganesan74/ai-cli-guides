# OpenCode — Install & Configuration (Linux)

Open-source terminal AI coding agent supporting many providers (Anthropic, OpenAI, Google, OpenCode Zen, local models via Ollama, etc.). Uses **JSON or JSONC** (JSON with comments) for configuration.

## 1. Install

### Recommended: install script

```bash
curl -fsSL https://opencode.ai/install | bash
```

The script picks an install directory in this priority order:

1. `$OPENCODE_INSTALL_DIR` — your override
2. `$XDG_BIN_DIR` — XDG Base Directory–compliant path
3. `$HOME/bin` — if it exists or can be created
4. `$HOME/.opencode/bin` — default fallback

You can force a specific location:

```bash
XDG_BIN_DIR=$HOME/.local/bin curl -fsSL https://opencode.ai/install | bash
```


## 2. Verify

```bash
opencode --version
```

## 3. Authenticate

LLM configuration is provided in  `~/.config/opencode/opencode.json` 
An exampl opencode.json is placed in this directory.

Credentials are stored at `~/.local/share/opencode/auth.json`.
An example auth.json for a local LLM is provided.

Run `opencode` inside any project directory. In the TUI:

1. Run `/connect` and pick a provider (OpenAI, Anthropic, Google, OpenCode Zen, etc.).
2. Paste your API key when prompted.
3. (Optional) Run `/init` so OpenCode scans your project and generates an `AGENTS.md` automatically.

Credentials are stored at `~/.local/share/opencode/auth.json`.

## 4. Where the config files live

OpenCode merges configs from multiple sources in this precedence order (later overrides earlier):

1. **Remote config** (organizational defaults from `.well-known/opencode`)
2. **User config** — `~/.config/opencode/opencode.json`
3. **Custom config** — path set via `$OPENCODE_CONFIG`
4. **Project config** — `<project>/opencode.json`

Configs are **merged**, not replaced — settings from each layer combine, with later layers winning only on conflicting keys.

### Directory layout

```
~/                                     # your home directory
├── .config/
│   └── opencode/
│       ├── opencode.json              # user-level config (or .jsonc for comments)
│       ├── tui.json                   # optional TUI-specific settings
│       └── plugins/                   # global plugins
│           └── ...
└── .local/
    └── share/
        └── opencode/
            ├── auth.json              # credentials
            └── ...                    # sessions, cache, etc.

<your-project>/
├── opencode.json                      # project config (highest precedence)
├── tui.json                           # optional project TUI settings
├── .opencode/                         # project-scoped agents/commands/plugins
│   ├── agents/
│   ├── commands/
│   └── plugins/
└── AGENTS.md                          # project guidance OpenCode reads each session
```

### What goes where

| File | Purpose | In Git? |
|------|---------|---------|
| `~/.config/opencode/opencode.json` | Default model, providers, autoupdate, formatters | N/A (personal machine) |
| `~/.local/share/opencode/auth.json` | Credentials | ❌ Never |
| `<project>/opencode.json` | Project model and provider overrides | ✅ Yes |
| `<project>/.opencode/` | Project-scoped agents, commands, plugins | ✅ Usually yes |
| `<project>/AGENTS.md` | Project-wide guidance and conventions | ✅ Yes |

> **Heads up**: Older OpenCode installs sometimes used `~/.local/share/opencode/opencode.jsonc`. The current canonical location is `~/.config/opencode/`. If you have both, consolidate to `~/.config/opencode/`.

### Minimal `~/.config/opencode/opencode.json` example

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "vllm": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "vLLM (local)",
      "options": {
        "baseURL": "http://localhost:8000/v1"
      },
      "models": {
        "qwen3.6": {
          "name": "Qwen Coder"
        }
      }
    }
  },
  "model": "vllm/qwen3.6"
}
```

### Project `opencode.json` example. This contains instructions files, schema files etc. Change LLM name / path etc according to your settings. 

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "vllm": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "vLLM (local)",
      "options": {
        "baseURL": "http://localhost:8000/v1"
      },
      "models": {
        "Qwen3-Coder-30B-A3B-Instruct": {
          "name": "My vLLM model"
        }
      }
    }
  },
  "model": "vllm/Qwen3-Coder-30B-A3B-Instruct",
  "small_model": "vllm/Qwen3-Coder-30B-A3B-Instruct",
  "instructions": ["CONTRIBUTING.md", "docs/guidelines.md", ".cursor/rules/*.md"]
}
```

### Custom config path

If you want a non-standard location:

```bash
export OPENCODE_CONFIG=/path/to/my-config.json
```

This is loaded between global and project configs in the precedence order.

## 5. Troubleshooting

- **`command not found: opencode`** — The chosen install directory isn't on PATH. Check `$XDG_BIN_DIR`, `$HOME/bin`, or `$HOME/.opencode/bin`. For npm installs, `npm config get prefix` and add `/bin`.
- **`ProviderInitError`** — Likely an invalid or corrupted config. Re-authenticate with `/connect`.
- **API call errors after a provider update** — Force-refresh cached provider packages: `opencode upgrade --force`.
- **Copy/paste not working** — Linux needs a clipboard utility (`xclip`, `xsel`, or `wl-clipboard`).

## 6. Uninstall

OpenCode includes a built-in uninstall command that cleans up everything:

```bash
opencode --uninstall
```

Or manually:

```bash
# Remove the binary (location depends on install method)
rm -rf ~/.opencode

# Remove configs and data
rm -rf ~/.config/opencode
rm -rf ~/.local/share/opencode
```
