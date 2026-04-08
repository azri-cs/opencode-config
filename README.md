# OpenCode configuration

This repository is a **sanitized snapshot** of how [OpenCode](https://opencode.ai) is configured on my machine. It mirrors the layout of `~/.config/opencode/` so others can copy ideas, agents, commands, and skills without picking up API keys, tokens, or other secrets.

Nothing here is guaranteed to match upstream OpenCode behavior forever; treat it as a reference you adapt for your own environment.

## What is included

| Path | Purpose |
|------|---------|
| [`opencode.json`](opencode.json) | Main config: default agent, permissions, MCP servers, compaction, file watcher ignores, and global instruction paths. Uses the [OpenCode config schema](https://opencode.ai/config.json). |
| [`AGENTS.md`](AGENTS.md) | Global instructions loaded via `instructions` in `opencode.json`. |
| [`agent/`](agent/) | Agent definitions (roles, prompts, workflows). |
| [`command/`](command/) | Custom slash commands. |
| [`skills/`](skills/) | Reusable skills (`SKILL.md` per skill). |
| [`templates/`](templates/) | Stack-specific agent templates (e.g. React, Laravel, Django). |

## Using this repo

1. **Clone or browse** and copy only the pieces you want into your local OpenCode config directory (typically `~/.config/opencode/` on Linux/macOS).

2. **Merge carefully** if you already have `opencode.json`: permissions, MCP blocks, and `external_directory` rules are opinionated and may conflict with yours.

3. **Adjust paths** in `opencode.json` to match your machine. Examples in this snapshot:
   - `permission.external_directory` may allow specific roots (e.g. under `/projects`).
   - `instructions` may reference `~/.config/opencode/AGENTS.md` or a path inside this repo after you copy it.
   - MCP servers such as `filesystem` pass a root path to the MCP server; change it to directories you are willing to expose.

## Secrets and sensitive values

Published configs intentionally **omit real credentials**. In `opencode.json` you will see placeholders such as:

- `GITHUB_PERSONAL_ACCESS_TOKEN` / `YOUR_GITHUB_PERSONAL_ACCESS_TOKEN`
- `CONTEXT7_API_KEY` / `YOUR_CONTEXT7_API_KEY`

Replace those with your own secrets via environment variables or your preferred secret manager, and **never commit** real tokens.

Before you publish or share your own fork of a config like this:

- Search for API keys, tokens, private URLs, internal hostnames, and personal paths.
- Review MCP `environment` and `headers` blocks.
- Confirm `bash` allow-lists do not encode private workflows you do not want to expose.

## License

This repository is released under the [MIT License](LICENSE).
