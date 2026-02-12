# 🥚 Système de Spawning de PNJ (NPC Spawning System)

Ce document décrit le fonctionnement interne du système de "spawning" (apparition) des PNJ, en particulier comment le jeu trouve des emplacements adaptés pour faire apparaître de nouvelles entités.

## `FloodFillPositionSelector`

Cette classe est un composant attaché à une entité (souvent une "balise de spawn") qui est responsable de trouver les meilleures positions pour l'apparition des PNJ dans une zone donnée. C'est un processus complexe et optimisé pour la performance.

*   **Objectif** : Identifier efficacement et intelligemment des emplacements valides dans le monde pour le spawning d'un ou plusieurs PNJ, en tenant compte de nombreux critères.
*   **Algorithme** : Utilise un algorithme de "flood fill" (remplissage par inondation) qui part d'un point central et explore les blocs voisins pour trouver des surfaces adaptées.

### Fonctionnement Détaillé

1.  **Initialisation** : Le `FloodFillPositionSelector` est attaché à un objet de spawn (par exemple, un `BeaconSpawnWrapper`). Il définit une zone de recherche autour de lui.
2.  **`floodFill()` - Recherche des Hauteurs** :
    *   À partir du centre de la zone, un algorithme de flood fill explore les blocs.
    *   Pour chaque colonne `(x, z)`, il détermine la hauteur `y` d'une surface solide où un PNJ pourrait potentiellement se tenir.
    *   Il prend en compte les limites `minY` et `maxY` (plage de hauteur valide).
    *   Les résultats sont stockés dans une grille `heightGrid`.
3.  **`findPositions()` - Sélection Optimisée** :
    *   Une fois la grille de hauteurs remplie, le système doit sélectionner les meilleures positions.
    *   Il utilise des **`resolutionMaps`** (cartes de résolution) pour représenter la zone à différentes échelles. Cela permet d'éliminer rapidement de grandes zones qui ne contiennent pas assez de spots de spawn.
    *   Les positions sont sélectionnées en tenant compte de leur "poids" (`WeightedPosition`), qui peut dépendre de la distance au joueur, par exemple.
    *   Il vérifie si le PNJ peut *réellement* apparaître à cette position en utilisant un `SpawningContext`. Ce contexte vérifie des critères comme le type de bloc au sol, le niveau de lumière, la présence de fluides, et si la zone n'est pas "supprimée" par un `SpawnSuppressionController` (par exemple, si un joueur a construit une base).
    *   Il contient également une logique pour "décaler" les positions si elles sont trop près d'un mur (`shiftIndexAwayFromWall`).

### Critères de Spawning

Avant de valider une position, le `FloodFillPositionSelector` vérifie :

*   **Hauteur** : La position est-elle dans la plage `minY/maxY` ?
*   **Bloc au sol** : Est-ce un type de bloc valide pour le PNJ (`spawnBlockSet`)?
*   **Fluide** : Le PNJ peut-il apparaître dans ce type de fluide (`spawnFluidTag`)?
*   **Lumière** : Le niveau de lumière est-il approprié (`withinLightRange`)?
*   **Suppression** : La zone est-elle activement supprimée ?

### Débogage

La classe inclut des outils de débogage (`debugDump...`) pour visualiser le processus de flood fill et la sélection des positions, ce qui est très utile pour ajuster les comportements de spawning.

En résumé, `FloodFillPositionSelector` est un composant clé pour la gestion dynamique et intelligente de l'apparition des PNJ dans le monde de Hytale, en s'assurant que les PNJ apparaissent aux bons endroits et au bon moment.

## `WorldSpawnManager`

`WorldSpawnManager` est la classe centrale qui supervise l'ensemble du système de spawning de PNJ pour le monde. Son rôle est d'orchestrer l'apparition des PNJ en fonction des configurations, des environnements et des règles du jeu.

### Responsabilités Clés

1.  **Gestion des Configurations de Spawn** :
    *   Charge (`addSpawnWrapper`) et décharge (`removeSpawnWrapper`) les règles de spawning définies dans les assets du jeu (chaque `WorldSpawnWrapper` contient un `WorldNPCSpawn`).
    *   Chaque configuration spécifie quels PNJ apparaissent, dans quels environnements, et selon quelles conditions.

2.  **Spawning Adapté à l'Environnement** :
    *   Associe les règles de spawning à des types d'environnement spécifiques (par exemple, "forêt", "désert", "caverne").
    *   Le `environmentSpawnParametersMap` stocke des informations comme la densité de spawn pour chaque environnement.
    *   Les `EnvironmentSpawnParameters` peuvent être mis à jour dynamiquement (`updateSpawnParameters`) si l'environnement change (par exemple, un biome transformé).

3.  **Prévention des Conflits de Règle** :
    *   `npcEnvCombinations`: Garde une trace des combinaisons PNJ-Environnement déjà définies pour éviter d'appliquer des règles de spawning contradictoires pour un même PNJ dans un même environnement.

4.  **Suivi des PNJ Spawns** :
    *   `trackNPCs` et `untrackNPCs`: Ces méthodes sont utilisées pour compter les PNJ apparus afin de respecter les limites de densité par environnement et par type de PNJ. Cela garantit que le monde ne soit ni surpeuplé, ni vide.

5.  **Mises à Jour Dynamiques** :
    *   `rebuildConfigurations`: Permet de recharger et de réappliquer les règles de spawning sans avoir à redémarrer le serveur, ce qui est utile pour les modifications en jeu ou via des plugins.
    *   `onEnvironmentChanged`: Déclenché lorsque des aspects de l'environnement changent (comme la phase de la lune ou des modifications de biome), ce qui peut impacter les densités et les types de PNJ à faire apparaître.

En bref, `WorldSpawnManager` est le chef d'orchestre qui s'assure que le monde est peuplé de PNJ de manière cohérente et équilibrée, en respectant les intentions des designers et les contraintes du jeu. Il utilise des classes comme `FloodFillPositionSelector` pour trouver les positions spécifiques.

## `ChunkSpawningSystems`

Ces systèmes gèrent le processus de spawning au niveau des "chunks", les blocs de monde qui sont chargés et déchargés. Ils s'assurent que les données de spawning sont correctement initialisées et nettoyées lorsque les chunks changent d'état.

### `processStartedChunk()`

Cette méthode est appelée lorsqu'un chunk devient "actif" pour le spawning (par exemple, il est chargé et devient visible par les joueurs).

*   **Initialisation des données de spawn du chunk** : Crée un composant `ChunkSpawnData` pour le chunk, qui stocke les informations de spawning spécifiques à ce chunk.
*   **Pré-traitement de l'environnement** : Analyse les colonnes d'environnement du chunk pour identifier quels environnements sont présents (forêt, désert, etc.) et à quelle densité de blocs spawnables.
*   **Enregistrement avec le `WorldSpawnData`** : Le chunk est enregistré auprès du `WorldSpawnData` global, qui maintient un décompte total des segments (zones spawnables) pour chaque environnement dans le monde.

### `processStoppedChunk()`

Cette méthode est appelée lorsqu'un chunk n'est plus actif pour le spawning (par exemple, il est déchargé ou mis en pause).

*   **Nettoyage** : Supprime le composant `ChunkSpawnData` et désenregistre le chunk du `WorldSpawnData`, réduisant ainsi les décomptes de segments.

### `ChunkRefAdded` (Système d'ajout de chunk)

Ce système détecte quand un chunk est ajouté et actif.

*   **Déclencheur** : Un `Ref` (référence) à un `ChunkStore` (un chunk) est ajouté et qu'il est "ticking" (actif pour la logique de jeu).
*   **Action** : Appelle `processStartedChunk()` pour configurer le chunk pour le spawning et met à jour le nombre total de chunks gérés.

### `TickingState` (Système d'état d'activité)

Ce système réagit aux changements de l'état "ticking" (actif/inactif) d'un chunk.

*   **Déclencheur** : Un composant `NonTicking` est ajouté (le chunk devient inactif) ou retiré (le chunk redevient actif).
*   **Action** : Appelle `processStoppedChunk()` ou `processStartedChunk()` en conséquence, assurant que le système de spawning s'adapte à l'activité du chunk.

Ces systèmes sont essentiels pour que le système de spawning soit dynamique et performant, en ne traitant que les chunks pertinents et en gardant les données à jour au fur et à mesure que les joueurs se déplacent dans le monde.
