# X Promo Post: Claude Code Project Manager Playbook

> Format: long-form single post (X Premium, per feedback_x_long_form_over_threads.md)
> Trigger word: **PM**
> Target length: 400-700 chars above the fold, full post up to 2,500 chars
> Pair with: a screenshot of a populated actions.md showing the Recurring block + P0 queue, or a 30-second screen recording of the SessionStart hook surfacing overdue work

---

I replaced Asana with two markdown files and a 200-line Python hook. Claude Code now runs project management across every project I touch.

Two files per project. CLAUDE.md holds the persistent project brief (loads every session). actions.md holds the Kanban board with strict caps: P0 ≤ 5 items, P1 ≤ 10, P2 ≤ 15. Every item ICE-scored with a weighted-effort twist. Effort scored 80/20 between me and Claude, because my time is the binding constraint and Claude hours are effectively free at the margin.

A Recurring section sits above the board. Weekly reviews, monthly audits, quarterly refreshes all live there with cadence and last_run metadata. A SessionStart hook fires at session open and surfaces anything overdue. I say "what's next?" and Claude triages the Recurring items against the P0 queue, ranks them by ICE, and tells me the top 5 to work on right now.

Five English phrases run the whole system.

"What's next?" triggers triage.
"What's due?" gives a read-only view.
"Let's do the weekly review" jumps to execution.
"Skip this week's review" advances last_run.
"Let's wrap up" reprioritises, commits, pushes.

Dogfooded across eight live projects for a month before I packaged it.

What's in the bundle:

> CLAUDE.md template (persistent project brief, loads on session start)
> actions.md template (Kanban + Recurring + Completed log)
> ICE scoring rubric with weighted 80/20 effort
> Task-owner tags: [Claude], [you], [Hybrid]
> ec-kanban-core plugin (SessionStart hook, Python, 200 lines)
> Natural-language trigger vocabulary (5 phrases)
> Master-prompt install flow (Claude interviews you, writes the files)

Like this post.
Comment "PM".
Must follow so I can DM the playbook.
