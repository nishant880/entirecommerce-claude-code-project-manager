# {Your project name}

> This file is the persistent project brief Claude Code loads automatically on every session start. It grounds every Claude Code session in the specifics of this project.
>
> **Two ways to populate it:**
>
> **Option A (fastest, recommended).** Leave the file as-is. Run the Claude Code Project Manager master prompt (`claude-code-project-manager.md` in this bundle) and Claude will interview you, ask every question below, and fill in the file for you as you go. Takes about 15 minutes.
>
> **Option B (DIY).** Fill in every `{placeholder}` yourself before running the master prompt. Keep it tight: one to two sentences per field.
>
> Either way the end state is the same: a populated `CLAUDE.md` that loads on every session start and that every future playbook reads from.

---

## Project overview

- **Project name**: {Your project name}
- **Primary URL** (if any): {https://yourproject.com}
- **Stage**: {Pre-launch / first year / established with traction / mature}
- **Why this project exists (one sentence)**: {What problem it solves, who for, why you}

## Who this project is for

- **Primary user or customer (one sentence)**: {e.g. "US-based DTC founders running on Shopify who want a monthly SEO audit"}
- **Secondary audience, if any**: {Optional}
- **Where they currently go for this**: {What they use today, what's broken about it}

## Wired-up tools and credentials

List every tool, API, MCP server, or credential this project depends on. Claude uses this to know what it can and cannot access directly.

- **Website or platform**: {Shopify store URL, WordPress site, Next.js on Vercel, etc.}
- **Analytics**: {GA4 property ID, Clarity project, Search Console verification}
- **Credentials location**: {`.env` at root / 1Password / etc.}
- **MCP servers wired**: {List the ones this project uses}
- **API keys available in `.env`**: {List variable names only, never the values}

## Operating rules

Anything Claude should always do or never do on this project. Keep it tight.

- **Always**: {e.g. "commit every change to git, push to origin/main, notify Telegram after deploy"}
- **Never**: {e.g. "never push to the live Shopify theme directly, always use GitHub integration"}
- **Brand voice, if applicable**: {Banned phrases, tone conventions, spelling conventions}
- **Deploy rules, if applicable**: {Staging first, review required, etc.}

## Project Management (do not edit; inserted by the master prompt)

This section is added by the Claude Code Project Manager master prompt. It contains the priority rules, tags, ICE formula, Recurring-tasks schema, and natural-language triggers that the system runs on. Copy every line verbatim.

### Priority rules

- **P0**: max 5 items. Work in flight right now.
- **P1**: max 10 items. On-deck queue.
- **P2**: max 15 items. Near-term backlog.
- **P3 and below**: no cap.

Reprioritise P0 and P1 at end of every session and during any weekly-review run. Caps are a pull-system target. Always fill to cap by pulling up from the next tier.

### Task-owner tags

Every item on the board carries one of three tags:

- **[Claude]**: Claude Code can execute end-to-end using existing tools and credentials. These items live in a dedicated "awaiting approval" section at the bottom of the board. When surfaced, Claude asks permission, executes, logs to Completed.
- **[{Your name}]**: requires your hands. A phone call, a decision, a UI only you can log into, an in-person meeting.
- **[Hybrid]**: Claude builds, you review and approve.

### ICE formula

ICE = Impact × Confidence × Weighted Ease.

- **Impact**: 1 to 10. Revenue or strategic weight. Higher wins.
- **Confidence**: 1 to 10. How sure you are it will work. Higher wins.
- **Ease**: 1 to 10. Inverse of effort. Higher ease = lower effort = easier to ship.

Effort weighted 80/20 between you and Claude:

```
Weighted Ease = 0.8 × Your_ease + 0.2 × Claude_ease
ICE           = Impact × Confidence × Weighted Ease
```

### Recurring tasks schema

Recurring items live in a `## Recurring` section above `## Next Actions` in `actions.md`. They do not count against P0/P1/P2 caps. Line format:

```
- [Tag] Title | cadence: <cadence> | last_run: YYYY-MM-DD | skill: /optional
```

Cadences: `daily`, `weekly`, `biweekly`, `monthly`, `quarterly`, `annually`, or `custom:Nd`.

The SessionStart hook (`ec-kanban-core` plugin) surfaces items where `last_run + cadence <= today`. Due items get promoted into P0 at their earned ICE rank when triaged.

### Natural-language triggers

Five English phrases map to five discrete behaviours.

| Phrase | Behaviour |
|---|---|
| "What's next?" / "What should I work on next?" / "Session kickoff" | Run the triage pass: parse Recurring, ICE-score due items against current P0, slot them at earned rank (demoting lowest-ICE non-blocker if needed), return top 5 P0 items with context. |
| "What's due?" / "Anything I'm behind on?" | Read-only view of the Recurring register. No P0 changes. |
| "Let's do the [X] review" / "Let's do the [X] audit" | Jump to execution. Invoke the matching skill. Update `last_run` when done. |
| "Skip the [X] this cycle" / "Push [X] to [date]" | Edit the Recurring section directly. Advance `last_run` by one cadence for skip, or set specific date for push. |
| "Let's wrap up" / "End of session" | Reprioritise P0 and P1, pull up from lower tiers, log completions, update `last_run` for any closed recurring P0 items, push to git. |

---

## Session history (updated by Claude across sessions)

> Claude will append notes below as it learns patterns specific to this project. Leave this section blank to start; it grows over time.

---

## How Claude should use this file

On every session start, load this brief and treat it as the ground truth for the project's specifics. When you run any work in this folder, cross-reference this file before asking the user for context. Do not invent values. If a field is blank, ask the user to fill it in before proceeding with work that depends on it.

If the user invokes any of the five natural-language triggers, follow the behaviour in the Project Management table exactly. Do not freelance around the triage or reprioritisation passes.
