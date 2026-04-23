<div align="center">

---

### **[Want us to install this on your workflow? Book a call →](https://entirecommerce.ai/book-a-call)**

---

</div>

# Claude Code Project Manager Playbook

## A persistent project-management operating system you install inside Claude Code in 30 minutes. Runs a Kanban board across every project you touch, with strict WIP caps, ICE-scored prioritisation, task-owner tags, and a Recurring-tasks layer that auto-surfaces overdue work at session start. Two files per project (`CLAUDE.md` and `actions.md`) plus one optional plugin (`ec-kanban-core`) give you continuity across sessions, clarity on what to work on next, and a natural-language interface to the whole system. Built by Nishant Kapoor at EntireCommerce AI.

---

<div align="center">

### Fastest way to use this playbook

**Paste this document's URL into Claude Code and say:**

> *"Read this and take me through it step by step."*

**Claude will interview you about your projects, create the files, seed the Recurring block, and wire the SessionStart hook. You don't need to read the rest of this document.**

</div>

---

## Who This Playbook Is For

This playbook is for three people.

**The solo founder running multiple workstreams.** You're the CEO, the marketer, the product person, the customer-support lead, and the bookkeeper. You have a project for the product, a project for marketing, a project for the agency's own website, a project for the investor deck, and four half-projects you keep meaning to come back to. You open Claude Code and ask it to help, and every session feels like starting from scratch. You want a system where Claude knows what you're working on, what it's helped you ship, and what the five most important things are this week.

**The agency operator running multi-client work.** You're running Claude Code across five to ten client folders. You want one consistent system that covers every client, so context doesn't evaporate between sessions and nothing critical slips through. You want a weekly review that takes an hour and stops eating half a day. You want recurring audits (the monthly CLAUDE.md review, the quarterly GTM refresh) to surface themselves at the right time without relying on your memory.

**The engineering lead or solo developer with side projects.** You have a main codebase and three side projects in various stages of half-done. You want to stop losing context when you switch. You want each project to have its own board with its own priorities, and you want a single trigger word ("what's next?") that tells you which of the 12 open tickets you should pick up in the next 90 minutes. You want Claude to be the project manager so you can be the one doing the work.

If you open Claude Code and find yourself typing the same context paragraph every time ("I'm building a Shopify theme for a jewellery brand, we have a spam-bot problem, the theme repo is at X, please don't push to the live theme directly"), this playbook collapses all of that into a persistent `CLAUDE.md` that loads automatically. If you've got a Notion board you never update, a to-do app with 400 overdue items, and three Google Docs that are meant to be the source of truth, this playbook collapses those into one markdown file per project that Claude reads, writes, and commits to git alongside your code.

---

## What The Claude Code Project Manager Actually Is

**The short version.** Two files per project, one Claude Code plugin, a strict Kanban discipline on top, and a handful of natural-language triggers. `CLAUDE.md` holds the persistent context for a project (what it is, who it's for, wired-up APIs, brand voice, operating rules). `actions.md` holds the live work state: a Recurring section at the top, a Kanban board in the middle (P0 max 5, P1 max 10, P2 max 15), and a Completed log at the bottom. Every item carries an ICE score, a task-owner tag (`[Claude]`, `[Nishant]`, `[Hybrid]`), and optionally a back-reference tag if it came from the Recurring register. The `ec-kanban-core` plugin installs a SessionStart hook that surfaces due recurring work automatically at the top of every session. A small English-phrase vocabulary ("what's next?", "let's wrap up", "skip this week's review") maps to the behaviours that run the system.

**Best for.** Operators who use Claude Code regularly across more than one project, value continuity between sessions, and want a forcing function against perpetual to-do-list bloat. Works equally well for solo operators, agencies, and small teams.

**How it's different.** Three things most project-management systems skip.

First, **Claude is the operator of the board**. Every session ends with a reprioritisation pass. Every session starts with a triage pass. The board stays honest because Claude is running it on every touch. Traditional project managers rely on human discipline to reopen the tool, review the backlog, and re-rank. This system removes that discipline tax by making prioritisation the default session behaviour.

Second, **effort is weighted 80/20 between the human operator and Claude** in the ICE score. Claude hours are effectively free at the margin. The 80/20 weighting surfaces work that draws minimally on operator time, which is the actual binding constraint. A task that takes you five minutes of review and Claude four hours of execution sits near the top of the board. A task that takes you a two-hour meeting and Claude five minutes of prep slides down. The board naturally biases toward leverage.

Third, **recurring tasks compete for P0 slots on merit**. Weekly reviews, monthly audits, quarterly refreshes all sit in a Recurring register. A SessionStart hook surfaces them when due. When surfaced, they ICE-compete against the current P0 queue. If a monthly CLAUDE.md audit genuinely outranks an in-flight client deliverable, it earns the slot (and the weaker item drops to P1). If it doesn't, that's information: the recurring work is overdue but the bottleneck this cycle lives elsewhere. You decide explicitly to defer or pull it in. Nothing slips silently.

**Skip this playbook if** you work on a single project with short, well-defined tasks (your existing to-do list works), you prefer UI-driven project managers like Asana or Linear as your source of truth (this system lives in markdown files), or you don't use Claude Code at all (the SessionStart hook is the forcing function).

---

## Setup (15 Minutes)

### Prerequisites

Before you start, make sure you have:

- **Claude Code** installed. Download from [claude.com/claude-code](https://claude.com/claude-code). Verify with `claude --version`.
- **A Claude subscription** (Pro or Max) so Claude Code can run sessions. Free-tier API access also works; the playbook is platform-agnostic.
- **git** installed and your project folder under version control. The system writes to `CLAUDE.md` and `actions.md` on every session, and git history becomes the audit trail.
- **Python 3.9 or newer** on the machine where Claude Code runs. Needed for the SessionStart hook. Verify with `python3 --version`. macOS and most Linux distros ship this by default.

### Cost overview

The playbook itself is free. Costs are whatever you already spend on Claude Code (Pro, Max, or API usage for sessions). No external API keys. No paid tools. No SaaS subscriptions.

| Tool | Subscription model | Typical cost | Role |
|---|---|---|---|
| Claude Code (Pro / Max / API) | Monthly subscription or pay-as-you-go | You already pay this | Runs the sessions that read, write, and reprioritise `actions.md`. |
| Python 3.9+ | Free | $0 | Runs the SessionStart hook that parses recurring items. |
| git + a git host (GitHub, GitLab, etc.) | Free tier usually sufficient | $0 | Version-controls `CLAUDE.md` and `actions.md`. Git is the audit trail. |
| pandoc | Free | $0 | Optional. Only needed if you want to export reviews as Word docs. |

**Typical total cost: $0 on top of your existing Claude Code subscription.**

### Step 1: Pick a project folder and open Claude Code inside it

```bash
cd ~/my-project
claude
```

If the folder already has a `CLAUDE.md`, great. If not, the master prompt in Step 3 will create one for you.

**Gotcha I hit:** make sure the folder is a git repository (`git status` works). If it isn't, run `git init` first. Otherwise `actions.md` changes don't persist as a version-controlled trail.

### Step 2: Decide whether you want the plugin

Two install paths.

**Path A (manual, no plugin).** You edit `CLAUDE.md` and `actions.md` by hand (or have Claude do it for you via chat). The Recurring block exists but nothing surfaces it automatically. You remember to check it, or you ask Claude "anything due this week?" and it parses the file on demand. Works fine for one or two projects. Lightweight.

**Path B (plugin, recommended for more than one project).** Install `ec-kanban-core`, a small Claude Code plugin that ships a SessionStart hook. The hook parses every `actions.md` in the project tree at session start, computes which recurring items are due, and injects a compact reminder at the top of the session. Zero friction once installed; the board surfaces overdue work without being asked.

The master prompt in Step 3 asks you which path you want and sets up the right one. Default is Path B because the forcing function is the whole point of the Recurring layer.

### Step 3: Paste the master prompt into Claude Code

Open `claude-code-project-manager.md` (the lean master prompt in this bundle) and paste its full contents into Claude Code. Hit enter. Or paste the Google Doc URL of this bundle and say:

> *"Read this and take me through it step by step."*

Claude will interview you briefly about the project, write `CLAUDE.md` and `actions.md` with your answers, offer to wire the plugin (Path B), and leave you with a working system.

**Gotcha I hit:** if you're installing across multiple projects at once, don't paste the prompt in each one separately. Run it in your highest-leverage project first, review the output, then let Claude apply the same pattern across your other project folders in batch. The playbook supports multi-project install via `"apply this to every project folder under ~/Library/CloudStorage/Dropbox/Projects/"` or similar.

---

## The Five Files in This Bundle

### File 1: README.md (this file)

**Path:** `{your-project-folder}/README.md` (or wherever you drop the bundle)
**What it does:** You're reading it. Setup, run, and troubleshooting reference. Safe to archive once the system is installed.
**How to use it:** Read once, paste the URL into Claude Code for the first run, then archive. The living docs are `CLAUDE.md` and `actions.md`. This README is reference only.

### File 2: CLAUDE.md (project-brief template)

**Path:** `{your-project-folder}/CLAUDE.md`
**What it does:** The persistent project brief Claude Code loads on every session start. Contains what the project is, who it's for, operating rules, and pointers to wired-up tools.
**How to use it:** Option A: let Claude interview you during the master-prompt run. Option B: replace every `{placeholder}` yourself before running the master prompt.

### File 3: claude-code-project-manager.md (the master prompt)

**Path:** `{your-project-folder}/claude-code-project-manager.md` (or paste directly into Claude Code)
**What it does:** The prompt Claude executes end-to-end to install the system. Interviews you, creates `CLAUDE.md` and `actions.md`, seeds the Recurring block, wires the optional plugin, and explains the natural-language triggers.
**How to use it:** Paste its contents into Claude Code and hit enter. Or paste the Google Doc URL. Don't edit the prompt unless you know what you're doing.

### File 4: ec-kanban-core plugin (optional)

**Path:** installed under `~/.claude/plugins/` (via the marketplace) or referenced by absolute path in `~/.claude/settings.json`
**What it does:** Ships a SessionStart hook (`recurring-due-check.py`) that parses `## Recurring` sections across every `actions.md` in the project tree and surfaces due items at the top of the session.
**How to use it:** The master prompt offers to install it during setup. If you decline, you can install it later via the install guide at the end of this README.

### File 5: linkedin-claude-code-project-manager-promo.md (distribution)

**Path:** `playbooks/claude-code-project-manager-bundle/linkedin-claude-code-project-manager-promo.md`
**What it does:** Pre-written LinkedIn post that explains the playbook and drives DM-subscribers. Lead-magnet copy for the agency's distribution loop.
**How to use it:** Ignore if you're using the playbook yourself. Relevant if you're an EntireCommerce partner or want to share the playbook with your audience.

---

## Your First Run

Four steps. Roughly 15 to 30 minutes.

1. **Open Claude Code in your project folder.** `cd ~/my-project && claude`
2. **Paste the full contents of `claude-code-project-manager.md` into the terminal.** Or paste the Google Doc URL and say *"read this and take me through it step by step"*.
3. **Answer the interview.** Claude will ask a short set of questions: what's the project, who's it for, what tools are wired up, what's actively in flight right now (for the P0 tier), what recurring work exists (weekly reviews, monthly audits, etc.), whether you want the plugin installed. Takes 10 to 20 minutes depending on how many recurring items you surface.
4. **Confirm the install.** Claude prints a summary of what it wrote: `CLAUDE.md`, `actions.md`, optional plugin wiring in `~/.claude/settings.json`. You review, confirm, and the system is live.

After the first session, the board is yours. On session two, you say "what's next?" and Claude surfaces due recurring items, ICE-ranks them against your P0 queue, and tells you what to pick up. Ending sessions with "let's wrap up" triggers the reprioritisation pass and a git push.

---

## What You'll Get

Eight concrete deliverables at the end of the install.

1. **A populated `CLAUDE.md`** for your project. Holds project context, operating rules, wired-up tools, and brand voice. Loads on every session start.
2. **A seeded `actions.md`** with your current work organised into P0 / P1 / P2 tiers, each item ICE-scored and tagged `[Claude]` / `[Nishant]` / `[Hybrid]`. Blockers auto-promoted to P0.
3. **A `## Recurring` section** above Next Actions, with your weekly / monthly / quarterly workstreams listed with `cadence` and `last_run` metadata.
4. **An optional `ec-kanban-core` plugin installation**, with the SessionStart hook wired to `~/.claude/settings.json`. Surfaces overdue recurring work at the top of every future session.
5. **A natural-language trigger vocabulary** documented in your `CLAUDE.md` so Claude responds consistently across sessions. "What's next?" runs the triage pass. "Let's wrap up" triggers the reprioritisation pass. "Skip the weekly review this cycle" advances `last_run` without running the task.
6. **A persistence model** (`CLAUDE.md` for context, `actions.md` for work state, git for history) that keeps continuity across every future session on the project.
7. **A Completed log** at the bottom of `actions.md` that becomes an audit trail of shipped work, date-stamped, ready for weekly-review narratives and year-end retrospectives.
8. **A multi-project pattern** (if you run the master prompt across more than one folder) that gives every project the same structure, same triggers, same discipline. The five English phrases work the same everywhere.

Everything is plain markdown, version-controlled in git, editable with any text editor. No lock-in.

---

## Modes

The playbook adapts to the intensity of the project. Three modes:

| Mode | When | What changes |
|---|---|---|
| **Lightweight** | Single solo project, personal side project, or pre-launch experiments | Skip the plugin install (Path A manual). Skip the Recurring section if you have no recurring work yet. Just `CLAUDE.md` + `actions.md` with the Kanban on top. |
| **Standard** | Agency engagement, multi-project solo operator, or a codebase with recurring audits | Install the plugin (Path B). Seed the Recurring section with whatever cadence work you do (weekly review, monthly audit). Full Kanban + tags + ICE. |
| **Heavy** | Many active projects running in parallel (e.g. multi-client agency, portfolio operator) | Standard mode in every project folder, plus layered plugins on top of `ec-kanban-core` for client-specific workflows (the `ec-gtm-toolkit` plugin for marketing engagements, per-client ops plugins for heavy integrations like Shopify, Klaviyo, Google Ads). |

The master prompt asks you which mode applies and provisions accordingly. You can always upgrade later (Lightweight → Standard → Heavy) by running the master prompt again from the same folder. It's idempotent; it won't clobber existing content.

---

## Honest Caveats

A list of limitations. Transparent.

1. **This is a discipline system.** The Kanban caps only work if you respect them. If you add 30 items to P0 because "they all feel urgent", the system breaks down. The 5-item P0 cap is a forcing function. Treat it as binding.
2. **The Recurring hook fires only at session start.** If you keep a single Claude Code session open for eight hours, the hook only ran once at the top. For long sessions, ask Claude "anything due?" mid-session to force a re-read.
3. **Date arithmetic for `monthly` is approximate.** The hook treats "monthly" as +30 days for simplicity. Calendar-month precision (e.g., always the 1st regardless of length) is a v0.2.0 enhancement. For most recurring work, +30 days is close enough.
4. **Git-committing `actions.md` on every session leaves a noisy git log.** Many operators consider this a feature (the log is the audit trail). If you find it noisy, the solution is to squash commits per session so multiple edits collapse into one commit, or use `git commit --amend` for trivial reprioritisation passes.
5. **The plugin is v0.1.0.** Only the SessionStart hook ships right now. Future versions will add `/reprioritise`, `/skip-recurring`, `/push-recurring`, and `/archive-sweep` slash commands. Until then, those operations happen via natural-language trigger or direct file edit.
6. **The system assumes Claude Code is your primary interface.** If half your team uses Claude Code and half uses the Anthropic web app, the continuity breaks for the web-app users. Either the whole team uses Claude Code, or the markdown files get used as a shared reference (which still beats Asana for context density but loses the auto-load behaviour).
7. **Scaling across dozens of projects is untested.** The pattern works cleanly for one to ten projects. At 20+ projects, the SessionStart hook surfaces overdue recurring work across all of them and the reminder block gets long. The roadmap's answer is per-project scoping (only flag items from the current working directory), but that's also a v0.2.0 ask.

---

## Quick-Start Checklist

- [ ] Install Claude Code and verify `claude --version`.
- [ ] Install Python 3.9 or newer and verify `python3 --version`.
- [ ] Pick a project folder, `cd` into it, make sure it's a git repo (`git status`).
- [ ] Open Claude Code: `claude`.
- [ ] Paste the full contents of `claude-code-project-manager.md` into the terminal.
- [ ] Answer the interview.
- [ ] Confirm the install (files written, plugin wired if you chose Path B).
- [ ] Restart Claude Code. Confirm the SessionStart hook reminder appears at the top (if plugin installed).
- [ ] Run "what's next?" and verify Claude surfaces a sensible P0 queue.
- [ ] Run "let's wrap up" at end of session and verify git commit and push happen.
- [ ] [Want us to install this on your workflow? Book a call →](https://entirecommerce.ai/book-a-call)

---

## Troubleshooting

| Symptom | Likely cause | Fix |
|---|---|---|
| SessionStart hook reminder doesn't appear | Hook not wired in `~/.claude/settings.json`, or Python path wrong | Check the `hooks.SessionStart` block in `~/.claude/settings.json`. Confirm the absolute path to `recurring-due-check.py` is correct. Run the script manually with `CLAUDE_PROJECT_DIR=$(pwd) python3 /path/to/recurring-due-check.py </dev/null` to see if it errors. |
| Hook runs but nothing is flagged as due | No `## Recurring` section in `actions.md`, or all `last_run` dates are recent enough that nothing is overdue | Open `actions.md`, confirm the Recurring section exists and has entries. Backdate one `last_run` to more than a cadence ago and re-run the hook to confirm it surfaces the item. |
| Hook output is noisy (many overdue items) | Genuine backlog of overdue recurring work | This is working as intended. Triage: for each, either promote to P0 or skip this cycle (advance `last_run` by one cadence). |
| Claude doesn't recognise "what's next?" as the triage trigger | The natural-language trigger table isn't loaded in the active `CLAUDE.md` | Check the Session Tracking section of your project's `CLAUDE.md`. The natural-language triggers table must be present. Re-run the master prompt to refresh. |
| `actions.md` grows past 500 lines of Completed history | No archival running | Run `/archive-sweep` (when v0.4.0 ships) or manually move prior-month completed items into `archive/completed-YYYY-MM.md`. |
| ICE scores feel arbitrary | No shared rubric across items | The weighted-effort formula (80/20 Nishant/Claude) is only useful if you score consistently. Calibrate by scoring ten items in a row, then checking the ranking feels right. Re-score as a batch if not. |
| Plugin install fails | Local marketplace path wrong in `settings.json` | For v0.1.0 the plugin is installed by absolute path (marketplace install lands in v1.0.0). Check `hooks.SessionStart[0].hooks[0].command` in `~/.claude/settings.json` points at your actual filesystem location. |

---

## Credit

Built by **Nishant Kapoor** at **[EntireCommerce AI](https://entirecommerce.ai)**.

- LinkedIn: [linkedin.com/in/nishantkapoor](https://linkedin.com/in/nishantkapoor)
- Email: nishant@entirecommerce.co
- Book a call: [entirecommerce.ai/book-a-call](https://entirecommerce.ai/book-a-call)
- More playbooks: [entirecommerce.ai/playbooks](https://entirecommerce.ai/playbooks)

The system was built and dogfooded across eight live projects (EntireCommerce itself, Nerd Productivity, Incred Golf, Riyanika Jewels, Dynamite Songs, UK Country Parks, Unbolted Luxury, Pre-Owned Luxury) before being packaged as this playbook. Every rule in here earned its place by being the answer to a specific mistake made in production.

If you run into a case the playbook doesn't cover, email me. The playbook gets better each time someone sends a real edge case.
