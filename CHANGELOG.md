# Changelog

## 0.1.0

- First release.
- `setup` and `wrap-up` keep the sdd-design rules and archive format, so existing `design-specs/` folders work unchanged.
- `run` takes a request, a Jira key, a Figma link, pasted text, or a brief name to work that is ready for the designer's judgment: brief, make, evidence bundle, critique, fix, designer gate.
- `critic` agent: a fresh, read-only reviewer that sees only the evidence bundle, the brief, and the rules. Small model in round 1, stronger model after.
- Feedback triage: each point is a change, refuted with evidence and a draft reply, or the designer's decision.
- `state.json` and `decisions.md` per brief, so any session can continue the work.
