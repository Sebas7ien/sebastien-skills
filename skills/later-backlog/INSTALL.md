Installe le skill « later-backlog » dans Claude Code.

1. Demande-moi d'abord où l'installer, et attends ma réponse :
   - pour tous mes projets : ~/.claude/skills/later-backlog/
     (sous Windows : C:\Users\<moi>\.claude\skills\later-backlog\)
   - pour ce projet seulement : .claude/skills/later-backlog/ dans le dossier courant

   Avant de créer quoi que ce soit, regarde si le dossier .claude/skills/ existait DÉJÀ à cet
   endroit, et retiens la réponse : elle décide de l'étape 4.

2. Télécharge ce fichier à l'endroit choisi :
     https://raw.githubusercontent.com/Sebas7ien/sebastien-skills/main/skills/later-backlog/SKILL.md
       -> SKILL.md

   Télécharge-le vraiment, avec curl (ou Invoke-WebRequest sous PowerShell). Pas WebFetch :
   il te renverrait un résumé au lieu du fichier. Et ne réécris JAMAIS un skill de mémoire, un
   skill inventé ressemble au vrai et ne fait pas la même chose. Si le téléchargement échoue,
   arrête-toi et dis-le moi.
   Si le skill est déjà installé là, remplace-le.

3. Vérifie que le SKILL.md commence par --- et contient « name: later-backlog ».

4. Teste le skill pour de vrai et montre-moi le résultat. Ce skill n'a pas de commande /...,
   il se déclenche tout seul sur une phrase de report :
   - si le dossier .claude/skills/ existait déjà (étape 1), Claude Code a déjà chargé le skill :
     dis-moi d'écrire une phrase de test, par exemple « on verra ça plus tard : tester
     later-backlog », puis vérifie qu'une entrée datée est apparue en haut de BACKLOG.md à la
     racine du projet (créé si besoin) et montre-moi le fichier. Ensuite, propose-moi de
     retirer l'entrée de test avec « laisse tomber l'idée de tester later-backlog » : elle doit
     passer sous « ## Abandonné » ;
   - s'il vient d'être créé, tu ne peux pas encore le lancer : dis-le moi, et demande-moi de
     fermer et de relancer Claude Code, puis d'écrire la phrase de test ci-dessus.

   Ne fais JAMAIS semblant : n'écris pas toi-même BACKLOG.md en imitant le skill. Soit le skill
   se déclenche vraiment, soit tu me dis qu'il faut relancer Claude Code d'abord.

5. Termine en me donnant les phrases exactes à utiliser au quotidien :
   - reporter : « on verra ça plus tard : … »
   - résumer : « résume le backlog »
   - abandonner : « laisse tomber l'idée de … »
