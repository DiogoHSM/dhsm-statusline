# dhsm-statusline

Plugin de statusline para Claude Code. Script bash em `dhsm-statusline/bin/dhsm-statusline` que lê JSON via stdin e imprime 1–3 linhas coloridas no rodapé do Claude Code.

## Estrutura

```
dhsm-statusline/
  bin/dhsm-statusline      # Script principal (bash)
  .claude-plugin/plugin.json
  commands/                # Slash commands de install/uninstall
.claude-plugin/            # Plugin manifest raiz
marketplace.json
README.md
```

## Layout das linhas

```
Linha 1: Model: <nome> [effort] | Cost: $X.XX | Mem: X/XG | Context: [bar] X/Xk (XX%)
Linha 2: Session: XX% | Reset: Xhr | Weekly: XX% | Weekly Reset: Xd (só quando rate-limit disponível)
Linha 3: ⎇ branch (repo) | (+X,-Y) | 🏠 main
```

## JSON payload (campos usados)

- `.model.display_name` — nome do modelo
- `.effort.level` — nível de esforço: `low`, `medium`, `high`, `xhigh`, `max` (opcional)
- `.context_window.*` — uso de contexto
- `.rate_limits.five_hour.*` e `.rate_limits.seven_day.*` — rate limits
- `.workspace.current_dir` — para git info

## Convenções

- Bash puro, sem dependências além de `jq`, `awk`, `git`
- Env vars `STATUSLINE_*` para customização (sem tocar no script)
- Testar simulando JSON no stdin antes de commitar
