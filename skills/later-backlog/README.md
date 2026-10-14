# later-backlog

Skill qui capture les idées/tâches **reportées** en cours de conversation dans un `BACKLOG.md` par projet, pour les retrouver dans une session future.

## Ce que fait le skill

Il se déclenche **automatiquement** (pas de commande à taper) dans 3 situations :

| Situation | Phrases déclencheuses (FR/EN) | Action |
|---|---|---|
| **Capturer** | « on verra plus tard », « à faire plus tard », « pas maintenant mais… », « on garde ça pour après », « later », « not now but » | Ajoute une entrée dans `BACKLOG.md` |
| **Résumer** | « résume le backlog », « qu'est-ce qu'il reste à faire », « c'est quoi le backlog du projet » | Lit `BACKLOG.md` et fait un récap groupé |
| **Abandonner** | « laisse tomber l'idée de X », « finalement on annule Y », « retire ça du backlog » | Archive l'entrée (jamais de suppression) |

Il ne remplace **pas** la TodoList de Claude Code (suivi de tâches de la session en cours).

## Fonctionnement détaillé

### 1. Capture
1. Racine du projet = `git rev-parse --show-toplevel` (sinon le dossier courant). Le backlog est **par projet**.
2. Crée `BACKLOG.md` (titre `# Backlog`) s'il n'existe pas.
3. Petit ou substantiel ?
   - **Petit** (rappel d'une ligne) : une simple puce dans `BACKLOG.md`.
   - **Substantiel** (plusieurs étapes, décisions ouvertes, contexte) : fichier dédié `backlog/<nom-kebab-case>.md` expliquant le *pourquoi* et l'état de la décision, + une puce qui y renvoie. Dans le doute, écrire le fichier détail.
4. La puce est insérée **juste sous `# Backlog`** (plus récent en premier) :
   ```markdown
   - **YYYY-MM-DD** — Description courte. [détail](backlog/nom-court.md)
   ```
5. Confirmation discrète en fin de réponse : `(Noté dans BACKLOG.md)`, sans demander la permission.

Si un `BACKLOG.md` existe déjà avec une autre structure, le skill la respecte.

### 2. Récap
Lit `BACKLOG.md`, ouvre les fichiers `backlog/*.md` seulement si le détail est demandé, présente un récap concis (groupé si besoin) et **ignore la section `## Abandonné`**. Backlog vide ou absent → il le dit simplement.

### 3. Archivage
1. Retrouve l'entrée concernée (en cas de doute ou d'ambiguïté, **demande** laquelle).
2. Déplace la puce sous `## Abandonné` (en bas du fichier) :
   ```markdown
   - **YYYY-MM-DD** — Description. _Abandonné le YYYY-MM-DD : raison._
   ```
3. Si un fichier détail existe, il est déplacé dans `backlog/archive/` et le lien est mis à jour.
4. Confirmation discrète : `(Archivé dans BACKLOG.md)`.

### Structure produite
```
mon-projet/
├── BACKLOG.md
└── backlog/
    ├── telegram-vps-docker.md
    └── archive/
        └── idee-abandonnee.md
```

## Installation

Un skill Claude Code = un dossier contenant un `SKILL.md`.

**Pour tous vos projets (global) :**
```bash
mkdir -p ~/.claude/skills/later-backlog
# copier le contenu de SKILL.md (voir ci-dessous) dans :
#   ~/.claude/skills/later-backlog/SKILL.md
```

**Pour un seul projet :**
```bash
mkdir -p .claude/skills/later-backlog
# puis SKILL.md dans .claude/skills/later-backlog/SKILL.md
```

Redémarrer Claude Code (ou ouvrir une nouvelle session) : le skill est détecté via son champ `description`. Vérification : dire « on verra ça plus tard : refaire la page de login » → une entrée doit apparaître dans `BACKLOG.md`.

> Si `CLAUDE_CONFIG_DIR` est personnalisé, remplacer `~/.claude` par ce dossier.
