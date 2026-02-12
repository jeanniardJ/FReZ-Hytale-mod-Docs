# NPCSystems

La classe `NPCSystems` sert de conteneur pour une collection de "systèmes" ECS (Entity Component System) statiques et imbriqués. Chaque système est responsable de la gestion d'aspects spécifiques du comportement et du cycle de vie des PNJ (Personnages Non Joueurs) au sein du serveur Hytale. Ces systèmes sont des pièces maîtresses de l'IA des PNJ, traitant des événements tels que l'ajout, la suppression, la mort, la téléportation et les changements de modèle des PNJ.

Pour les développeurs de mods, ces classes sont principalement des mécanismes internes du serveur. Cependant, comprendre leur existence et leur rôle peut aider à mieux appréhender le fonctionnement des PNJ et à diagnostiquer les problèmes ou à anticiper les comportements lors de la création de mods qui interagissent avec les PNJ.

## Systèmes Imbriqués

### `NPCSystems.AddedSystem`
Gère la logique qui se déclenche lorsqu'une entité PNJ est ajoutée à ou retirée du "store" (le système de stockage des entités).
-   **Rôle :** Initialise le rôle du PNJ, ses vues sur le "blackboard" (tableau noir d'IA), et gère l'ajout/suppression des composants de base.
-   **Méthodes Clés :**
    -   `onEntityAdded()` : Appelé lors de l'ajout d'une entité PNJ.
    -   `onEntityRemove()` : Appelé lors de la suppression d'une entité PNJ.

### `NPCSystems.AddSpawnEntityEffectSystem`
Applique les effets d'apparition (spawn effects) aux PNJ, basés sur les ressources d'équilibrage définies dans leur rôle.
-   **Rôle :** Assure que les PNJ reçoivent les effets visuels ou fonctionnels appropriés dès leur apparition.

### `NPCSystems.AddedFromExternalSystem`
Gère les PNJ qui sont ajoutés au monde à partir de sources externes, telles que la génération de monde (`worldgen`) ou les "prefabs" (structures préconstruites).
-   **Rôle :** Initialise la position, la rotation et d'autres propriétés du PNJ en fonction de son origine externe.

### `NPCSystems.AddedFromWorldGenSystem`
Système spécifique pour les PNJ générés par le système de génération de monde.
-   **Rôle :** S'assure que les PNJ générés par le monde reçoivent un `WorldGenId` pour leur traçabilité.

### `NPCSystems.OnTeleportSystem`
Gère le comportement des PNJ lorsqu'ils sont téléportés entre différents mondes.
-   **Rôle :** Informe le rôle du PNJ de l'événement de téléportation, permettant d'adapter son comportement ou son état.

### `NPCSystems.OnDeathSystem`
Gère la logique qui se produit lorsqu'un PNJ meurt.
-   **Rôle :** Applique la logique de "retrait différé de cadavre" (DeferredCorpseRemoval), permettant aux animations de mort de se terminer avant la suppression de l'entité.

### `NPCSystems.ModelChangeSystem`
S'assure que les contrôleurs de mouvement des PNJ sont correctement mis à jour lorsque leur modèle visuel change.
-   **Rôle :** Adapte le comportement de mouvement du PNJ si son apparence physique est modifiée.

### `NPCSystems.LegacyWorldGenId`
*Déprécié.* Un système pour gérer les anciens ID de génération de monde. Il est marqué comme déprécié et destiné à être retiré.

### `NPCSystems.KillFeedKillerEventSystem`
Gère les messages du "kill feed" (journal des éliminations) lorsque le PNJ est l'entité qui a effectué la mise à mort.
-   **Rôle :** Personnalise le message affiché dans le journal des éliminations lorsque le PNJ est le tueur.

### `NPCSystems.KillFeedDecedentEventSystem`
Gère les messages du "kill feed" lorsque le PNJ est la victime (celui qui est mort).
-   **Rôle :** Annule ou adapte le message affiché dans le journal des éliminations lorsque le PNJ est la victime.

### `NPCSystems.PrefabPlaceEntityEventSystem`
Gère la réaffectation de l'appartenance au groupe (`FlockMembership`) lorsqu'un PNJ est placé à partir d'un "prefab" (structure préfabriquée).
-   **Rôle :** Assure que les PNJ provenant de "prefabs" sont correctement intégrés dans les systèmes de gestion de groupe.
