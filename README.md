# dhsm-statusline

A three-line Claude Code status bar with cost estimate, macOS memory, context-window progress, optional 5h/7d rate-limit usage, and git info.

```
Model: Sonnet 4.6 [high] | Cost: $0.42 | Context: [████░░░░░░] 78k/200k (39%)
Session: 22.4% | Reset: 3hr 12m | Weekly: 41.0% | Weekly Reset: 4d 8hr
Mem: 24.9G/32.0G | ⎇ feature/billing (tilt-api) | (+312,-87) | 🏠 main
```

Line 2 only renders when Claude Code provides rate-limit data. Line 3 only renders inside a git repo.

## Requirements

- macOS (the memory and `vm_stat` bits are macOS-specific)
- `jq` — install with `brew install jq`
- `awk` and `git` — ship with macOS

## Install

```text
/plugin marketplace add diogohsm/dhsm-statusline
/plugin install dhsm-statusline@dhsm-statusline
/reload-plugins
/dhsm-statusline:install-statusline
```

`/reload-plugins` makes the plugin's slash commands available in the current session — without it, `/dhsm-statusline:install-statusline` won't be registered yet.

The install command asks Claude to edit your `~/.claude/settings.json` and add the `statusLine` block. You'll see the diff and approve it like any other Edit.

Then **restart Claude Code** so it picks up the new status line.

## Update

```text
/plugin update dhsm-statusline
/reload-plugins
/dhsm-statusline:update-statusline
```

Then **restart Claude Code**. The update command re-detects the binary path and refreshes `settings.json` — necessary because Claude Code bakes in an absolute path that may change between plugin versions.

## Uninstall

```text
/dhsm-statusline:uninstall-statusline
/plugin uninstall dhsm-statusline@dhsm-statusline
```

The first command removes the `statusLine` block from your settings.json. The second removes the plugin itself.

## Customization

All knobs are environment variables. Set them in your shell profile (`~/.zshrc`, `~/.bashrc`) — they survive plugin updates.

| Variable | Default | What it does |
|---|---|---|
| `STATUSLINE_INPUT_PRICE` | auto-detected | USD per 1M input tokens |
| `STATUSLINE_OUTPUT_PRICE` | auto-detected | USD per 1M output tokens |
| `STATUSLINE_CACHE_READ_PRICE` | auto-detected | USD per 1M cache-read tokens |
| `STATUSLINE_CACHE_WRITE_PRICE` | auto-detected | USD per 1M cache-write tokens |
| `STATUSLINE_CTX_YELLOW_TOKENS` | `120000` | Context-bar turns yellow above this |
| `STATUSLINE_SHOW_MEMORY` | `1` | `0` to hide the macOS memory segment |
| `STATUSLINE_SHOW_GIT` | `1` | `0` to hide the git line |

Pricing auto-detects per request from the model's display name (Opus/Sonnet/Haiku/Fable/Mythos tiers — matching Anthropic's published per-1M-token rates, with cache-read at ~0.1x and cache-write at ~1.25x the input price). Unrecognized model names fall back to Sonnet-tier pricing. Set any `STATUSLINE_*_PRICE` var to override auto-detection.

## Manual install (no slash command)

If you'd rather wire it up yourself, add this to your `settings.json` — but note that Claude Code does **not** expand `${CLAUDE_PLUGIN_ROOT}` inside `statusLine.command`, so you must write the resolved absolute path:

```json
"statusLine": {
  "type": "command",
  "command": "/Users/<you>/.claude/plugins/.../dhsm-statusline/bin/dhsm-statusline"
}
```

Find the resolved path with `ls ~/.claude/plugins/cache/*/dhsm-statusline*/bin/dhsm-statusline` (or wherever your Claude Code installs plugins — check `~/.claude/plugins/` or `$CLAUDE_CONFIG_DIR/plugins/`). The slash command does this lookup for you, which is why it's the recommended path.

Because the path is baked in, if you uninstall and reinstall the plugin, re-run `/dhsm-statusline:install-statusline` to refresh the absolute path.

## Inspiration

Inspired by [ccstatusline](https://github.com/sirmalloc/ccstatusline) by sirmalloc — a much more feature-rich statusline worth checking out. Both are MIT licensed.

## License

MIT — see [LICENSE](LICENSE).
