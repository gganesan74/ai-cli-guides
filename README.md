# AI Coding CLI Install Guides (Linux)

Step-by-step install and configuration guides for the three most popular terminal AI coding agents: **Claude Code**, **Codex**, and **OpenCode**.

Each tool has its own directory with a self-contained guide.

## Repository layout

```
ai-cli-guides/
├── README.md                  # this file
├── claude-code/
│   └── INSTALL.md             # Claude Code (Anthropic)
└── opencode/
    └── INSTALL.md             # OpenCode (open source, multi-provider)
```

## Quick comparison

| Tool        | Vendor    | Install method (recommended)              | Config format | User config path                     | Project instructions |
|-------------|-----------|-------------------------------------------|---------------|--------------------------------------|----------------------|
| Claude Code | Anthropic | `curl -fsSL https://claude.ai/install.sh \| bash` | JSON          | `~/.claude/settings.json`            | `CLAUDE.md`          |         |
| OpenCode    | sst / community | `curl -fsSL https://opencode.ai/install \| bash` | JSON / JSONC | `~/.config/opencode/opencode.json`   | `AGENTS.md`          |

## Prerequisites (shared)

Most of these install most cleanly with **Node.js 18+**. The cleanest way to get Node on Linux without `sudo` headaches is `nvm`:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/master/install.sh | bash
# Restart your shell, then:
nvm install --lts
node --version  # should show v18 or higher
```

If you already have a system Node, **don't use `sudo` for global installs**. If you hit `EACCES` errors, set up a user-owned npm prefix once:

```bash
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

## Where to start

- **New to terminal AI agents?** Start with `claude-code/INSTALL.md` — Anthropic's native installer is the most painless.
- **Want maximum flexibility / open source / multi-provider?** Start with `opencode/INSTALL.md`.

You can install both side-by-side without conflicts — they use entirely separate config directories.

## A note on `AGENTS.md` vs `CLAUDE.md`

OpenCode reads `AGENTS.md` from your project root for project-specific instructions. Claude Code reads `CLAUDE.md`. If you use multiple tools in one repo, you'll likely end up with both files. They can contain similar guidance — most teams keep them in sync manually or symlink one to the other.
