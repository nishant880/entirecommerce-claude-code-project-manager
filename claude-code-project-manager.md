# Claude Code Project Manager: Master Prompt

> Paste this entire file into Claude Code from a project folder. Claude will interview you briefly, create `CLAUDE.md` and `actions.md`, seed the Recurring block, and optionally wire the `ec-kanban-core` plugin's SessionStart hook.
>
> Alternative fastest path: paste this document's URL into Claude Code and say *"read this and take me through it step by step"*. Claude reads the document directly and runs Step 1 through Step 8 without any manual setup.
>
> Built by Nishant Kapoor at EntireCommerce AI. Version 1, April 2026.

---

You are installing the Claude Code Project Manager system in the current working directory. Follow these steps in order. Do not skip steps. Do not invent data. Honour the voice conventions in Step 7.

---

## Step 1. Load or populate the project brief

Look for `CLAUDE.md` in the current working directory.

**If `CLAUDE.md` exists** and contains a project description (even a thin one), read it and add the Project Management section at the bottom of the existing file. Do not overwrite the user's existing content.

**If `CLAUDE.md` does not exist**, create it from the template in this bundle's `CLAUDE.md` file and populate it via the interview below.

### The project-brief interview

Ask the user the following questions conversationally, one batch at a time. Write answers into `CLAUDE.md` progressively. Confirm after each batch.

**Batch 1: Project overview.** What is this project (one sentence). Who is it for. What stage is it at (pre-launch, first year, established, mature). Primary URL if it has one.

**Batch 2: Wired-up tools.** Which tools, APIs, or services does this project depend on. Which credentials live in `.env` or elsewhere. Which ones does Claude have access to through MCP servers.

**Batch 3: Active work in flight.** What are you actively working on right now. Anything urgent. Anything blocked. This populates the initial P0 tier.

**Batch 4: On-deck work.** What's next after the in-flight items. These become P1 and P2.

**Batch 5: Recurring work.** What cycles in this project. Weekly reviews. Monthly audits. Quarterly refreshes. Daily standups. Who runs each one (you, Claude, or both).

**Batch 6: Operating rules.** Anything Claude should always do or never do on this project. Brand voice rules. Banned phrases. Commit conventions. Deployment rules.

### Minimum required to proceed to Step 2

Three fields must be captured before moving on:

1. Project one-sentence description
2. At least one P0, P1, or P2 item
3. Whether any recurring work exists (can be "none" explicitly)

If the user says "I don't know" to one of these, flag it and explain the system needs at least a placeholder. Do not invent values.

---

## Step 2. Inventory environment

Check the following and print the result to the user:

```
Environment check
- Claude Code: {version or "not found"}
- Python 3.9+: {version or "not found"}
- git in this folder: {repo found / not a git repo}
- Existing CLAUDE.md: {found and has content / exists but thin / not found}
- Existing actions.md: {found / not found}
- ~/.claude/settings.json: {exists / not found}
```

If git is not initialised in the folder, ask the user if you should run `git init`. The system assumes git-tracked markdown files.

If Python 3.9+ is missing and the user wants Path B (plugin), flag it: the SessionStart hook requires Python 3.9 or newer.

---

## Step 3. Write CLAUDE.md

If `CLAUDE.md` does not exist, create it using the template in the bundle's `CLAUDE.md` file as the starting structure. Populate every section from the Step 1 interview answers.

If `CLAUDE.md` exists, append a `## Project Management` section at the bottom containing:

- The priority rules (P0 max 5, P1 max 10, P2 max 15, P3+ unlimited)
- The task-owner tag vocabulary (`[Claude]`, `[Nishant]`, `[Hybrid]`); adapt `[Nishant]` to the user's own name
- The ICE scoring formula with weighted effort (80% user effort, 20% Claude effort)
- The Recurring-tasks schema (line format, cadence vocabulary)
- The natural-language triggers table (five phrases)

Do not clobber existing content. Do not rewrite the user's own project brief. Only add the Project Management section if it isn't already present.

---

## Step 4. Write actions.md

Create `actions.md` in the project root with the following structure:

```markdown
# {Project name}: Actions

> Priority rules: P0 ≤ 5, P1 ≤ 10, P2 ≤ 15, P3+ unlimited.
> Tags: [Claude], [Nishant] (or user's name), [Hybrid].
> ICE = Impact × Confidence × Weighted Ease. Weighted Ease = 0.8 × User_ease + 0.2 × Claude_ease.

---

## Recurring

{Seeded from Batch 5. Group by cadence. If user had no recurring work, leave the section with a single comment line: "No recurring items yet. Add them here as they emerge."}

---

## Next Actions

### P0 (this week)

{Seeded from Batch 3. One line per item with tag + ICE-scored description.}

### P1 (next 2 weeks)

{Seeded from Batch 4 if provided.}

### P2 (later)

{Seeded from Batch 4 overflow.}

### P3+ (backlog)

_(none)_

---

### [Claude] queue (awaiting approval, run-and-done, no tier slot)

_(none yet)_

---

## Completed

_(none yet)_
```

Every item gets an ICE score in a comment or inline tag. Example:

```
- [Hybrid] Refactor onboarding flow to reduce dropoff (ICE: I=8, C=7, N_ease=3, C_ease=6 → Weighted=3.6 → 201.6)
```

The ICE numbers can sit in the item line or in a comment block above it, whichever the user prefers visually. Ask.

---

## Step 5. Offer the plugin install (Path A / Path B)

Ask the user:

> "Do you want the `ec-kanban-core` plugin installed? It ships a SessionStart hook that auto-surfaces due recurring work at the top of every session. Recommended if you have recurring items and will use this across more than one project. Skip if you want to keep the install manual-only for now."

### Path A (skip)

Confirm the manual install is complete. Point the user at the natural-language triggers in `CLAUDE.md` and exit.

### Path B (install)

Continue to Step 6.

---

## Step 6. Wire the SessionStart hook

Two parts.

### 6a. Install the hook script

Ask the user where they want the plugin to live:

- **Option 1 (recommended):** inside the current project at `plugins/ec-kanban-core/`. Best if this project is a "master" workspace.
- **Option 2:** a shared location like `~/.claude-plugins/ec-kanban-core/`. Best if the user runs Claude Code across many unrelated project folders.

Copy the plugin files (manifest, hook script, hooks.json, README) from this bundle's plugin reference into the chosen location. The hook script is `recurring-due-check.py`.

### 6b. Wire the hook into ~/.claude/settings.json

Read `~/.claude/settings.json`. Add a `hooks` block (or extend the existing one) with:

```json
{
  "hooks": {
    "SessionStart": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "python3 /absolute/path/to/recurring-due-check.py",
            "timeout": 15
          }
        ]
      }
    ]
  }
}
```

Use the absolute path to where the hook script ended up in Step 6a. Validate the resulting JSON with `python3 -c "import json; json.load(open('/Users/{user}/.claude/settings.json'))"`.

If a `hooks.SessionStart` block already exists (e.g., from another plugin), add this hook to the same array without removing existing entries.

---

## Step 7. Voice conventions (enforcement gate)

Before committing the files created in Steps 3 and 4, run a grep-pass against the new content:

```bash
grep -nE $'—|, not | rather than | instead of |opposite of |Here\'s the|Most people|e-commerce' {CLAUDE.md,actions.md}
```

Fix every hit. No em dashes. No negative parallelisms. No AI clichés. No "e-commerce" (use "ecommerce" one word). No one-word or two-word sentences.

If the user has their own voice rules (captured in Batch 6), add them to the grep pattern too.

---

## Step 8. Verify, commit, and exit

### 8a. Verify

Run the hook manually to confirm it works:

```bash
CLAUDE_PROJECT_DIR=$(pwd) python3 /absolute/path/to/recurring-due-check.py </dev/null
```

Expected output: a JSON payload with `hookSpecificOutput.additionalContext` listing any overdue or upcoming recurring items. If none are due, the script exits silently with empty output.

### 8b. Commit

Stage and commit the new files:

```bash
git add CLAUDE.md actions.md plugins/ec-kanban-core/
git commit -m "install claude-code-project-manager playbook v1"
```

Push if a remote is configured.

### 8c. Exit

Print a summary to the user:

- Files written: `{list}`
- Plugin installed: `{yes/no}`
- Hook wired in `~/.claude/settings.json`: `{yes/no}`
- Next steps: restart Claude Code to see the SessionStart reminder; use "what's next?" to trigger the triage pass; use "let's wrap up" at end of every session.

---

## Acceptance criteria

The install is complete when:

1. `CLAUDE.md` exists in the project folder and contains the Project Management section with priority rules, tags, ICE formula, Recurring schema, and natural-language triggers.
2. `actions.md` exists in the project folder with Recurring, Next Actions (P0/P1/P2/P3), Claude approval section, and Completed sections.
3. If Path B: the SessionStart hook is wired in `~/.claude/settings.json` with an absolute path that resolves to an executable Python script.
4. Voice gate passes (grep returns no hits against the banned-pattern list).
5. Files are committed to git.

If any criterion fails, report which one and stop. Do not ship a partial install.

---

## Honest caveats

Mention these during the install if relevant:

1. The Recurring hook fires only at session start. Long sessions should re-trigger manually.
2. Monthly cadence is approximated as +30 days. Calendar-month precision is a v0.2.0 enhancement.
3. Plugin is v0.1.0; slash commands (`/reprioritise`, `/skip-recurring`, etc.) roadmapped for future versions.
4. System assumes Claude Code is the primary interface. Team members using other Anthropic surfaces lose continuity.

---

## Version history

- **v1, April 2026.** First release. Ships with manual install and v0.1.0 plugin (SessionStart hook only).
