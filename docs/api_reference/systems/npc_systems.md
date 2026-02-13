# ⚙️ Systèmes de PNJ (NPC Systems)

Dans une architecture ECS (Entity-Component-System), les "Systèmes" contiennent la logique du jeu. Ils opèrent sur des ensembles d'entités qui possèdent des combinaisons spécifiques de composants. Ce document décrit certains des systèmes liés aux PNJ.

## `NPCDeathSystems`

Cette classe contient des systèmes qui réagissent à la mort d'entités.

### `NPCKillsEntitySystem`

Ce système s'exécute chaque fois qu'une entité vivante meurt.

*   **Objectif** : Détecter si le tueur est un PNJ et, si c'est le cas, l'en informer.
*   **Logique** :
    1.  Le système s'abonne aux événements de mort (`OnDeathSystem`).
    2.  Quand une entité meurt, il récupère la source des dégâts.
    3.  Si la source est une autre entité (`EntitySource`), il vérifie si cette entité source est un PNJ (en regardant si elle a le `NPCEntity` component).
    4.  Si c'est un PNJ, il appelle la méthode `onKill()` sur les données de dégâts du PNJ (`sourceNpcComponent.getDamageData().onKill()`).

*   **Cas d'usage** :
    *   Mettre à jour un compteur de quête pour un PNJ ("Tuer 10 slimes").
    *   Faire changer le PNJ d'état (par exemple, de "combat" à "inactif" après avoir tué sa cible).

### `EntityViewSystem`

Ce système s'exécute également à la mort d'une entité et sert à notifier d'autres systèmes de cet événement via le `Blackboard`.

*   **Objectif** : Diffuser un événement de "mort" sur le `Blackboard` pour que d'autres systèmes (comme l'IA d'autres PNJ) puissent y réagir.
*   **Logique** :
    1.  Le système s'abonne aux événements de mort.
    2.  Il vérifie que la mort a été causée par une autre entité.
    3.  Il récupère la position de la mort et trouve le `Blackboard` correspondant (qui est organisé par chunk).
    4.  Il publie un événement `EntityEventType.DEATH` sur la `EntityEventView` du `Blackboard`.

*   **Cas d'usage** :
    *   Permettre à des PNJ alliés dans la zone de réagir à la mort d'un ami.
    *   Permettre à un "boss" de PNJ d'entrer dans une phase de rage lorsque ses sbires sont tués.
    *   **Note spéciale** : Ce système ignore les morts causées par des joueurs en mode Créatif, sauf si une option spécifique (`allowNPCDetection`) est activée, pour éviter de déclencher des logiques de jeu pendant la construction ou le test.

## `NPCSystems`

Cette classe est un conteneur pour de nombreux systèmes qui gèrent le cycle de vie et l'initialisation des PNJ.

### `AddedSystem`

Ce système est l'un des plus importants. Il gère ce qui se passe lorsqu'un PNJ est ajouté au monde.

*   **Déclencheur** : Une entité avec un composant `NPCEntity` est ajoutée au monde.
*   **Logique à l'ajout (`onEntityAdded`)** :
    1.  Récupère le `Role` du PNJ. S'il n'y en a pas, l'entité est invalide et supprimée.
    2.  Appelle `role.loaded()` pour permettre au rôle d'initialiser sa logique.
    3.  Initialise la connexion du PNJ au `Blackboard` pour les changements de blocs.
    4.  S'assure que le PNJ a bien les composants de base nécessaires (`PrefabCopyableComponent`, `PositionDataComponent`, etc.).
    5.  Si le PNJ vient d'être créé (`AddReason.SPAWN`), il lui ajoute un `NewSpawnComponent` pour le protéger temporairement.
*   **Logique à la suppression (`onEntityRemove`)** :
    1.  Nettoie la connexion au `Blackboard`.
    2.  Appelle `role.removed()` ou `role.unloaded()` pour que le rôle puisse nettoyer son état et ses ressources.

### `AddedFromExternalSystem`

Ce système s'occupe des PNJ qui ne sont pas créés dynamiquement, mais qui proviennent de la génération du monde (`FromWorldGen`) ou d'un "prefab" placé par un designer (`FromPrefab`).

*   **Déclencheur** : Un PNJ avec un composant `FromWorldGen` ou `FromPrefab` est ajouté.
*   **Logique** :
    1.  Définit le "point d'attache" (`LeashPoint`) du PNJ à sa position de départ. C'est sa "maison", vers laquelle il pourrait essayer de retourner.
    2.  Sauvegarde son orientation de départ.
    3.  Appelle la méthode spéciale `role.onLoadFromWorldGenOrPrefab()` pour permettre au rôle de s'initialiser différemment (par exemple, en choisissant un nom aléatoire ou en remplissant son inventaire).

### `OnDeathSystem`

Ce système gère un aspect spécifique de la mort d'un PNJ.

*   **Déclencheur** : Un composant `DeathComponent` est ajouté à un PNJ.
*   **Logique** :
    1.  Il lit la durée de l'animation de mort (`deathAnimationTime`) depuis le `Role` du PNJ.
    2.  Si cette durée est supérieure à 0, il ajoute un composant `DeferredCorpseRemoval` au PNJ.
    3.  Ce composant va simplement retarder la suppression définitive de l'entité du monde, laissant le temps à l'animation de se jouer entièrement.

### `ModelChangeSystem`

Ce système réagit aux changements d'apparence du PNJ.

*   **Déclencheur** : Le `ModelComponent` du PNJ est ajouté ou modifié.
*   **Logique** :
    1.  Récupère le nouveau `Model`.
    2.  Appelle `role.updateMotionControllers()` en lui passant les nouvelles informations du modèle (comme la `Box` de collision et les valeurs physiques).
    3.  Ceci est essentiel pour que le mouvement du PNJ s'adapte à sa nouvelle forme.

Ces systèmes montrent comment la logique est découpée en petites responsabilités qui réagissent aux changements d'état (ajout/suppression de composants) des entités, ce qui est le cœur de la philosophie ECS.

## `PositionCacheSystems`

Ces systèmes gèrent le `PositionCache`, un objet crucial pour les performances de l'IA des PNJ.

### Le `PositionCache` : pourquoi est-ce important ?

Un PNJ a constamment besoin de connaître son environnement : "Où sont les joueurs les plus proches ?", "Y a-t-il d'autres PNJ à proximité pour ne pas leur rentrer dedans ?". Faire ces recherches spatiales pour chaque PNJ à chaque tick serait extrêmement coûteux en performances.

Le `PositionCache` résout ce problème. C'est un objet, attaché au `Role` du PNJ, qui contient des listes pré-calculées d'entités proches. Le `PositionCacheSystems.UpdateSystem` met à jour ces listes à intervalle régulier, et ensuite toutes les autres parties de l'IA (les "sensors", les "actions") peuvent lire ces listes en cache au lieu de refaire une recherche coûteuse.

### `UpdateSystem`

C'est le système principal qui met à jour le cache.

*   **Déclencheur** : S'exécute à chaque tick pour tous les PNJ.
*   **Logique** :
    1.  Il ne s'exécute pas en continu. Il utilise une temporisation (`tickPositionCacheNextUpdate`) pour décider s'il doit rafraîchir les données.
    2.  Quand une mise à jour est nécessaire, il utilise des `SpatialResource` (des structures de données optimisées pour les recherches spatiales) pour trouver tous les joueurs, PNJ, et objets proches.
    3.  Il remplit les listes dans le `PositionCache` du PNJ avec les résultats.

### `RoleActivateSystem` et `OnFlockJoinSystem`

Ces systèmes gèrent l'initialisation du `PositionCache`.

*   **Logique** :
    1.  Ils appellent la méthode statique `initialisePositionCache()`.
    2.  Cette méthode inspecte le `Role` du PNJ (ses actions, ses capteurs, son comportement de groupe) pour déterminer de quelles informations il aura besoin. Par exemple, si le rôle a une action pour éviter les collisions, `initialisePositionCache` va configurer le `PositionCache` pour qu'il recherche activement les autres PNJ à proximité.
    3.  Le `RoleActivateSystem` fait cela à la création du PNJ, et `OnFlockJoinSystem` le refait si le PNJ rejoint un groupe (`flock`), car ses besoins en détection peuvent changer.

En résumé, `PositionCacheSystems` est un système d'optimisation qui effectue les recherches spatiales coûteuses de manière centralisée et périodique, permettant au reste de l'IA d'être beaucoup plus performant.
