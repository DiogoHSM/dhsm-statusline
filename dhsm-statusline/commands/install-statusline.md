---
description: Wire dhsm-statusline into the user's Claude Code settings.json
---

You are wiring the `dhsm-statusline` plugin into the user's Claude Code settings.

## Important: this field does NOT expand variables

Claude Code does not expand `${CLAUDE_PLUGIN_ROOT}` inside `settings.json`'s `statusLine.command` field. You must therefore write the **fully expanded absolute path** that this variable resolves to on the user's machine. Conveniently, Claude Code already expanded that variable for you in the example below — the path shown is correct for this machine and is what should be written to the file.

## Steps

1. **Locate the settings file.** Check, in order:
   - `$CLAUDE_CONFIG_DIR/settings.json` if `CLAUDE_CONFIG_DIR` is set
   - `~/.claude/settings.json` (standard location)
   - `~/.claude-tilt/settings.json` (only if `~/.claude` doesn't exist)

   If none exist, create `~/.claude/settings.json` containing `{}` (after confirming with the user).

2. **Read the file** and inspect any existing top-level `statusLine` key.
   - If its `command` ends in `/bin/dhsm-statusline`, check whether that path actually exists (e.g. `ls <path>`):
     - If it exists and is executable: tell the user it's already installed and stop.
     - If it does NOT exist (stale path from a previous install location): silently upgrade by overwriting with the new path in step 3 — do not stop.
   - If it points elsewhere (some other status-line tool): show the current value, ask whether to overwrite or abort.

3. **Edit the file** to add (or replace) this top-level key, preserving every other key and the existing formatting:

   ```json
   "statusLine": {
     "type": "command",
     "command": "${CLAUDE_PLUGIN_ROOT}/bin/dhsm-statusline"
   }
   ```

   The `${CLAUDE_PLUGIN_ROOT}` portion above will be replaced with an absolute path by the time you read this — write that absolute path into the file verbatim.

4. **Validate** the resulting JSON with `jq . <file> > /dev/null`. On failure, restore from what you read in step 2 and report.

5. **Verify dependencies**: `command -v jq` and `command -v awk`. If `jq` is missing, instruct `brew install jq`.

6. **Confirm to the user**:
   - The settings file path edited
   - That they should restart Claude Code for the change to take effect
   - Note that the baked-in absolute path means uninstalling/reinstalling the plugin may require re-running this command
   - Point them at the README's "Customization" section for optional env vars

Be concise. One short status line per step is fine. Do not over-narrate.
