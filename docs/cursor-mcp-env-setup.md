# Cursor MCP environment setup

This guide explains how to provide secrets for the user-level Cursor MCP config at `~/.cursor/mcp.json` without hardcoding tokens into config files.

## What Cursor expects

The generated Cursor MCP config uses these placeholders:

- `GITHUB_PERSONAL_ACCESS_TOKEN`
- `CONTEXT7_API_KEY`

Cursor must be launched from an environment where these variables are already defined, or those MCP servers may fail to authenticate.

## Recommended approach on Windows + WSL2

Because you run Cursor against WSL2 projects, the simplest approach is:

1. Store the secrets in your WSL shell startup files.
2. Start Cursor from a WSL shell session that already has those variables loaded.
3. Restart Cursor after changing the variables.

## Option A: put variables in `~/.zshrc` (simple)

Add this to your WSL user shell config:

```bash
export GITHUB_PERSONAL_ACCESS_TOKEN="your_github_pat_here"
export CONTEXT7_API_KEY="your_context7_api_key_here"
```

Then reload your shell:

```bash
source ~/.zshrc
```

Verify:

```bash
printf '%s\n' "$GITHUB_PERSONAL_ACCESS_TOKEN" | wc -c
printf '%s\n' "$CONTEXT7_API_KEY" | wc -c
```

You should see non-zero lengths.

## Option B: keep secrets in a separate file (cleaner)

Create a private env file such as `~/.config/opencode/secrets.env`:

```bash
export GITHUB_PERSONAL_ACCESS_TOKEN="your_github_pat_here"
export CONTEXT7_API_KEY="your_context7_api_key_here"
```

Then source it from `~/.zshrc`:

```bash
[ -f ~/.config/opencode/secrets.env ] && source ~/.config/opencode/secrets.env
```

This keeps your main shell config cleaner and makes rotation easier.

## Launching Cursor with the variables loaded

After your shell exports the variables, start Cursor from that shell if needed so it inherits the environment.

Examples:

```bash
cursor .
```

or if using the Windows-installed Cursor command bridged into WSL, launch it the same way you normally do from a shell session that already has the variables.

If Cursor was already open before the variables existed, fully close and reopen it.

## Verify inside WSL before reopening Cursor

Run:

```bash
env | grep '^GITHUB_PERSONAL_ACCESS_TOKEN='
env | grep '^CONTEXT7_API_KEY='
```

If both appear, your shell environment is ready.

## Token guidance

### GitHub PAT

Use a GitHub Personal Access Token with the smallest permissions you need.

Typical minimum for repo/PR workflows:
- repository access for the repos you use
- pull request / issue access as needed

Avoid overly broad classic tokens if a fine-grained token works for your use case.

### Context7 API key

Use your existing Context7 key. Treat it like any other secret and do not commit it to any repo.

## Troubleshooting

### Cursor MCP server still fails

Check:
- Cursor was fully restarted
- the shell you launched from has the variables set
- the variable names exactly match:
  - `GITHUB_PERSONAL_ACCESS_TOKEN`
  - `CONTEXT7_API_KEY`
- `~/.cursor/mcp.json` still contains placeholders and was not manually edited to invalid JSON

### Variables exist in shell but not in Cursor

This usually means Cursor was started before the variables were loaded, or it was launched from a context that did not inherit your WSL shell environment.

Fix:
1. `source ~/.zshrc`
2. confirm with `env | grep`
3. fully quit Cursor
4. relaunch Cursor from the ready shell session

## Security notes

- Never commit real tokens into `~/.cursor/mcp.json`, `opencode.json`, or this repo.
- Prefer a separate sourced secrets file over storing secrets directly in tracked files.
- Rotate tokens if they were ever committed or exposed.
