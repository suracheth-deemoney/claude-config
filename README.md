# claude-config

Shareable global configuration for [Claude Code](https://claude.com/claude-code).

This repo holds the portable bits of `~/.claude/` (settings, agents, commands, skills, hooks, top-level `CLAUDE.md`, etc.) so the same setup can be reused across machines. It does **not** contain credentials, session history, caches, or per-project state.

## Setup

Clone the repo somewhere stable (the path is referenced by symlinks, so don't move it after):

```bash
git clone <this-repo> ~/configs/claude-config
```

Back up your existing `~/.claude/` before linking anything:

```bash
mv ~/.claude ~/.claude.backup
```

Create `~/.claude/` and symlink the tracked items from this repo into it:

```bash
mkdir -p ~/.claude
cd ~/.claude

# link each tracked file/dir from the repo
for item in CLAUDE.md settings.json agents commands skills hooks memory; do
  [ -e ~/configs/claude-config/$item ] && ln -s ~/configs/claude-config/$item $item
done
```

Restore any machine-local items you want to keep from the backup (e.g. `.credentials.json`, `projects/`, `sessions/`, `settings.local.json`):

```bash
cp -a ~/.claude.backup/.credentials.json ~/.claude/ 2>/dev/null || true
cp -a ~/.claude.backup/settings.local.json ~/.claude/ 2>/dev/null || true
```

Verify the links:

```bash
ls -la ~/.claude
```

## What goes in this repo

Tracked (safe to share):

- `CLAUDE.md` — global user instructions
- `settings.json` — global Claude Code settings
- `agents/` — custom subagent definitions
- `commands/` — custom slash commands
- `skills/` — custom skills
- `hooks/` — hook scripts
- `memory/` — long-lived memory entries (review before committing — may contain personal context)

Never tracked (machine-local or sensitive):

- `.credentials.json`, `policy-limits.json`
- `settings.local.json` — per-machine overrides
- `history.jsonl`, `sessions/`, `projects/`, `tasks/`, `plans/`
- `cache/`, `downloads/`, `paste-cache/`, `shell-snapshots/`, `session-env/`, `file-history/`, `backups/`, `debug/`, `telemetry/`
- `mcp-needs-auth-cache.json`

These are covered by `.gitignore`.

## Updating config

Edit files in this repo (or in `~/.claude/` — the symlinks point back here), then commit and push:

```bash
cd ~/configs/claude-config
git add -A
git commit -m "update: <what changed>"
git push
```

On another machine, `git pull` is enough — the symlinks pick up the new content automatically.

## Removing the setup

```bash
rm -rf ~/.claude
mv ~/.claude.backup ~/.claude
```
