# Pull Requests

- Do not include a "Test plan" section in PR descriptions.
- When merging a PR, do **not** squash by default. Use `gh pr merge <n> --merge --delete-branch` (or `--rebase` for a linear history) so the individual commits on the branch are preserved on `main`. Only squash if the user explicitly asks for it on a specific PR.

# Commits

- Prefer tangible, meaningful commits — each one a self-contained idea with a meaningful subject. Not 500-line dumps that collapse multiple unrelated changes; not "fix typo / fix typo / fix typo" chains. A good commit answers "what changed and why" in its own subject line.
- If a branch has accumulated noise (WIP, fixups, typos) before opening a PR, clean it up with an interactive rebase first. Never rewrite already-pushed shared history without explicit authorization.

# Project conventions

- If `AGENTS.md` exists in the project root, read it at the start of the session and follow its instructions for the rest of the conversation. Treat it as project-specific guidance that supplements (and where it conflicts, overrides) generic defaults. If both `AGENTS.md` and `CLAUDE.md` exist in the project root, read both and follow both — they are additive.
