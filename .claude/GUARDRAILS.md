# Guardrails — dhsm-statusline

> **Sempre carregado.** Tabela de lazy loading: quais docs ler antes de tocar em cada área.

**Última revisão**: 2026-07-22

---

## Antes de editar

| Ao tocar em… | Leia antes | Por quê |
|---|---|---|
| `dhsm-statusline/bin/dhsm-statusline` | Testar com JSON simulado antes de publicar; bump version em `.claude-plugin/plugin.json` no mesmo commit | Script bash puro, erros silenciosos; `/plugin update` só detecta mudança pelo campo `version`, não por diff de arquivo |
| `dhsm-statusline/.claude-plugin/plugin.json` | `marketplace.json` | Versão deve ser consistente |
| README.md | Exemplo no topo deve refletir output real | Documentação é a interface do usuário |

---

## Antes de executar ações destrutivas

- [ ] `git status` limpo antes de qualquer push
- [ ] Push em main = publicado no marketplace via plugin registry
