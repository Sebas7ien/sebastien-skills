# sebastien-skills

Collection de skills [Claude Code](https://claude.com/claude-code) que j'utilise au quotidien.

## Skills

| Skill | Rôle | Doc |
|---|---|---|
| `later-backlog` | Capture les idées reportées (« on verra plus tard ») dans un `BACKLOG.md` par projet, les résume, les archive | [skills/later-backlog](skills/later-backlog/README.md) |

## Installer un skill

Pour chaque skill, ouvrez son `INSTALL.md`, collez-en le contenu dans Claude Code, et répondez à la question sur l'emplacement (global `~/.claude/skills/` ou projet `.claude/skills/`). Claude télécharge le `SKILL.md` depuis ce repo et teste le skill.

Installation manuelle :

```bash
mkdir -p ~/.claude/skills/later-backlog
curl -fsSL https://raw.githubusercontent.com/Sebas7ien/sebastien-skills/main/skills/later-backlog/SKILL.md \
  -o ~/.claude/skills/later-backlog/SKILL.md
```

Puis ouvrez une nouvelle session Claude Code.

## Structure

```
skills/<nom>/
├── SKILL.md    # le skill
├── INSTALL.md  # prompt d'installation à coller dans Claude Code
└── README.md   # comment s'en servir
```

## Licence

MIT
