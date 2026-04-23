# LinkedIn Promo Post: Claude Code Project Manager Playbook

> Format: 8-part canonical structure from PLAYBOOK-TEMPLATE.md
> Trigger word: **PM**
> Target length: 300-500 words
> Pair with: a 30-60 second screen recording of Claude Code at session start, showing the SessionStart hook surfacing overdue recurring work at the top of the reminder block, then the user typing "what's next?" and Claude returning a ranked top-5 P0 queue

---

I turned Claude Code into my project manager across eight projects and stopped using Asana 🤯

Every project I work on has two files at its root. CLAUDE.md holds the context a new team member would need on day one. actions.md holds the work: a Kanban board with P0 ≤ 5, P1 ≤ 10, P2 ≤ 15, ICE-scored and tagged by who does the work (me, Claude, or both).

Claude reads the files on every session start. When I open a new session I say "what's next?" and Claude scans a Recurring register for anything overdue, ICE-scores it against the current P0 queue, and tells me which five items to work on right now.

The 80/20 weighted-effort formula means the board biases toward leverage. A task that takes me five minutes and Claude four hours sits at the top. A task that takes me a two-hour meeting and Claude five minutes of prep slides down. My time is the binding constraint; the board respects that.

The Recurring layer is what broke my old to-do list. Weekly reviews, monthly CLAUDE.md audits, quarterly GTM refreshes all live in a ## Recurring section with cadence and last_run metadata. A SessionStart hook fires at every session open and surfaces anything due. I never remember "it's been a month since the last audit" because the system remembers for me.

What you get:

> CLAUDE.md template (persistent project brief, loads on every session)
> actions.md template (Kanban board with strict WIP caps + Completed log)
> ICE scoring rubric with weighted effort (80/20 you/Claude)
> Task-owner tag system (Claude-run, you-run, Hybrid)
> Recurring-tasks schema with cadence vocabulary (daily / weekly / monthly / quarterly / custom)
> ec-kanban-core plugin (SessionStart hook, Python, 200 lines)
> Natural-language trigger vocabulary (5 English phrases, no slash commands to memorise)
> Multi-project install pattern (same system across every folder)
> Troubleshooting table for the first-time install

I put together the full playbook: master prompt, CLAUDE.md template, actions.md template, the plugin script, install instructions, the natural-language trigger reference.

Frictionless use: paste the Google Doc URL into Claude Code with *"read this and take me through it step by step"* and Claude does the install. Interviews you about the project, creates the files, seeds the Recurring block, wires the SessionStart hook. Works on an existing project or a fresh folder. You don't need to read the doc.

Dogfooded across eight live projects (two ecommerce brands, two DTC side projects, an agency, a music publishing leads platform, two side experiments) before packaging.

Want it for free?

Like this post.
Comment "PM".
Must be following so I can DM.
