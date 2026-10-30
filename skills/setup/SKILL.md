---
name: setup
description: Set up or update a project's design rules for crit — sources of truth (such as a Figma library), colors, typography, layout, components, voice, and accessibility. Use the first time a designer works in a folder, or when the design system or brand changes. Optionally takes one area to update, e.g. "/crit:setup colors", or "figma" to re-sync from the Figma library.
---

# Setup

Write or update this project's design rules in `design-specs/rules/`. Every brief and every review checks work against these rules.

Talk to the designer in plain, non-technical words. Ask about one topic at a time.

## Before you start

1. Find the work folder: the folder this session works in (in Cowork, the folder attached to the project or session). If there are several, use the one that contains `design-specs/`. If none does, ask the designer which folder is the work folder.
2. Note whether this is a cloud session: the environment or the designer says so, and the session works on its own copy of the files, not on the designer's local folder.
3. If `design-specs/rules/` does not exist and you cannot tell for sure that this is a local session, ask with `AskUserQuestion`: "I can't find this project's design rules (design-specs/rules). Were design rules set up for this project before (with crit or sdd-design)?" Options: "Yes, it was set up before" and "No, this is a new project". Skip this question if the designer already answered it in this session (for example, when run started setup). If yes, explain how to give Claude access to the folder and stop. Never create a second set of rules.
   - Cowork session on the computer: open the Cowork Project that has the work folder, or add the work folder to this session.
   - Cowork cloud session: attach the whole `design-specs` folder to this session.
   - Claude Code: start Claude in the work folder.
4. Whenever you create `design-specs/` or write a file under it, make sure `design-specs/.gitignore` exists and contains `*`, so git ignores the whole folder.
5. If this project was used with sdd-design (`CLAUDE.md` has an `<!-- sdd-design:start -->` block, or a folder in `design-specs/briefs/` has no `state.json`), help the designer move to crit before anything else. Skip this if the designer already chose to keep sdd-design in this session, or, in Claude Code, if `claude plugin list` does not show `sdd-design`.
   1. List the briefs in `design-specs/briefs/` whose `brief.md` does not say `Status: approved`. Tell the designer, in plain words:
      - Everything in `design-specs/` stays: rules, notes, briefs, reviews, and the archive. crit reads the same files, and `/crit:run <name>` continues a brief.
      - I recommend that you finish or save the work in progress before you remove sdd-design.
      - If any of this work was made in a cloud session, save its files to the local work folder first.
   2. Ask with `AskUserQuestion` whether to remove the sdd-design plugin: "Remove sdd-design (Recommended)", "It's already removed", or "Keep it for now". If they keep it, tell them that with both plugins installed, Claude may pick the wrong `setup` or `wrap-up`.
   3. To remove it:
      - Cowork (the usual case): tell the designer to open **Customize** → **Plugins**, open **sdd-design**, and select **Uninstall**. If there is no **Uninstall** button, their organization manages the plugin: tell them to ask their admin to remove it, and until then to turn off sdd-design's skills on its plugin page. Then ask with `AskUserQuestion`: "Done" or "I can't remove it". If they can't, tell them that Claude may pick the wrong `setup` or `wrap-up` until it is removed, and continue.
      - Claude Code (only when you are sure this is not Cowork): run `claude plugin list`, find the plugin named `sdd-design@<marketplace>`, and run `claude plugin uninstall <that name>`. If the command fails, tell the designer to run `/plugin` and uninstall sdd-design there.
   4. Never move, change, or delete anything in `design-specs/` for this step.

## Steps

1. If `design-specs/rules/` does not exist, do the **first run** (step 2). Otherwise do **update mode** (step 3).
2. **First run**
   1. Explain in two sentences what happens: a short interview, then Claude saves the project's design rules in a `design-specs` folder inside the work folder, so every later task follows them.
   2. Interview the designer, one topic at a time. Use `AskUserQuestion` for real choices (recommended option first, marked "(Recommended)"), and free text otherwise. Skip any topic the designer says does not apply, except accessibility.
      1. **Project**: what is designed, for whom, and the brand or product name.
      2. **Sources of truth**: Figma library or file links, brand guideline files, reference images.
      3. **Colors**: palette with names and hex values, and what each color is for.
      4. **Typography**: font families, sizes, weights, line heights, text styles.
      5. **Layout**: spacing scale or grid, radii, breakpoints.
      6. **Components**: the component library and the rules for using it.
      7. **Voice**: tone, words to use or avoid, length limits.
      8. **Accessibility**: the standard to meet. If the designer has none or skips the topic, use WCAG 2.2 AA and tell them you recorded it as the default. Always write `rules/accessibility.md`.
      9. **Usual outputs**: Figma frames, images, HTML prototypes, copy, other. Record them in the `## About` section of `rules/project.md`.
   3. **Figma pre-fill**: if Figma tools are available and the designer gave a Figma library or file, offer to read its variables and styles to pre-fill colors, typography, and layout. Show what you found and let the designer confirm or correct it before you use it. If no Figma tools are available, explain in one or two sentences that a connector lets Claude use another app, and that they can connect Figma from the crit plugin's Connectors tab (in Claude Code, with `/mcp`). Then continue without Figma.
   4. Write `design-specs/rules/<area>.md` for each area that has content (`project`, `colors`, `typography`, `layout`, `components`, `voice`, `accessibility`), in the shape of `${CLAUDE_SKILL_DIR}/templates/rules.md`. Give each rule an ID with its area prefix, a class, a way to check it, and today's date as `Added:`. Skip areas with no content. Write only rules the designer gave or confirmed. If you think a rule follows from their answers, ask before you add it.
3. **Update mode**
   1. If `$ARGUMENTS` names an area (`project`, `colors`, `typography`, `layout`, `components`, `voice`, `accessibility`), work only on that file. If it is `figma`, go to step 3.5. Otherwise ask which areas changed.
   2. Read the rule files as they are now. The designer may have edited them by hand. Show the current rules as a short, readable list.
   3. Ask what changed. Edit in place:
      - Keep every rule and every word the designer did not ask to change.
      - Keep existing IDs. Give a new rule the highest ID ever used in its area, removed stubs included, plus one. Never renumber or reuse an ID.
      - Set `Changed:` to today on each edited rule. Replace each removed rule with its stub heading and no body: `## <ID> — removed <YYYY-MM-DD>`.
   4. Before writing, show a short summary of the changes (added, changed, removed, by ID) and ask the designer to confirm.
   5. **Figma re-sync**: read the Figma source named in `rules/project.md`. Compare its variables and styles with the rules and show only the differences. Write only the differences the designer confirms, as in steps 3.3 and 3.4. If Figma tools are not available, say so and stop.
4. On every run:
   - Make sure `design-specs/.gitignore` exists and contains `*`. On the first run, tell the designer in one sentence that this keeps the folder out of git, in case the work folder is ever used with git.
   - In `<work-folder>/CLAUDE.md`, write the content of `${CLAUDE_SKILL_DIR}/templates/claude-md-block.md`. Replace the text between `<!-- crit:start -->` and `<!-- crit:end -->` if the markers exist. Otherwise append the block, or create the file. Never change anything outside the markers.
   - If `CLAUDE.md` also has an sdd-design block (`<!-- sdd-design:start -->` to `<!-- sdd-design:end -->`), tell the designer that it names the old workflow, and ask with `AskUserQuestion` whether to remove that block: "Remove it (Recommended)" or "Keep it". Remove only the text between and including those markers.
   - On the first run, explain in plain words:
     - `CLAUDE.md` is a note that Claude reads when it works in this folder.
     - A Cowork Project keeps the work folder, standing instructions, and Claude's memory together. Give the designer this line to paste into the Project's Instructions: `Before any design task, read design-specs/rules/ and follow it.`
     - Claude's memory can forget details. The rules files are the lasting, editable record of what matters.
5. Do the steps in **Finish**.
6. Suggest the next step: `/crit:run <request>` to start a piece of work.

## Finish

If this skill created, changed, moved, or deleted files:

- Show a "Files changed" list: one line per file, the path relative to the work folder, and what happened (created, updated, moved, removed).
- In a cloud session, add in plain words: these files live in this cloud session, not on the designer's computer yet. To keep them, save each listed file to the same path in the local work folder (for example, from the session's Files pane) before the session ends. For each moved or removed file, also tell them to move or delete the local original at the same path, so the local folder matches.
