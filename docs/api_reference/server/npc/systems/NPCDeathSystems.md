# NPCDeathSystems

La classe `NPCDeathSystems` regroupe un ensemble de systèmes qui gèrent la logique complexe associée à la mort des entités PNJ (Personnages Non Joueurs). Dans le contexte de l'architecture basée sur les composants (ECS), un "système" est une logique qui opère sur les entités possédant des composants spécifiques. Ces systèmes sont essentiels pour définir ce qui se passe lorsqu'un PNJ meurt, y compris les conséquences pour l'entité qui l'a tué, et la réaction du monde du jeu.

## Classes Imbriquées Statiques

### `NPCDeathSystems.NPCKillsEntitySystem`

Ce système est déclenché lorsqu'une entité meurt et que la source des dégâts (le tueur) est un PNJ. Il gère la logique spécifique qui se produit lorsqu'un PNJ tue une autre entité, comme la mise à jour des statistiques de dégâts du PNJ tueur.

#### Méthodes

##### `public Query<EntityStore> getQuery()`
Définit la requête qui identifie les entités pertinentes pour ce système. Pour ce système, il recherche toutes les entités vivantes ayant un `TransformComponent`.
- **Retourne :** Une `Query` pour l'`EntityStore`.

##### `public void onComponentAdded(@Nonnull Ref<EntityStore> ref, @Nonnull DeathComponent component, @Nonnull Store<EntityStore> store, @Nonnull CommandBuffer<EntityStore> commandBuffer)`
Cette méthode est appelée lorsqu'un `DeathComponent` est ajouté à une entité, signalant sa mort. Elle vérifie si la source des dégâts est un PNJ et met à jour les données de ce PNJ en conséquence.
- **Paramètres :**
    - `ref` : Une `Ref` vers l'entité qui est morte.
    - `component` : Le `DeathComponent` qui a été ajouté.
    - `store` : L'`EntityStore` contenant l'entité.
    - `commandBuffer` : Un `CommandBuffer` pour effectuer des modifications.

### `NPCDeathSystems.EntityViewSystem`

Ce système est également déclenché à la mort d'une entité et gère la logique de détection et de signalement de cet événement, notamment en lien avec les joueurs, en particulier ceux en mode créatif. Il s'assure que les événements de mort sont correctement enregistrés dans le "Blackboard" (tableau noir) du PNJ pour que d'autres systèmes d'IA puissent y réagir.

#### Méthodes

##### `public Query<EntityStore> getQuery()`
Définit la requête qui identifie les entités pertinentes pour ce système (PNJ ou joueurs avec un `TransformComponent`).
- **Retourne :** Une `Query` pour l'`EntityStore`.

##### `public void onComponentAdded(@Nonnull Ref<EntityStore> ref, @Nonnull DeathComponent component, @Nonnull Store<EntityStore> store, @Nonnull CommandBuffer<EntityStore> commandBuffer)`
Cette méthode est appelée lorsqu'un `DeathComponent` est ajouté à une entité. Elle vérifie si un joueur en mode créatif est la source des dégâts et enregistre l'événement de mort dans le "Blackboard" du PNJ pour une détection par l'IA.
- **Paramètres :**
    - `ref` : Une `Ref` vers l'entité qui est morte.
    - `component` : Le `DeathComponent` qui a été ajouté.
    - `store` : L'`EntityStore` contenant l'entité.
    - `commandBuffer` : Un `CommandBuffer` pour effectuer des modifications.
