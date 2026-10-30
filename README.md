# crit

[![Validate plugin](https://github.com/rohaquinlop/crit/actions/workflows/validate-plugin.yml/badge.svg)](https://github.com/rohaquinlop/crit/actions/workflows/validate-plugin.yml)

Design work with an independent critique. You give Claude a request. Claude writes a brief, makes the work, and a separate critic checks it against your design rules and references. Claude fixes what the critic finds. You see the work only when it is ready for your judgment.

It works with Figma, HTML prototypes, images, and copy. It is only Markdown and JSON: no scripts, no dependencies, no telemetry. It reads the same `design-specs/` folder as [sdd-design](https://github.com/rohaquinlop/spec-driven-designment), so projects you set up there work here with no changes.

## How it works

```
/crit:setup                  → once per project: save your design rules
/crit:run <request>          → brief → make → critique → fix → ready for you
/crit:run <pasted feedback>  → triage the feedback, fix, critique again
/crit:wrap-up <name>         → add what you learned to your rules, archive the brief
```

A request can be plain words ("a pricing page for the Pro plan"), a Jira key or link, a Figma link, or a pasted Slack message. You don't have to type the commands: "let's start the pricing page" works too.

### The critique loop

Design has no automatic test like code does. Your rules are the test.

- Every rule and every acceptance criterion is one of three kinds:
  - **Measurable**: checked by exact value, e.g. "buttons use `#1A73E8`" or "text contrast is at least 4.5:1".
  - **Visual**: checked against a reference, e.g. "matches the hero frame in Figma".
  - **Judgment**: only you can decide, e.g. "feels calm".
- Claude makes the work, then captures the evidence once: Figma screenshots and variables, browser renders at each breakpoint, images, the copy. It puts them in a **bundle**.
- A separate **critic** reads only the bundle, the brief, and the rules. It cannot open Figma or change files, and it never sees Claude's reasoning. It reports pass, fail, or needs-designer for every criterion and rule, with evidence.
- Claude fixes the failures and the critic checks again, up to 3 rounds by default. The first round runs on a small model; later rounds use a stronger one.
- Then you get the result: what passed, what is still open, and the judgment items for you to decide. You approve it or give feedback.

### Feedback from others

Paste feedback from a stakeholder, a Slack thread, or a Jira comment, and Claude sorts each point:

- **change**: a real gap against the brief or the rules. Claude fixes it, and the critic checks the fix.
- **refuted**: the rules or a measured value answer it. Claude writes the evidence and a draft reply for you to send.
- **you decide**: taste, or something the brief does not cover.

Your own feedback is always a change. Claude never posts replies, approves work, publishes a library, or hands work off. Those stay with you.

### When Claude stops to ask you

- A choice the brief needs and the rules don't settle.
- Judgment items, and feedback that is yours to decide.
- Feedback of yours that conflicts with a rule.
- Missing access: a connector, Figma edit rights, a Jira story it cannot read.
- When the work is ready for your judgment.

## Install

**Moving from sdd-design?** Finish or save any work in progress first. If you made work in a cloud session, save its files to your local work folder. Your `design-specs/` folder stays as it is. `/crit:setup` then walks you through removing sdd-design: in Cowork, open **Customize** → **Plugins**, open **sdd-design**, and select **Uninstall**.

### In Claude Desktop (Cowork)

Cowork needs a paid Claude plan.

1. Open the **Cowork** tab and select **Customize** in the sidebar, then **Plugins**.
2. Select **Add marketplace** and enter `rohaquinlop/crit`.
3. Find **crit** and select **Install**.
4. Optional: open the plugin's **Connectors** tab and connect **Figma**, so Claude can read and edit your Figma files. Editing needs a Figma Full seat and edit access to the file.

### In Claude Code

```
/plugin marketplace add rohaquinlop/crit
/plugin install crit@crit
```

## First steps

1. In Cowork, create a **Project** and attach the folder where you keep the work for this project. Start sessions from that project, on your computer.
2. Run `/crit:setup`. If the folder already has `design-specs/rules/` from sdd-design, setup shows them and changes only what you ask.
3. Paste the line that setup gives you into the Project's **Instructions**.
4. Start your first piece of work with `/crit:run <request>`.

## Where your files live

```
your-work-folder/
├── CLAUDE.md                 # a short note Claude reads in this folder
├── pricing-page/             # your deliverables (location set per brief)
└── design-specs/
    ├── rules/                # your design rules — edit them any time
    ├── notes/                # saved research
    ├── briefs/<name>/
    │   ├── source.md         # the request, as received
    │   ├── brief.md          # goal, output, references, acceptance criteria
    │   ├── checklist.md      # deliverables
    │   ├── rule-changes.md   # proposed new rules (NEW-01, ...)
    │   ├── references/       # images and files you supplied
    │   ├── bundles/round-NN/ # evidence the critic reviewed
    │   ├── reviews/          # critic reports
    │   ├── feedback.md       # every piece of feedback and its verdict
    │   ├── decisions.md      # choices made along the way
    │   └── state.json        # where the work stands, so any session can continue
    └── archive/<name>/       # finished briefs
```

## FAQ

**Do I need git?** No. If the folder is ever used with git, `design-specs/` is ignored automatically.

**I don't use Figma.** Everything still works with HTML prototypes, images, and copy.

**Why a separate critic?** When the same session reviews its own work, it tends to see what it meant to make. The critic sees only the evidence. Because it reads one prepared bundle instead of capturing everything again, it also costs fewer tokens than a reviewer that redoes the work.

**What if the critic can't run?** If the environment cannot start the critic, Claude reviews its own work with strict rules, and tells you the review was a weaker fallback.

**How is it different from sdd-design?** Same rules, briefs, and folder. crit adds the independent critic, evidence bundles, feedback triage, one command for the whole flow, and a decisions log so work continues across sessions.

**I used sdd-design. Should I keep it installed?** No. Both plugins have `setup` and `wrap-up`, so with both installed, Claude may pick the wrong one. Run `/crit:setup` once: it offers to remove sdd-design and updates the note in `CLAUDE.md`. Your rules, notes, briefs in progress, and archive stay in `design-specs/` and keep working: run `/crit:run <name>` to continue a brief.

## Status

Pre-1.0. Some Cowork behavior is unconfirmed. See [docs/cowork-checklist.md](./docs/cowork-checklist.md).

## Versioning

The `version` field in [`.claude-plugin/plugin.json`](./.claude-plugin/plugin.json) is set explicitly. Installs receive an update only when it is bumped.

- **Major**: a breaking change to command names, arguments, or the `design-specs/` folder convention.
- **Minor**: a new skill, template, or backward-compatible improvement.
- **Patch**: wording, doc, or example fixes with no effect on behavior.

## License

MIT
