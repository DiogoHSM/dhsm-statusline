---
description: Remove dhsm-statusline from the user's Claude Code settings.json
---

You are removing the `dhsm-statusline` wiring from the user's Claude Code settings.

## Steps

1. **Locate the settings file** (same order as install):
   - `$CLAUDE_CONFIG_DIR/settings.json` if set
   - `~/.claude/settings.json`
   - `~/.claude-tilt/settings.json`

2. **Read the file** and inspect the top-level `statusLine` key:
   - If absent: tell the user there is nothing to uninstall and stop.
   - If present and its `command` references `dhsm-statusline`: proceed.
   - If present but references something else: show it, ask the user whether to remove it anyway or abort.

3. **Edit the file** to remove the `statusLine` key entirely, preserving every other key and the file's formatting.

4. **Validate** with `jq . <file> > /dev/null`. On failure, restore the file from what you read in step 2.

5. **Confirm to the user**: the file path edited, and that they should restart Claude Code so the status line reverts to default. Mention that they can also run `/plugin uninstall statusline@dhsm-statusline` to remove the plugin itself.

Be concise.
