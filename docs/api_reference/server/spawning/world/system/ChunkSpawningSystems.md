# ChunkSpawningSystems

La classe `ChunkSpawningSystems` regroupe un ensemble de "systèmes" ECS (Entity Component System) statiques et imbriqués qui gèrent la logique de l'apparition d'entités (spawning) dans les "chunks" (morceaux de monde) du serveur Hytale. Ces systèmes sont cruciaux pour déterminer où et quand les PNJ ou autres entités devraient apparaître dans le monde.

Pour les développeurs de mods, ces classes sont des mécanismes très internes. Vous n'aurez probablement pas à interagir directement avec elles, mais comprendre leur rôle peut éclairer la manière dont le jeu gère l'apparition des entités en arrière-plan.

## Méthodes Statiques Internes

Ces méthodes gèrent la logique de base du traitement des chunks pour l'apparition et sont utilisées par les systèmes imbriqués.

### `protected static boolean processStoppedChunk(@Nonnull Ref<ChunkStore> ref, @Nonnull Store<ChunkStore> store, @Nonnull WorldSpawnData worldSpawnData, @Nonnull ComponentType<ChunkStore, ChunkSpawnData> chunkSpawnDataComponentType, @Nonnull CommandBuffer<ChunkStore> commandBuffer)`
Traite un "chunk" qui a cessé d'être actif pour l'apparition. Cela implique de nettoyer les données d'apparition associées au chunk et de mettre à jour les statistiques globales d'apparition du monde.

### `protected static boolean processStartedChunk(@Nonnull Ref<ChunkStore> ref, @Nonnull Store<ChunkStore> store, @Nonnull WorldChunk worldChunk, @Nonnull WorldSpawnData worldSpawnData, @Nonnull ComponentType<ChunkStore, ChunkSpawnData> chunkSpawnDataComponentType, @Nonnull ComponentType<ChunkStore, ChunkSpawnedNPCData> chunkSpawnedNPCDataComponentType, @Nonnull CommandBuffer<ChunkStore> commandBuffer)`
Traite un "chunk" qui est devenu actif pour l'apparition. Cela initialise les données d'apparition du chunk et les intègre dans le système global d'apparition du monde.

### `protected static void updateChunkCount(int newChunks, @Nonnull WorldSpawnData worldSpawnData)`
Met à jour le nombre de "chunks" gérés par les données d'apparition du monde.

## Systèmes Imbriqués

### `ChunkSpawningSystems.ChunkRefAdded`
Un système `RefSystem` qui est activé lorsqu'une référence à un "chunk" est ajoutée. Cela signifie qu'un nouveau chunk devient actif et doit potentiellement être traité pour l'apparition d'entités.
- **Rôle :** Initialise le processus d'apparition pour les chunks nouvellement ajoutés.

### `ChunkSpawningSystems.TickingState`
Un système `RefChangeSystem` qui réagit aux changements de l'état "ticking" (actif) d'un "chunk". Cela signifie qu'il gère les transitions lorsqu'un chunk commence ou cesse d'être traité pour la logique du jeu, y compris l'apparition.
-   **Rôle :** Met à jour l'état d'apparition d'un chunk lorsqu'il devient ou cesse d'être "ticking", gérant ainsi l'ajout ou la suppression de chunks du pool d'apparition.
