# Cowork checklist

crit is written for Cowork but not yet tested there. Run these checks in Claude Desktop (Cowork) before `1.0.0`. Record the result and the date. If a check fails, open an issue with what happened.

Setup: Claude Desktop on a paid plan, a test folder with no `design-specs/`, and a Figma file you can edit.

| # | Check | Steps | Expected | Result |
| - | ----- | ----- | -------- | ------ |
| 1 | Install from GitHub | Customize → Plugins → Add marketplace → `rohaquinlop/crit` → Install **crit** | Plugin shows 3 skills and the `critic` agent | |
| 2 | Connect Figma | Open the plugin's **Connectors** tab → connect **Figma** | Figma tools are available in a session | |
| 3 | Slash commands | Type `/crit:` in a session | `setup`, `run`, `wrap-up` are offered | |
| 4 | Critic agent starts | Run `/crit:run` on a small HTML or image request | `reviews/review-01.md` says `Reviewer: crit:critic`, not the fallback | |
| 5 | Critic reads images | Same run as check 4 | The review describes what the screenshots show, not only the file names | |
| 6 | Critic model | Same run, round 2 if there is one | Round 1 runs on the light model, round 2 on Opus. Record what the session shows | |
| 7 | Figma evidence | Run on a Figma request | The bundle has frame screenshots, `variables.json`, `styles.json`, `metadata.json` | |
| 8 | Figma comments | Ask Claude to read the comments on a Figma file | Record whether the Figma tools can read comments. If not, feedback comes in by paste | |
| 9 | HTML render | Run on an HTML request | The bundle has screenshots at each breakpoint. Record which browser tool was used | |
| 10 | Resume | Close the session at the designer gate, start a new one, run `/crit:run <name>` | Claude continues from `state.json` without redoing the brief | |
| 11 | sdd-design project | Run `/crit:run <name>` on a brief made with sdd-design | Claude creates `state.json` with the right phase and the brief's `Review rounds` as cap; review numbers keep counting; pending `designer-NN.md` points appear in `feedback.md`; open blocking findings appear in the first bundle; Claude suggests `/crit:setup` if `CLAUDE.md` has only the sdd-design block | |
| 12 | Cloud session | Start a cloud session with `design-specs` attached, run a step | Files changed are listed with the save-back instructions | |
