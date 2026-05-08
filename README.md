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

### Option A — Plugin marketplace (Claude Code, recommandé)

Une seule commande dans Claude Code :

```
/plugin marketplace add RunLittleTurtle/coordination-skills
/plugin install coordination@coordination-skills
```

Le slash command devient `/coordination:coordination` (namespacé par le plugin).

**Mise à jour** : `/plugin update coordination@coordination-skills`
**Désinstallation** : `/plugin uninstall coordination@coordination-skills`

### Option B — Clone + copie (toutes plateformes)

Marche pour Claude Code, Claude Desktop, OpenCode, GitHub Copilot, VS Code, Cursor et tout outil compatible avec le standard [agentskills.io](https://agentskills.io).

```bash
git clone https://github.com/RunLittleTurtle/coordination-skills.git
cp -R coordination-skills/plugins/coordination/skills/coordination ~/.claude/skills/
```

Le slash command devient `/coordination`.

Pour les autres outils, copier le dossier `coordination-skills/plugins/coordination/skills/coordination/` dans le répertoire skills de l'outil (`~/.claude/skills/`, `~/.opencode/skills/`, etc.).

**Mise à jour** :
```bash
cd coordination-skills && git pull
cp -R plugins/coordination/skills/coordination ~/.claude/skills/
```

**Désinstallation** : `rm -rf ~/.claude/skills/coordination`

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
