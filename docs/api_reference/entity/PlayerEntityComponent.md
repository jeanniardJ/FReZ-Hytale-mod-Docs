# Joueur (Composant d'Entité)

La classe `Player` représente l'entité de jeu réelle pour un joueur. Elle étend `LivingEntity` (Entité Vivante) et implémente plusieurs interfaces pour l'envoi de commandes, la détention de permissions et la fourniture de métriques. Elle sert de composant principal pour interagir avec l'état d'un joueur, son inventaire, les gestionnaires d'interface utilisateur côté client, le mode de jeu et diverses mécaniques de jeu.

## Constantes

### `public static final MetricsRegistry<Player> METRICS_REGISTRY`
Un `MetricsRegistry` spécifiquement pour les entités `Player`, utilisé pour suivre les statistiques et les métriques liées au joueur.

### `public static final KeyedCodec<PlayerConfigData> PLAYER_CONFIG_DATA`
Un `KeyedCodec` pour la sérialisation et la désérialisation de `PlayerConfigData`, qui contient la configuration persistante et les paramètres d'un joueur.

### `public static final BuilderCodec<Player> CODEC`
Un `BuilderCodec` pour l'entité `Player`, utilisé pour sa sérialisation et sa désérialisation.

### `public static final int DEFAULT_VIEW_RADIUS_CHUNKS = 6`
Le rayon de vue par défaut pour les joueurs, exprimé en "chunks" (morceaux de monde).

### `public static final long RESPAWN_INVULNERABILITY_TIME_NANOS`
La durée (en nanosecondes) pendant laquelle un joueur reste invulnérable après être réapparu.

### `public static final long MAX_TELEPORT_INVULNERABILITY_MILLIS`
La durée maximale (en millisecondes) pendant laquelle un joueur peut rester invulnérable après une téléportation.

## Méthodes Statiques

### `public static ComponentType<EntityStore, Player> getComponentType()`
Récupère le `ComponentType` spécifiquement associé aux composants `Player`. C'est crucial pour accéder ou manipuler les instances de `Player` au sein d'un `EntityStore`.
- **Retourne :** Le `ComponentType` pour `Player`.

### `public static CompletableFuture<Transform> getRespawnPosition(@Nonnull Ref<EntityStore> ref, @Nonnull String worldName, @Nonnull ComponentAccessor<EntityStore> componentAccessor)`
Calcule et retourne de manière asynchrone (en arrière-plan) la position de réapparition optimale pour un joueur dans un monde donné, en tenant compte des points de réapparition configurés et des obstructions potentielles.
- **Paramètres :**
    - `ref` : Une `Ref` (référence) vers l'entité du joueur dans l'`EntityStore`.
    - `worldName` : Le nom du monde dans lequel le joueur réapparaît.
    - `componentAccessor` : Un accesseur pour les composants d'entité, permettant d'accéder aux données des autres composants attachés à cette entité.
- **Retourne :** Un `CompletableFuture<Transform>` qui se résout en `Transform` (position et rotation) de réapparition calculée.

### `public static void setGameMode(@Nonnull Ref<EntityStore> playerRef, @Nonnull GameMode gameMode, @Nonnull ComponentAccessor<EntityStore> componentAccessor)`
Définit le mode de jeu (`GameMode`) pour un joueur spécifique. Cette méthode gère la mise à jour de l'état interne du joueur, l'envoi des paquets appropriés au client et la gestion des permissions associées.
- **Paramètres :**
    - `playerRef` : Une `Ref` vers l'entité du joueur.
    - `gameMode` : Le `GameMode` à définir (par exemple, `Creative`, `Survival`).
    - `componentAccessor` : Un accesseur pour les composants d'entité.

### `public static void initGameMode(@Nonnull Ref<EntityStore> playerRef, @Nonnull ComponentAccessor<EntityStore> componentAccessor)`
Initialise le mode de jeu pour un joueur, généralement appelé lorsqu'un joueur se connecte pour la première fois ou que son entité est créée. Si aucun mode de jeu n'est défini, il utilise le mode de jeu configuré pour le monde.
- **Paramètres :**
    - `playerRef` : Une `Ref` vers l'entité du joueur.
    - `componentAccessor` : Un accesseur pour les composants d'entité.

## Méthodes

### `public void copyFrom(@Nonnull Player oldPlayerComponent)`
Copie l'état et les données pertinentes d'une autre instance de composant `Player`. Ceci est souvent utilisé lors des réapparitions ou des transferts de joueurs pour maintenir la continuité de leur état.
- **Paramètres :**
    - `oldPlayerComponent` : Le composant `Player` à partir duquel copier les données.

### `public void init(@Nonnull UUID uuid, @Nonnull PlayerRef playerRef)`
Initialise le composant `Player` avec l'`UUID` du joueur et son `PlayerRef`. Cette méthode configure divers gestionnaires spécifiques au joueur comme `WindowManager` (gestionnaire de fenêtres) et `PageManager` (gestionnaire de pages d'interface).
- **Paramètres :**
    - `uuid` : L'`UUID` du joueur.
    - `playerRef` : L'instance de `PlayerRef` associée à ce joueur.

### `public Inventory setInventory(Inventory inventory)`
Définit l'`Inventory` (inventaire) actuel du joueur. Cela remplacera l'inventaire existant du joueur par celui fourni.
- **Paramètres :**
    - `inventory` : Le nouvel objet `Inventory` pour le joueur.
- **Retourne :** L'`Inventory` qui a été défini.

### `public PlayerConfigData getPlayerConfigData()`
Récupère l'objet `PlayerConfigData` pour ce joueur, qui contient les paramètres et les données persistantes spécifiques au joueur (ceux qui sont sauvegardés entre les sessions).
- **Retourne :** L'instance de `PlayerConfigData`.

### `public void markNeedsSave()`
Marque les données de configuration du joueur comme "sales" (`dirty`), signalant qu'elles doivent être sauvegardées dans le stockage persistant (sur le disque).

### `public WorldMapTracker getWorldMapTracker()`
Récupère le `WorldMapTracker` (suiveur de carte du monde) pour ce joueur, qui gère les données de carte du monde découvertes par le joueur.
- **Retourne :** L'instance de `WorldMapTracker`.

### `public WindowManager getWindowManager()`
Récupère le `WindowManager` (gestionnaire de fenêtres) pour ce joueur, qui gère l'affichage et la gestion des fenêtres d'interface utilisateur côté client.
- **Retourne :** L'instance de `WindowManager`.

### `public PageManager getPageManager()`
Récupère le `PageManager` (gestionnaire de pages) pour ce joueur, qui gère les différentes pages ou écrans d'interface utilisateur présentés au joueur.
- **Retourne :** L'instance de `PageManager`.

### `public HudManager getHudManager()`
Récupère le `HudManager` (gestionnaire de l'ATH - Affichage Tête Haute) pour ce joueur, qui contrôle divers éléments affichés sur l'ATH du joueur.
- **Retourne :** L'instance de `HudManager`.

### `public HotbarManager getHotbarManager()`
Récupère le `HotbarManager` (gestionnaire de la barre rapide) pour ce joueur, qui gère l'inventaire de la barre rapide du joueur.
- **Retourne :** L'instance de `HotbarManager`.

### `public boolean isFirstSpawn()`
Vérifie si c'est la première fois que le joueur apparaît dans la session actuelle ou sur le serveur.
- **Retourne :** `true` si c'est la première apparition, `false` sinon.

### `public void setFirstSpawn(boolean firstSpawn)`
Définit si cela est considéré comme la première apparition du joueur.
- **Paramètres :**
    - `firstSpawn` : `true` si c'est la première apparition, `false` sinon.

### `public void resetManagers(@Nonnull Holder<EntityStore> holder)`
Réinitialise divers gestionnaires spécifiques au joueur (par exemple, `WorldMapTracker`, `HudManager`, `CameraManager`) à leurs états par défaut. Ceci est souvent appelé lors de la réinitialisation du joueur.
- **Paramètres :**
    - `holder` : Le `Holder` du joueur dans l'`EntityStore`.

### `public boolean isOverrideBlockPlacementRestrictions()`
Vérifie si le joueur a actuellement des restrictions de placement de blocs annulées.
- **Retourne :** `true` si les restrictions sont annulées, `false` sinon.

### `public void setOverrideBlockPlacementRestrictions(@Nonnull Ref<EntityStore> ref, boolean overrideBlockPlacementRestrictions, @Nonnull ComponentAccessor<EntityStore> componentAccessor)`
Définit si les restrictions de placement de blocs du joueur sont annulées. Cela peut être utilisé pour accorder des capacités de construction de type créatif.
- **Paramètres :**
    - `ref` : Une `Ref` vers l'entité du joueur.
    - `overrideBlockPlacementRestrictions` : `true` pour annuler, `false` pour appliquer les restrictions.
    - `componentAccessor` : Un accesseur pour les composants d'entité.

### `public boolean hasPermission(@Nonnull String id)`
Vérifie si le joueur a une permission spécifique identifiée par `id`.
- **Paramètres :**
    - `id` : La chaîne de permission (par exemple, "monplugin.commande.utiliser").
- **Retourne :** `true` si le joueur a la permission, `false` sinon.

### `public boolean hasPermission(@Nonnull String id, boolean def)`
Vérifie si le joueur a une permission spécifique, en fournissant une valeur par défaut si la permission n'est pas explicitement définie.
- **Paramètres :`
    - `id` : La chaîne de permission.
    - `def` : La valeur booléenne par défaut à retourner si la permission n'est pas trouvée.
- **Retourne :** `true` si le joueur a la permission ou si la valeur par défaut est `true`, `false` sinon.

### `public boolean hasSpawnProtection()`
Vérifie si le joueur bénéficie actuellement d'une protection de réapparition (invulnérabilité après la réapparition/téléportation).
- **Retourne :** `true` si le joueur est protégé à la réapparition, `false` sinon.

### `public boolean isWaitingForClientReady()`
Vérifie si le serveur attend actuellement que le client signale qu'il est prêt après une apparition ou une téléportation.
- **Retourne :** `true` si le serveur attend que le client soit prêt, `false` sinon.

### `public void setClientViewRadius(int clientViewRadius)`
Définit le rayon de vue demandé par le client en "chunks". Le rayon de vue réel peut être plafonné par les paramètres du serveur.
- **Paramètres :**
    - `clientViewRadius` : Le rayon de vue souhaité en "chunks".

### `public int getClientViewRadius()`
Récupère le rayon de vue demandé par le client en "chunks".
- **Retourne :** Le rayon de vue demandé par le client.

### `public int getViewRadius()`
Récupère le rayon de vue effectif pour le joueur, qui est le minimum du rayon demandé par le client et du rayon de vue maximal autorisé par le serveur.
- **Retourne :** Le rayon de vue effectif en "chunks".

### `public boolean canDecreaseItemStackDurability(@Nonnull Ref<EntityStore> ref, @Nonnull ComponentAccessor<EntityStore> componentAccessor)`
Détermine si la durabilité d'un objet (`ItemStack`) peut diminuer pour ce joueur, généralement en fonction de son mode de jeu (par exemple, la durabilité ne diminue pas en mode Créatif).
- **Paramètres :**
    - `ref` : Une `Ref` vers l'entité du joueur.
    - `componentAccessor` : Un accesseur pour les composants d'entité.
- **Retourne :** `true` si la durabilité peut diminuer, `false` sinon.

### `public boolean canApplyItemStackPenalties(@Nonnull Ref<EntityStore> ref, @Nonnull ComponentAccessor<EntityStore> componentAccessor)`
Détermine si des pénalités d'objet (par exemple, casser un objet) peuvent être appliquées à ce joueur.
- **Paramètres :**
    - `ref` : Une `Ref` vers l'entité du joueur.
    - `componentAccessor` : Un accesseur pour les composants d'entité.
- **Retourne :** `true` si des pénalités peuvent être appliquées, `false` sinon.

### `public ItemStackSlotTransaction updateItemStackDurability(@Nonnull Ref<EntityStore> ref, @Nonnull ItemStack itemStack, ItemContainer container, int slotId, double durabilityChange, @Nonnull ComponentAccessor<EntityStore> componentAccessor)`
Met à jour la durabilité d'un `ItemStack` dans l'inventaire du joueur, en appliquant la logique nécessaire pour la casse de l'objet et l'envoi de notifications.
- **Paramètres :**
    - `ref` : Une `Ref` vers l'entité du joueur.
    - `itemStack` : L'`ItemStack` dont la durabilité est mise à jour.
    - `container` : Le `ItemContainer` qui contient l'objet.
    - `slotId` : L'ID de l'emplacement de l'objet dans le conteneur.
    - `durabilityChange` : La quantité de changement de durabilité (positive pour une augmentation, négative pour une diminution).
    - `componentAccessor` : Un accesseur pour les composants d'entité.
- **Retourne :** Une `ItemStackSlotTransaction` si la durabilité a été mise à jour, `null` sinon.

### `public GameMode getGameMode()`
Récupère le `GameMode` (mode de jeu) actuel du joueur.
- **Retourne :** Le `GameMode` du joueur.

### `public String getDisplayName()`
Retourne le nom d'affichage du joueur, qui est généralement son nom d'utilisateur.
- **Retourne :** Le nom d'affichage du joueur sous forme de `String`.

## Méthodes Dépréciées

### `public PacketHandler getPlayerConnection()`
*Déprécié.* Utilisez `PlayerRef.getPacketHandler()` à la place pour obtenir le gestionnaire de paquets du joueur. Cette méthode existe pour la compatibilité, mais la nouvelle approche est de passer par `PlayerRef`.

### `public PlayerRef getPlayerRef()`
*Déprécié.* Récupère l'instance `PlayerRef` associée. Cette méthode est dépréciée car le composant `Player` fonctionne généralement avec sa propre `Ref<EntityStore>` et `ComponentAccessor`. Elle est moins directe que les nouvelles méthodes basées sur les composants.
