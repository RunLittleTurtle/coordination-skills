# coordination-skill

Skill Claude Code pour coordonner plusieurs instances Claude qui travaillent en parallèle dans différentes fenêtres iTerm sur le même repo.

## `/coordination` — Coordination multi-instance

**Use case** : tu travailles dans 2-5 fenêtres iTerm en parallèle sur le même repo (typiquement de la doc : PRD, sprints, user stories) et tu veux que les instances Claude ne s'écrasent pas mutuellement les édits.

**Ce que fait le skill** :
- Crée un fichier `COORDINATION.<FOCUS>.md` à la racine du repo, un par sujet (sprint, task, prd, etc.)
- Permet aux instances de claim des locks logiques (markdown) avant d'éditer
- Génère un recap structuré du contexte conversationnel pour que les nouvelles instances puissent ramp up rapidement
- Auto-incrémente les identités d'instance par focus (`inst-A`, `inst-B`, ...)
- Gère 4 modes : créer un focus, se connecter à un focus existant, clear un focus, ne rien faire

**Mécanisme** : locks advisory (convention sociale, pas verrou OS) avec TTL, format markdown lisible humainement.

## Structure

```
coordination-skills/
├── README.md
└── coordination/
    └── SKILL.md
```

Le fichier `SKILL.md` contient le frontmatter (nom, description) et le body que Claude exécute quand on invoque la commande.

## Installation

### Sur une nouvelle machine

```bash
# 1. Cloner le repo quelque part
git clone https://github.com/RunLittleTurtle/coordination-skills.git ~/code/coordination-skills

# 2. Symlinker le skill dans ~/.claude/skills/
mkdir -p ~/.claude/skills
ln -s ~/code/coordination-skills/coordination ~/.claude/skills/coordination
```

La commande `/coordination` devient alors disponible dans toutes tes sessions Claude Code.

### Mettre à jour le skill

```bash
cd ~/code/coordination-skills
git pull
```

Le symlink pointe vers le repo, donc le pull suffit — pas besoin de recopier.

## Notes

- Skill user-level (`~/.claude/skills/`) : disponible dans tous tes repos.
- Si tu veux le skill scope-é à un repo spécifique, mets-le plutôt dans `<repo>/.claude/skills/` au lieu de symlinker.
- Réponses du skill en français.
