# coordination-skills

Skills Claude Code personnels pour gérer plusieurs instances Claude qui travaillent en parallèle dans différentes fenêtres iTerm sur le même repo.

## Skills inclus

### `/coordination` — Coordination multi-instance

**Use case** : tu travailles dans 2-5 fenêtres iTerm en parallèle sur le même repo (typiquement de la doc : PRD, sprints, user stories) et tu veux que les instances Claude ne s'écrasent pas mutuellement les édits.

**Ce que fait le skill** :
- Crée un fichier `COORDINATION.<FOCUS>.md` à la racine du repo, un par sujet (sprint, task, prd, etc.)
- Permet aux instances de claim des locks logiques (markdown) avant d'éditer
- Génère un recap structuré du contexte conversationnel pour que les nouvelles instances puissent ramp up rapidement
- Auto-incrémente les identités d'instance par focus (`inst-A`, `inst-B`, ...)
- Gère 4 modes : créer un focus, se connecter à un focus existant, clear un focus, ne rien faire

**Mécanisme** : locks advisory (convention sociale, pas verrou OS) avec TTL, format markdown lisible humainement.

### `/worktree` — Sélection de git worktree

**Use case** : tu veux ouvrir 2-3 sessions Claude en parallèle sur des branches différentes du même repo sans conflit physique de fichiers.

**Ce que fait le skill** :
- Liste les git worktrees existants
- Permet d'en créer un nouveau (dans `~/worktrees/<repo>-<nom>/`) avec une nouvelle branche
- Bascule la session courante dans un worktree existant

## Structure

```
coordination-skills/
├── README.md                  ← ce fichier
├── coordination/
│   └── SKILL.md              ← skill /coordination
└── worktree/
    └── SKILL.md              ← skill /worktree
```

Chaque sous-dossier correspond à un skill. Le fichier `SKILL.md` à l'intérieur contient le frontmatter (nom, description) et le body que Claude exécute quand on invoque la commande.

## Installation

### Sur une nouvelle machine

```bash
# 1. Cloner le repo quelque part
git clone https://github.com/RunLittleTurtle/coordination-skills.git ~/code/coordination-skills

# 2. Symlinker chaque skill dans ~/.claude/skills/
mkdir -p ~/.claude/skills
ln -s ~/code/coordination-skills/coordination ~/.claude/skills/coordination
ln -s ~/code/coordination-skills/worktree ~/.claude/skills/worktree
```

Les commandes `/coordination` et `/worktree` deviennent alors disponibles dans toutes tes sessions Claude Code.

### Mettre à jour les skills

```bash
cd ~/code/coordination-skills
git pull
```

Les symlinks pointent vers le repo, donc le pull suffit — pas besoin de recopier.

## Notes

- Skills user-level (`~/.claude/skills/`) : disponibles dans tous tes repos.
- Si tu veux un skill scope-é à un repo spécifique, mets-le plutôt dans `<repo>/.claude/skills/` au lieu de symlinker.
- Réponses des skills en français (j'ai écrit ces skills pour mon usage perso francophone).
