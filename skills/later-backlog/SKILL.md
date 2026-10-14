---
name: later-backlog
description: Capture ideas or tasks the user wants to defer instead of doing now, recall them later, and archive them if abandoned. Use this skill proactively — without waiting for an explicit command — whenever the user says something in French (or English) that signals a task should be postponed rather than acted on, e.g. "on verra plus tard", "à faire plus tard", "plus tard", "on garde ça pour après", "on verra ça une autre fois", "pas maintenant mais...", "later", "not now but". Also use this skill whenever the user asks for a recap of pending/deferred work, e.g. "résume ce qu'il y a à faire", "résume le backlog", "qu'est-ce qu'il reste à faire", "c'est quoi le backlog du projet". Also use it when the user says a previously-deferred item should be dropped/cancelled, e.g. "laisse tomber l'idée de X", "finalement on annule Y", "on n'a plus besoin de Z", "retire ça du backlog", "abandonne le truc sur...". Do not use it for the normal Claude Code TodoList (in-conversation task tracking for the current session) — this skill is for things explicitly pushed out of the current conversation, to be picked up in a future session.
---

# Later backlog

People drop good ideas mid-conversation all the time — "pas maintenant, mais on pourrait faire X plus tard" — and without something durable, those ideas live only in a chat transcript that gets summarized away or never reopened. This skill gives each project a single, predictable place (`BACKLOG.md` at the project root) to catch that, so a future session — yours or a future Claude session — can pick it back up without the user having to re-explain the context.

## Capturing a deferred item

1. **Find the project root.** Run `git rev-parse --show-toplevel` from the current working directory; if that fails (not a git repo), use the current working directory itself. `BACKLOG.md` always lives at that root, never inside `claude-config/` or any other global location — the backlog is per-project.

2. **Create `BACKLOG.md` if it doesn't exist**, with just a title:
   ```markdown
   # Backlog
   ```

3. **Decide small vs. substantial:**
   - **Small** (a one-off reminder, no real context needed to resume it): add a single bullet directly in `BACKLOG.md`.
   - **Substantial** (anything with multiple steps, open decisions, or context that would take more than a sentence to reconstruct — like an architecture idea, a multi-step migration, a "we discussed three options and picked one" situation): write a dedicated file in `backlog/<short-descriptive-name>.md` (kebab-case, e.g. `backlog/telegram-vps-docker.md`) capturing the *why* and the state of the decision, not just the *what* — the same spirit as a good commit message or a memory note. Then add one bullet in `BACKLOG.md` linking to it.

   When in doubt, lean toward writing the detail file — losing five minutes of context because a note was too terse is the failure mode that defeats the point of this skill.

4. **Insert the new bullet right under the `# Backlog` heading** (not at the bottom), so the list stays newest-first — that's what makes a quick glance at the file useful. Format:
   ```markdown
   - **YYYY-MM-DD** — Short description of the task/idea. Optional: [détail](backlog/short-descriptive-name.md)
   ```
   Use today's date. Write the description in French, matching the user's own working language for this note (this is a personal working file, not code or a commit message).

5. **Acknowledge briefly, don't make a ceremony of it.** After writing, add a short discreet mention at the end of your normal response, like `(Noté dans BACKLOG.md)` — no need to ask for permission first or interrupt the flow of the conversation. The whole point is that this happens without friction.

If `BACKLOG.md` already exists with a different structure than the one above (e.g. the user or a previous session organized it differently), follow the existing structure rather than forcing this template on top of it — the goal is a useful backlog, not a rigid format.

## Summarizing the backlog

When asked to recap pending work:

1. Read `BACKLOG.md` at the project root.
2. For entries linking to a `backlog/*.md` file, open it if the user's question suggests they need the detail (e.g. "résume en détail", or they ask about that specific item) — for a general "qu'est-ce qu'il reste à faire", the one-line bullets are usually enough on their own.
3. Present a concise recap in the response — group related items if there are several, and don't just dump the raw file back at them. If the backlog is empty or the file doesn't exist, say so plainly rather than treating it as an error.
4. Ignore anything already under an `## Abandonné` section (see below) — that's history, not pending work.

## Archiving an abandoned item

Sometimes an idea gets dropped before it's ever picked back up — plans change, or in hindsight it wasn't worth doing. Don't just delete it: the fact that something was considered and *why* it was dropped is exactly the kind of context that saves someone from re-litigating the same idea again in six months. Archive it instead of deleting it.

1. **Find the matching entry.** Read `BACKLOG.md` and match the item the user is referring to against the bullets there. If more than one entry could plausibly match, or nothing matches with reasonable confidence, ask the user which one they mean rather than guessing — archiving the wrong item is worse than a small interruption to confirm.

2. **Move the bullet into an `## Abandonné` section** at the bottom of `BACKLOG.md` (create the section, after the active list, if it doesn't exist yet). Keep the original text, add today's date and, if the user gave a reason, a short note of why:
   ```markdown
   - **YYYY-MM-DD** — Short description of the task/idea. _Abandonné le YYYY-MM-DD : raison si connue._
   ```

3. **If the entry links to a detail file in `backlog/`**, move that file to `backlog/archive/` (create the folder if needed) and update the link in the archived bullet to point there — don't delete it, the detail is exactly what makes the "why" useful later.

4. **Acknowledge briefly**, same spirit as capturing: a short discreet mention like `(Archivé dans BACKLOG.md)` at the end of the response, no need for a confirmation step once the right item is identified.
