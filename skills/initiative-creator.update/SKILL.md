# Initiative Creator Update Skill

Pull the latest skill files from the initiative-creator GitHub repo and sync them into the local workspace. Reports exactly what changed.

All output must be in English.

---

## Step 1 — Pull Latest from GitHub

Run the following shell command from the repo root:

```bash
git pull origin main
```

Capture the output.

**If "Already up to date."** — tell the user and stop:
```
✅ Already up to date. No changes to sync.
```

**If there are changes** — continue to Step 2.

**If git pull fails** (network error, merge conflict, etc.):
```
⚠️ git pull failed.

Error: {error message}

You can update manually:
  cd ~/Projects/initiative-creator
  git pull origin main

Then re-run /initiative-creator.update
```
Stop immediately.

---

## Step 2 — Show What Changed

Parse the git pull output and list which skill files were updated:

```
📦 Updates pulled from GitHub
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Branch: main
Commit: {short hash} — {commit message}

Files changed:
  • skills/initiative.create/SKILL.md
  • skills/initiative.submit/SKILL.md
  [or: "No skill files changed in this pull"]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Skills are now up to date in skills/
```

---

## Step 3 — Sync to Other Workspaces (Optional)

If the user runs this command with the argument `--sync-kb`, also sync the updated files to the knowledge base at `~/Projects/abe-personal-knowledge-base` using the path mapping:

```
skills/{name}/SKILL.md → .cursor/skills/initiative-creator/{short-name}/SKILL.md
```

After copying, fix path references (replace `skills/` prefixes with `.cursor/skills/initiative-creator/` and `guidelines/` with workspace-specific paths).

Report each file synced.

---

## Arguments

`$ARGUMENTS` — Optional. Pass `--sync-kb` to also sync files to the knowledge base workspace.
