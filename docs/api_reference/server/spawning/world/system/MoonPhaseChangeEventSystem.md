# MoonPhaseChangeEventSystem

La classe `MoonPhaseChangeEventSystem` est un système ECS (`WorldEventSystem`) qui réagit spécifiquement aux changements de phase lunaire (`MoonPhaseChangeEvent`). Dans Hytale, la phase de la lune peut avoir un impact significatif sur l'apparition d'entités (spawning) dans le monde. Ce système met à jour les données d'apparition pour refléter ces changements.

## Constructeur

### `public MoonPhaseChangeEventSystem()`
Initialise une nouvelle instance du système. Le système est configuré pour écouter les événements de `MoonPhaseChangeEvent`.

## Méthodes

### `public void handle(@Nonnull Store<EntityStore> store, @Nonnull CommandBuffer<EntityStore> commandBuffer, @Nonnull MoonPhaseChangeEvent event)`
Cette méthode est appelée chaque fois qu'un `MoonPhaseChangeEvent` est envoyé. Elle met à jour les poids d'apparition pour chaque environnement en fonction de la nouvelle phase lunaire.
- **Paramètres :**
    - `store` : Le `Store` de l'`EntityStore` sur lequel le système opère.
    - `commandBuffer` : Un `CommandBuffer` pour effectuer des modifications ou des actions sur l'ECS.
    - `event` : L'`MoonPhaseChangeEvent` qui a déclenché ce système, contenant des informations sur la nouvelle phase lunaire.
