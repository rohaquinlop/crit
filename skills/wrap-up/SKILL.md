---
name: wrap-up
description: Close a finished, designer-approved brief — add the rule changes the designer confirms to the project's design rules and move the brief to the archive. Optionally takes the brief name, e.g. "/crit:wrap-up pricing-page".
---

# Wrap-up

Close a finished brief.

## Before you start

1. Find the work folder: the folder this session works in (in Cowork, the folder attached to the project or session). If there are several, use the one that contains `design-specs/`. If none does, ask the designer which folder is the work folder.
2. Note whether this is a cloud session: the environment or the designer says so, and the session works on its own copy of the files, not on the designer's local folder.
3. If `design-specs/` does not exist, ask with `AskUserQuestion`: "I can't find this project's design-specs folder. Were design rules set up for this project before (with crit or sdd-design)?" Options: "Yes, it was set up before" and "No, this is a new project".
   - **Yes**: explain how to give Claude access to the folder, then stop without creating any file:
     - Cowork session on the computer: open the Cowork Project that has the work folder, or add the work folder to this session.
     - Cowork cloud session: attach the whole `design-specs` folder to this session.
     - Claude Code: start Claude in the work folder.
   - **No**: stop and suggest `/crit:run <request>`.
4. Whenever you create `design-specs/` or write a file under it, make sure `design-specs/.gitignore` exists and contains `*`, so git ignores the whole folder.

## Steps

1. Find the brief the same way `/crit:run` does: use `$ARGUMENTS` if it names a brief, otherwise infer it from the conversation or the most recently changed folder in `design-specs/briefs/`, otherwise list the briefs and ask. If you inferred the brief, ask the designer to confirm it before you change anything.
2. **Gate.** Stop, change nothing, and say why if:
   - `brief.md` does not say `Status: approved`: the designer has not approved the work yet. Suggest `/crit:run <name>`.
   - `checklist.md` has unchecked items: list them.
3. **Rule changes.** For each entry in `rule-changes.md` (if the file exists):
   - Show it in plain words and ask with `AskUserQuestion`: "Add to rules (Recommended)", "Edit first", or "Skip".
   - **Edit first**: ask what to change, apply it, then treat the entry as confirmed.
   - **Before writing** a confirmed entry, check it against the existing rules. If it contradicts one, ask with `AskUserQuestion` how to resolve it, with the recommended option first, marked "(Recommended)". Write nothing for that entry until it is resolved.
   - **Confirmed**: update `design-specs/rules/<area>.md` in the shape of `${CLAUDE_SKILL_DIR}/templates/rules.md`. Create the file if it does not exist.
     - A new rule gets the highest ID ever used in its area, removed stubs included, plus one, and today's date as `Added:`.
     - An edit keeps the rule's ID and sets `Changed:` to today.
     - A removal replaces the rule with its stub heading and no body: `## <ID> — removed <YYYY-MM-DD>`. Its ID is never used again.
     - Write the real rule ID in the entry's `Merged as:` line in `rule-changes.md`.
4. **Archive.**
   - Move `design-specs/briefs/<name>/` to `design-specs/archive/<name>/`.
   - In the archived `brief.md`, replace each `NEW-` ID that has a `Merged as:` ID with that real ID. Leave the review files unchanged.
   - For each note linked in `brief.md`: if no other brief in `design-specs/briefs/` links it, move it to `design-specs/archive/<name>/notes/` and update its link in the archived `brief.md`. Otherwise leave it in place.
   - Do not move the deliverables.
5. Do the steps in **Finish**.
6. Tell the designer what was archived, which rules were added or changed (by ID), and where the deliverables are.

## Finish

If this skill created, changed, moved, or deleted files:

- Show a "Files changed" list: one line per file, the path relative to the work folder, and what happened (created, updated, moved, removed).
- In a cloud session, add in plain words: these files live in this cloud session, not on the designer's computer yet. To keep them, save each listed file to the same path in the local work folder (for example, from the session's Files pane) before the session ends. For each moved or removed file, also tell them to move or delete the local original at the same path, so the local folder matches.
