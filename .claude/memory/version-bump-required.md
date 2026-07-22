---
name: version-bump-required
description: plugin.json version must be bumped whenever bin/dhsm-statusline behavior changes, or /plugin update won't detect it
metadata:
  type: feedback
---

Whenever `dhsm-statusline/bin/dhsm-statusline` (or any plugin behavior) changes, bump `version` in `dhsm-statusline/.claude-plugin/plugin.json` in the same commit.

**Why:** Shipped a pricing-auto-detection fix (commit `5009882`) without bumping the version. On another machine, `/plugin update dhsm-statusline` reported it was already up to date at `0.2.1` even though the code had changed — the update mechanism keys off the `version` field, not a file diff. Had to follow up with a separate bump (`0.2.1` → `0.2.2`, commit `e58f309`) just to make the update propagate.

**How to apply:** Treat "bump plugin.json version" as a mandatory step of any commit that changes `dhsm-statusline/` behavior (script logic, commands, defaults) — not doc-only changes to the repo-root `README.md`. Do this proactively in the same commit, don't wait for the user to report a stale version on another machine. See [[GUARDRAILS]] table entry for `bin/dhsm-statusline`.
