---
name: worktree
description: Choisir un git worktree pour la session Claude actuelle. Liste les worktrees existants, permet d'en créer un nouveau, ou de rester sur le dossier courant. À invoquer en début de session dans une nouvelle fenêtre iTerm pour décider rapidement sur quelle branche/dossier travailler sans risquer de conflit avec une autre instance Claude.
---

# Worktree session selector

Aide l'utilisateur à choisir un git worktree pour cette session Claude. L'objectif est qu'il puisse, en début de session iTerm, soit rester sur le repo principal, soit basculer dans un worktree existant, soit en créer un nouveau, sans avoir à retenir les commandes git.

## Étapes à suivre

### 1. Vérifier qu'on est dans un repo git

Exécute `git rev-parse --show-toplevel`. Si ça échoue, dis-le à l'utilisateur et arrête le skill.

### 2. Lister l'état actuel

Exécute en parallèle :
- `git worktree list` (liste tous les worktrees)
- `git branch --show-current` (branche actuelle)
- `pwd` (où on est)

Présente brièvement à l'utilisateur dans quel worktree/branche il est actuellement.

### 3. Demander ce qu'il veut faire

Utilise `AskUserQuestion` avec **une seule question** intitulée par exemple "Quel worktree pour cette session ?" et propose comme options :

- **Une option par worktree existant** sauf celui où on est déjà. Label format : `<branche> — <chemin court>`.
- **"Créer un nouveau worktree"**
- **"Rester ici"** (ne rien faire)

Si aucun autre worktree n'existe, propose seulement les deux dernières options.

### 4. Exécuter le choix

#### Option A : "Rester ici"
Confirme en une phrase et termine le skill. Ne fais rien d'autre.

#### Option B : Worktree existant
1. `cd` vers le chemin du worktree via Bash.
2. Vérifie la nouvelle branche avec `git branch --show-current`.
3. Confirme à l'utilisateur en une phrase : "On travaille maintenant dans `<chemin>` sur la branche `<branche>`."

> ⚠️ Note importante à mentionner UNE FOIS à l'utilisateur : `cd` dans Bash change le dossier pour cette session Claude, mais **pas** pour le shell iTerm. Quand il quittera Claude, son terminal retombera dans le dossier original. C'est normal.

#### Option C : Créer un nouveau worktree

1. Demande le nom du worktree avec `AskUserQuestion` (texte libre, court, en kebab-case, ex: `prd-update`, `sprint-recap`).

2. Détermine l'emplacement. Par défaut, utilise `~/worktrees/<nom-du-repo>-<nom-fourni>` pour éviter de polluer le dossier parent (utile quand le repo est dans un Google Drive ou un dossier partagé). Récupère le nom du repo avec `basename "$(git rev-parse --show-toplevel)"`.

3. Crée le dossier parent si besoin : `mkdir -p ~/worktrees`.

4. Crée le worktree avec une nouvelle branche du même nom :
   ```bash
   git worktree add "$HOME/worktrees/<repo>-<nom>" -b <nom>
   ```
   Si la branche existe déjà, utilise `git worktree add "$HOME/worktrees/<repo>-<nom>" <nom>` (sans `-b`).

5. `cd` dans le nouveau worktree.

6. Confirme : "Worktree créé à `~/worktrees/<...>` sur la branche `<nom>`. On y travaille maintenant."

### 5. Rappel de fin de vie (mentionner brièvement, une fois)

Quand l'utilisateur aura fini avec un worktree, il peut le supprimer depuis n'importe où avec :
```bash
git worktree remove ~/worktrees/<repo>-<nom>
```
Et lister tous les worktrees avec `git worktree list`. Ne le redis pas à chaque fois — juste à la création initiale.

## Règles

- **Ne jamais pousser ni merger automatiquement.** L'utilisateur décide quand.
- **Ne jamais supprimer un worktree** sans confirmation explicite de l'utilisateur.
- Si un `git worktree add` échoue (branche existe, dossier existe, etc.), explique l'erreur clairement et propose une alternative (autre nom, ou utiliser la branche existante sans `-b`).
- Réponds en français (l'utilisateur est francophone).
- Sois concis. Pas de longs paragraphes — confirme l'action en une phrase.
