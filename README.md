# claude-config

Shareable global configuration for [Claude Code](https://claude.com/claude-code).

This repo holds the portable bits of `~/.claude/` (settings, agents, commands, skills, hooks, top-level `CLAUDE.md`, etc.) so the same setup can be reused across machines. Runtime and sensitive files (credentials, session history, caches, per-project state) live in the same directory at runtime but are excluded by `.gitignore` and never committed.

## Setup

Clone the repo somewhere stable (the path becomes the symlink target — don't move it after):

```bash
git clone git@github.com:suracheth-deemoney/claude-config.git ~/configs/claude-config
```

Back up your existing `~/.claude/` (so you can recover credentials and any machine-local files):

```bash
mv ~/.claude ~/.claude.backup
```

Symlink `~/.claude` to the repo:

```bash
ln -s ~/configs/claude-config ~/.claude
```

Restore the machine-local items you want to keep from the backup. These live inside `~/.claude/` (which now points at the repo) but are gitignored, so they won't be committed:

```bash
cp -a ~/.claude.backup/.credentials.json     ~/.claude/ 2>/dev/null || true
cp -a ~/.claude.backup/settings.local.json   ~/.claude/ 2>/dev/null || true
cp -a ~/.claude.backup/projects              ~/.claude/ 2>/dev/null || true
cp -a ~/.claude.backup/sessions              ~/.claude/ 2>/dev/null || true
cp -a ~/.claude.backup/history.jsonl         ~/.claude/ 2>/dev/null || true
```

Verify:

```bash
ls -la ~/.claude    # should show: ~/.claude -> ~/configs/claude-config
cd ~/.claude && git status   # should be clean (runtime files are ignored)
```

> **Note:** Because `~/.claude` now resolves to the repo working tree, your credentials and session data sit on disk inside that directory. Keep the repo directory permissions private (the clone inherits your umask; `chmod 700 ~/configs/claude-config` if unsure). The repo itself stays safe to push publicly — `.gitignore` blocks the sensitive files.

## What goes in this repo

Tracked (safe to share):

- `CLAUDE.md` — global user instructions
- `settings.json` — global Claude Code settings
- `agents/` — custom subagent definitions
- `commands/` — custom slash commands
- `skills/` — custom skills
- `hooks/` — hook scripts
- `memory/` — long-lived memory entries (review before committing — may contain personal context)

Never tracked (machine-local or sensitive — see `.gitignore`):

- `.credentials.json`, `policy-limits.json`, `mcp-needs-auth-cache.json`
- `settings.local.json` — per-machine overrides
- `history.jsonl`, `sessions/`, `projects/`, `tasks/`, `plans/`
- `cache/`, `downloads/`, `paste-cache/`, `shell-snapshots/`, `session-env/`, `file-history/`, `backups/`, `debug/`, `telemetry/`, `plugins/`

## Updating config

Edit files in `~/.claude/` (which is the repo). Commit and push from either path:

```bash
cd ~/.claude
git add -A
git commit -m "update: <what changed>"
git push
```

On another machine, `git pull` is enough — Claude Code picks up the new content automatically.

## Removing the setup

```bash
rm ~/.claude              # removes the symlink, not the repo
mv ~/.claude.backup ~/.claude
```
