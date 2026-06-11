---
description: Refresh the dhsm-statusline path in settings.json after a plugin update
---

You are refreshing the `dhsm-statusline` path in the user's Claude Code settings after a plugin update.

## Why this is needed

Claude Code bakes the absolute path to the plugin binary into `settings.json`. When `/plugin update` fetches a new version, the cache directory may change, leaving `settings.json` pointing to the old (now stale) binary. This command re-detects the current path and writes it.

## Important: this field does NOT expand variables

Claude Code does not expand `${CLAUDE_PLUGIN_ROOT}` inside `settings.json`'s `statusLine.command` field. Write the **fully expanded absolute path** shown below — Claude Code has already resolved `${CLAUDE_PLUGIN_ROOT}` for you.

## Steps

1. **Locate the settings file.** Check, in order:
   - `$CLAUDE_CONFIG_DIR/settings.json` if `CLAUDE_CONFIG_DIR` is set
   - `~/.claude/settings.json` (standard location)
   - `~/.claude-tilt/settings.json` (only if `~/.claude` doesn't exist)

   If none exist: tell the user to run `/dhsm-statusline:install-statusline` first and stop.

2. **Read the file** and inspect the top-level `statusLine` key.
   - If absent or not pointing to `dhsm-statusline`: tell the user to run `/dhsm-statusline:install-statusline` first and stop.
   - Otherwise: proceed regardless of whether the path is stale or current — always overwrite with the resolved path below.

3. **Edit the file** to replace the `statusLine` key with:

   ```json
   "statusLine": {
     "type": "command",
     "command": "${CLAUDE_PLUGIN_ROOT}/bin/dhsm-statusline"
   }
   ```

   The `${CLAUDE_PLUGIN_ROOT}` portion above will be replaced with an absolute path by the time you read this — write that absolute path into the file verbatim.

4. **Validate** the resulting JSON with `jq . <file> > /dev/null`. On failure, restore from what you read in step 2 and report.

5. **Confirm to the user**: the new path written, and that they should restart Claude Code for the updated status line to take effect.

Be concise. One short status line per step is fine. Do not over-narrate.
