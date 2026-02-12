# WorldSpawnJobSystems

La classe `WorldSpawnJobSystems` regroupe un ensemble de "systèmes" ECS (Entity Component System) statiques et imbriqués qui gèrent les tâches d'apparition (spawning) d'entités dans le monde. Ces systèmes sont le moteur des processus d'apparition, évaluant les conditions, sélectionnant les emplacements et instanciant les entités dans le jeu.

Pour les développeurs de mods, ces classes sont des mécanismes très internes du serveur. Elles détaillent le fonctionnement précis de l'apparition des entités, mais les développeurs interagiront plus probablement avec des APIs de spawning de plus haut niveau pour ajouter ou modifier des comportements d'apparition.

## Méthodes Statiques Internes

Ces méthodes gèrent la logique centrale des jobs de spawning et sont utilisées par les systèmes imbriqués.

### `private static Result run(@Nonnull SpawnJobData spawnJobData, @Nonnull WorldChunk chunk, @Nonnull ChunkEnvironmentSpawnData chunkEnvironmentSpawnData, @Nonnull WorldSpawnData worldSpawnData, @Nonnull SpawnSuppressionController spawnSuppressionController)`
Exécute un job de spawning pour un PNJ. C'est la méthode principale qui coordonne le processus de tentative d'apparition, incluant la vérification des conditions et la sélection d'un emplacement.
- **Retourne :** Un `Result` indiquant si le job a réussi, échoué ou doit être réessayé.

### `private static ISpawnableWithModel getSpawnable(int roleIndex)`
Récupère l'objet "spawnable" (l'entité qui peut apparaître) correspondant à un index de rôle donné.
- **Retourne :** L'`ISpawnableWithModel` si le rôle est spawnable, `null` sinon.
- **Lance :** `IllegalArgumentException` si le rôle n'est pas spawnable ou si l'interface `ISpawnableWithModel` est manquante.

### `private static Result trySpawn(...)`
Tente de faire apparaître une entité à un emplacement potentiel. Cette méthode effectue toutes les vérifications nécessaires (conditions de lumière, blocs, fluides) avant de valider l'apparition.
- **Retourne :** Un `Result` en fonction du succès ou de l'échec de la tentative d'apparition.

### `private static Result spawn(...)`
Crée et place l'entité dans le monde après que toutes les vérifications de `trySpawn` ont réussi.

### `private static void preAddToWorld(...)`
Effectue des actions avant que le PNJ soit ajouté au monde, telles que la configuration de son index de rôle d'apparition ou de son état gelé.

### `private static boolean canSpawnOnBlock(...)`
Vérifie si une entité peut apparaître sur un bloc ou dans un fluide spécifique.

### `private static void rejectSpan(...)`
Enregistre une raison de rejet d'apparition pour les statistiques.

### `protected static Result endProbing(...)`
Termine le processus de "sondage" (probing) d'un job de spawning, met à jour les statistiques et ajuste l'état du job.

### `private static void updateSpawnStats(...)`
Met à jour les statistiques d'apparition globales du monde après qu'un job de spawning est terminé.

### `private static String getSpawnableName(...)`
Récupère le nom d'une entité "spawnable" par son index de rôle.

## Systèmes Imbriqués

### `WorldSpawnJobSystems.EntityRemoved`
Un système `HolderSystem` qui s'active lorsqu'un job de spawning est supprimé (par exemple, si le chunk devient inactif).
- **Rôle :** Nettoie les jobs de spawning inachevés lorsque leur entité associée est supprimée.

### `WorldSpawnJobSystems.TickingState`
Un système `RefChangeSystem` qui gère les jobs de spawning en fonction des changements de l'état "ticking" (actif) d'un "chunk".
-   **Rôle :** Annule les jobs de spawning pour les chunks qui deviennent non-ticking.

### `WorldSpawnJobSystems.Ticking`
Le système principal qui exécute les jobs de spawning. Il est appelé à chaque "tick" du jeu.
-   **Rôle :** Parcourt les jobs de spawning actifs, exécute la logique d'apparition (via la méthode `run`), et met à jour l'état du job et les statistiques.

## Énumération Imbriquée : `Result`

### `SUCCESS`
Le job de spawning a réussi et une entité a été apparue.

### `FAILED`
Le job de spawning a échoué et aucune entité n'a pu être apparue pour le moment.

### `TRY_AGAIN`
Le job de spawning n'a pas pu être terminé en un seul "tick" et doit être réessayé lors du prochain "tick".

### `PERMANENT_FAILURE`
Le job de spawning a rencontré une erreur irrécupérable et ne doit pas être réessayé.
