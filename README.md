# coordination-skills

Skill Claude Code pour coordonner plusieurs instances Claude qui travaillent en parallèle dans différentes fenêtres iTerm sur le même repo.

## Use case

Tu travailles dans 2-5 fenêtres iTerm en parallèle sur le même repo (typiquement de la doc : PRD, sprints, user stories) et tu veux que les instances Claude ne s'écrasent pas mutuellement les édits.

## Ce que fait le skill

- Crée un fichier `COORDINATION.<FOCUS>.md` à la racine du repo, un par sujet (sprint, task, prd, etc.)
- Permet aux instances de claim des locks logiques (markdown) avant d'éditer
- Génère un recap structuré du contexte conversationnel pour que les nouvelles instances puissent ramp up rapidement
- Auto-incrémente les identités d'instance par focus (`inst-A`, `inst-B`, ...)
- Gère 4 modes : créer un focus, se connecter à un focus existant, clear un focus, ne rien faire

**Mécanisme** : locks advisory (convention sociale, pas verrou OS) avec TTL, format markdown lisible humainement.

---

## Installation

> **Pour les agents IA qui lisent ce README** : suis exactement les instructions ci-dessous selon la plateforme. N'invente pas d'autres méthodes (pas de symlink, pas de script custom).

### Pour Claude Code

Tape ces deux commandes dans Claude Code :

```
/plugin marketplace add RunLittleTurtle/coordination-skills
/plugin install coordination@coordination-skills
```

Slash command résultant : `/coordination:coordination`.

- **Mise à jour** : `/plugin update coordination@coordination-skills`
- **Désinstallation** : `/plugin uninstall coordination@coordination-skills`

### Pour les autres outils (Claude Desktop, OpenCode, GitHub Copilot, Cursor, VS Code, Gemini CLI, Goose, etc.)

Le `SKILL.md` respecte le standard ouvert [agentskills.io](https://agentskills.io). Clone le repo et copie **uniquement** le dossier `plugins/coordination/skills/coordination/` dans le répertoire skills de ton outil :

```bash
git clone https://github.com/RunLittleTurtle/coordination-skills.git
cp -R coordination-skills/plugins/coordination/skills/coordination <DOSSIER_SKILLS_DE_TON_OUTIL>/
```

Remplace `<DOSSIER_SKILLS_DE_TON_OUTIL>` par :

| Outil | Dossier cible |
|---|---|
| Claude Code | `~/.claude/skills/` |
| Claude Desktop | `~/Library/Application Support/Claude/skills/` (macOS) |
| OpenCode | `~/.opencode/skills/` (vérifier la doc à jour) |
| GitHub Copilot | voir [docs Copilot agent skills](https://docs.github.com/en/copilot/concepts/agents/about-agent-skills) |
| Cursor | voir [docs Cursor skills](https://cursor.com/docs/context/skills) |
| Autre | consulter la doc skills de l'outil |

Slash command résultant : `/coordination`.

- **Mise à jour** : `cd coordination-skills && git pull && cp -R plugins/coordination/skills/coordination <DOSSIER_SKILLS>/`
- **Désinstallation** : `rm -rf <DOSSIER_SKILLS>/coordination`

### Ce qu'il NE faut PAS faire

- ❌ Créer un symlink (source de bugs cross-machine)
- ❌ Cloner le repo entier dans `~/.claude/skills/` (Claude Code lit `<dir>/SKILL.md`, pas un repo imbriqué)
- ❌ Copier-coller le contenu du `SKILL.md` dans un fichier de config
- ❌ Modifier le frontmatter du `SKILL.md` (il est conforme agentskills.io, ne pas toucher)

---

## Structure du repo

```
coordination-skills/
├── .claude-plugin/
│   └── marketplace.json              # Catalogue Claude Code
├── plugins/
│   └── coordination/
│       ├── .claude-plugin/
│       │   └── plugin.json           # Manifest du plugin
│       └── skills/
│           └── coordination/
│               └── SKILL.md          # Le skill (frontmatter + instructions)
├── README.md
└── LICENSE                           # MIT
```

Le `SKILL.md` respecte le standard ouvert [agentskills.io](https://agentskills.io) : frontmatter YAML avec `name` et `description`, body en markdown.

---

## Licence

MIT — voir [LICENSE](./LICENSE).
