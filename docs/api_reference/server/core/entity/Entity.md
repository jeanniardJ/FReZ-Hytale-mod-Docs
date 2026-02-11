# Entité (Entity)

La classe `Entity` représente tout objet ou être interactif dans le monde de Hytale. Cela inclut les joueurs, les PNJ (Personnages Non Joueurs), les créatures, les objets, et bien plus encore. Les entités sont construites sur une architecture basée sur les composants (`component-based architecture`), ce qui signifie que leurs comportements et propriétés sont définis par divers `Component`s (composants) qui leur sont attachés et stockés dans un `EntityStore`.

De nombreuses méthodes liées à la manipulation directe des propriétés d'entité peuvent être marquées `@Deprecated` (dépréciées) ou `forRemoval = true` (destinées à être supprimées). Cela indique une évolution vers une approche davantage axée sur les composants pour modifier le comportement et les données des entités. Les développeurs de mods devraient privilégier l'interaction avec les entités via leurs `Component`s attachés et l'`EntityStore` lorsque cela est possible.

## Constantes

### `public static final int VERSION = 5`
La version actuelle du format de sérialisation (sauvegarde et chargement) des entités.

### `public static final KeyedCodec<Model.ModelReference> MODEL`
Un `KeyedCodec` utilisé pour l'encodage et le décodage du composant `Model.ModelReference` d'une entité, qui définit son modèle visuel. Un "codec" est un système pour convertir des données d'un format à un autre.

### `public static final KeyedCodec<String> DISPLAY_NAME`
Un `KeyedCodec` utilisé pour l'encodage et le décodage du nom affiché d'une entité.

### `public static final KeyedCodec<UUID> UUID`
Un `KeyedCodec` utilisé pour l'encodage et le décodage de l'`UUID` unique (identifiant universel unique) d'une entité.

### `public static final BuilderCodec<Entity> CODEC`
Un `BuilderCodec` utilisé pour la sérialisation et la désérialisation des objets `Entity`.

### `public static final int UNASSIGNED_ID = -1`
Une constante représentant un ID réseau non assigné pour une entité. Un ID réseau est un identifiant temporaire utilisé par le serveur pour suivre les entités en ligne.

## Constructeurs

### `public Entity()`
Le constructeur par défaut pour créer une instance de `Entity`.

### `public Entity(@Nullable World world)`
*Déprécié.* Un constructeur qui initialise une `Entity` et l'associe à un `World` spécifique. Cette méthode est dépréciée, ce qui suggère que les entités devraient être créées et gérées par des systèmes basés sur des composants plus explicites ou des méthodes de création d'entités dédiées.

## Méthodes

### `public boolean remove()`
Tente de supprimer l'entité de son monde actuel. Cela déclenchera un `EntityRemoveEvent` (un événement de suppression d'entité) et effacera la référence de l'entité dans l'`EntityStore`.
- **Retourne :** `true` si l'entité a été marquée avec succès pour suppression, `false` sinon (par exemple, si elle était déjà supprimée).

### `public void loadIntoWorld(@Nonnull World world)`
Charge cette entité dans le `World` spécifié. Cette méthode définit le `World` associé à l'entité et lui attribue un ID réseau si ce n'est pas déjà fait.
- **Paramètres :**
    - `world` : L'instance du `World` dans lequel l'entité doit être chargée.
- **Lance :** `IllegalArgumentException` si l'entité est déjà dans un monde.

### `public void unloadFromWorld()`
Décharge l'entité de son `World` actuel. Cela efface l'association de l'entité avec le `World` et réinitialise son ID réseau.
- **Lance :** `IllegalArgumentException` si l'entité n'est pas actuellement dans un monde.

### `public World getWorld()`
Récupère l'instance du `World` dans lequel cette entité est actuellement chargée.
- **Retourne :** L'instance du `World`, ou `null` si l'entité n'est pas actuellement dans un monde.

### `public boolean wasRemoved()`
Vérifie si cette entité a été marquée pour suppression.
- **Retourne :** `true` si l'entité a été supprimée, `false` sinon.

### `public boolean isCollidable()`
Vérifie si cette entité est considérée comme "collidable" (c'est-à-dire si elle peut entrer en collision avec d'autres objets).
- **Retourne :** `true` si l'entité peut entrer en collision avec d'autres objets, `false` sinon.

### `public boolean isHiddenFromLivingEntity(@Nonnull Ref<EntityStore> ref, @Nonnull Ref<EntityStore> targetRef, @Nonnull ComponentAccessor<EntityStore> componentAccessor)`
Détermine si cette entité doit être cachée à une entité vivante spécifiée.
- **Paramètres :**
    - `ref` : Une `Ref` vers cette entité dans l'`EntityStore`.
    - `targetRef` : Une `Ref` vers l'entité vivante cible.
    - `componentAccessor` : Un accesseur pour les composants d'entité.
- **Retourne :** `true` si cette entité doit être cachée à l'entité cible, `false` sinon.

### `public void setReference(@Nonnull Ref<EntityStore> reference)`
Définit la `Ref` interne qui pointe vers les données de cette entité dans l'`EntityStore`. Cette méthode doit généralement être gérée par le système d'entités lui-même, pas directement par les développeurs de mods.
- **Paramètres :**
    - `reference` : La `Ref` à associer à cette entité.
- **Lance :** `IllegalArgumentException` si l'entité a déjà une référence valide.

### `public Ref<EntityStore> getReference()`
Récupère la `Ref` qui pointe vers les données de cette entité dans l'`EntityStore`.
- **Retourne :** La `Ref<EntityStore>` pour cette entité, ou `null` si non définie.

### `public Component<EntityStore> clone()`
Crée un "clone" (une copie en profondeur) de cette instance de `Entity`, incluant tous ses composants.
- **Retourne :** Une nouvelle instance de `Entity` qui est une copie de l'actuelle.

### `public Holder<EntityStore> toHolder()`
Convertit cette instance d'`Entity` et ses composants associés en un `Holder<EntityStore>`, qui est un conteneur pour les données d'entité utilisé par le système de composants.
- **Retourne :** Un `Holder<EntityStore>` contenant les données de l'entité.

## Méthodes Dépréciées

Les méthodes suivantes sont dépréciées et devraient être évitées dans le nouveau code. Elles sont probablement conservées pour la compatibilité ascendante ou sont progressivement remplacées par des interactions basées sur les composants.

### `public void setLegacyUUID(@Nullable UUID uuid)`
*Déprécié.* Définit un `UUID` "legacy" (ancien format) pour l'entité.

### `public int getNetworkId()`
*Déprécié.* Récupère l'ID réseau attribué à cette entité.

### `public String getLegacyDisplayName()`
*Déprécié.* Récupère l'ancien nom affiché de l'entité.

### `public UUID getUuid()`
*Déprécié.* Récupère l'`UUID` de l'entité.

### `public void setTransformComponent(TransformComponent transform)`
*Déprécié.* Définit le `TransformComponent` (composant de transformation) pour l'entité.

### `public TransformComponent getTransformComponent()`
*Déprécié.* Récupère le `TransformComponent` de l'entité.

### `public void moveTo(@Nonnull Ref<EntityStore> ref, double locX, double locY, double locZ, @Nonnull ComponentAccessor<EntityStore> componentAccessor)`
*Déprécié.* Déplace l'entité aux coordonnées spécifiées.

### `public void clearReference()`
*Déprécié.* Efface la référence interne de l'entité.

## Classe Imbriquée : `DefaultAnimations`

La classe statique imbriquée `DefaultAnimations` fournit des méthodes utilitaires et des constantes pour les ID d'animation courants.

### Constantes

-   `public static final String DEATH = "Death"` (Animation de mort)
-   `public static final String HURT = "Hurt"` (Animation de blessure)
-   `public static final String DESPAWN = "Despawn"` (Animation de disparition)
-   `public static final String SWIM_SUFFIX = "Swim"` (Suffixe pour les animations de nage, par exemple "HurtSwim")
-   `public static final String FLY_SUFFIX = "Fly"` (Suffixe pour les animations de vol, par exemple "HurtFly")

### Méthodes

### `public static String[] getHurtAnimationIds(@Nonnull MovementStates movementStates, @Nonnull DamageCause damageCause)`
Génère un tableau d'ID d'animation pertinents pour être blessé, en tenant compte des `MovementStates` (états de mouvement) de l'entité (par exemple, nage, vol) et de la `DamageCause` (cause des dégâts).
- **Paramètres :**
    - `movementStates` : Les `MovementStates` actuels de l'entité.
    - `damageCause` : La `DamageCause` qui a infligé les dégâts.
- **Retourne :** Un tableau de `String` (ID d'animation).

### `public static String[] getDeathAnimationIds(@Nonnull MovementStates movementStates, @Nonnull DamageCause damageCause)`
Génère un tableau d'ID d'animation pertinents pour la mort de l'entité, en tenant compte de ses `MovementStates` et de la `DamageCause`.
- **Paramètres :**
    - `movementStates` : Les `MovementStates` actuels de l'entité.
    - `damageCause` : La `DamageCause` qui a conduit à la mort.
- **Retourne :** Un tableau de `String` (ID d'animation).
