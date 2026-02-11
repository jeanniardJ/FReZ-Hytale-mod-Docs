# Player

The `Player` class represents the actual in-game entity for a player, extending `LivingEntity` and implementing several interfaces for command sending, permission holding, and metrics provision. It serves as the primary component for interacting with a player's state, inventory, client-side UI managers, game mode, and various in-game mechanics.

## Constants

### `public static final MetricsRegistry<Player> METRICS_REGISTRY`
A `MetricsRegistry` specifically for `Player` entities, used for tracking player-related statistics and metrics.

### `public static final KeyedCodec<PlayerConfigData> PLAYER_CONFIG_DATA`
A `KeyedCodec` for serializing and deserializing `PlayerConfigData`, which holds persistent configuration and settings for a player.

### `public static final BuilderCodec<Player> CODEC`
A `BuilderCodec` for the `Player` entity, used for its serialization and deserialization.

### `public static final int DEFAULT_VIEW_RADIUS_CHUNKS = 6`
The default view radius for players, in chunks.

### `public static final long RESPAWN_INVULNERABILITY_TIME_NANOS`
The duration (in nanoseconds) a player remains invulnerable after respawning.

### `public static final long MAX_TELEPORT_INVULNERABILITY_MILLIS`
The maximum duration (in milliseconds) a player can remain invulnerable after a teleportation.

## Static Methods

### `public static ComponentType<EntityStore, Player> getComponentType()`
Retrieves the `ComponentType` specifically associated with `Player` components. This is crucial for accessing or manipulating `Player` instances within an `EntityStore`.
- **Returns:** The `ComponentType` for `Player`.

### `public static CompletableFuture<Transform> getRespawnPosition(@Nonnull Ref<EntityStore> ref, @Nonnull String worldName, @Nonnull ComponentAccessor<EntityStore> componentAccessor)`
Asynchronously calculates and returns the optimal respawn position for a player within a given world, taking into account any configured respawn points and potential obstructions.
- **Parameters:**
    - `ref`: A `Ref` to the player's entity in the `EntityStore`.
    - `worldName`: The name of the world the player is respawning in.
    - `componentAccessor`: An accessor for entity components.
- **Returns:** A `CompletableFuture<Transform>` that resolves to the calculated respawn `Transform`.

### `public static void setGameMode(@Nonnull Ref<EntityStore> playerRef, @Nonnull GameMode gameMode, @Nonnull ComponentAccessor<EntityStore> componentAccessor)`
Sets the game mode for a specific player. This method handles updating internal player state, sending the appropriate packets to the client, and managing associated permissions.
- **Parameters:**
    - `playerRef`: A `Ref` to the player's entity.
    - `gameMode`: The `GameMode` to set (e.g., `Creative`, `Survival`).
    - `componentAccessor`: An accessor for entity components.

### `public static void initGameMode(@Nonnull Ref<EntityStore> playerRef, @Nonnull ComponentAccessor<EntityStore> componentAccessor)`
Initializes the game mode for a player, typically called when a player first joins or their entity is created. If no game mode is set, it defaults to the world's configured game mode.
- **Parameters:**
    - `playerRef`: A `Ref` to the player's entity.
    - `componentAccessor`: An accessor for entity components.

## Methods

### `public void copyFrom(@Nonnull Player oldPlayerComponent)`
Copies relevant state and data from another `Player` component instance. This is often used during player respawns or transfers to maintain continuity.
- **Parameters:**
    - `oldPlayerComponent`: The `Player` component to copy data from.

### `public void init(@Nonnull UUID uuid, @Nonnull PlayerRef playerRef)`
Initializes the `Player` component with the player's `UUID` and `PlayerRef`. This method sets up various player-specific managers like `WindowManager` and `PageManager`.
- **Parameters:**
    - `uuid`: The `UUID` of the player.
    - `playerRef`: The `PlayerRef` instance associated with this player.

### `public Inventory setInventory(Inventory inventory)`
Sets the player's current `Inventory`. This will replace the player's existing inventory with the provided one.
- **Parameters:**
    - `inventory`: The new `Inventory` object for the player.
- **Returns:** The `Inventory` that was set.

### `public PlayerConfigData getPlayerConfigData()`
Retrieves the `PlayerConfigData` object for this player, which contains persistent settings and data specific to the player.
- **Returns:** The `PlayerConfigData` instance.

### `public void markNeedsSave()`
Marks the player's configuration data as dirty, signaling that it needs to be saved to persistent storage.

### `public WorldMapTracker getWorldMapTracker()`
Retrieves the `WorldMapTracker` for this player, which manages the player's discovered world map data.
- **Returns:** The `WorldMapTracker` instance.

### `public WindowManager getWindowManager()`
Retrieves the `WindowManager` for this player, which handles the display and management of client-side UI windows.
- **Returns:** The `WindowManager` instance.

### `public PageManager getPageManager()`
Retrieves the `PageManager` for this player, which manages different UI pages or screens presented to the player.
- **Returns:** The `PageManager` instance.

### `public HudManager getHudManager()`
Retrieves the `HudManager` for this player, which controls various elements displayed on the player's Heads-Up Display.
- **Returns:** The `HudManager` instance.

### `public HotbarManager getHotbarManager()`
Retrieves the `HotbarManager` for this player, which manages the player's hotbar inventory.
- **Returns:** The `HotbarManager` instance.

### `public boolean isFirstSpawn()`
Checks if this is the player's first time spawning in the current session or on the server.
- **Returns:** `true` if it's the first spawn, `false` otherwise.

### `public void setFirstSpawn(boolean firstSpawn)`
Sets whether this is considered the player's first spawn.
- **Parameters:**
    - `firstSpawn`: `true` if it's the first spawn, `false` otherwise.

### `public void resetManagers(@Nonnull Holder<EntityStore> holder)`
Resets various player-specific managers (e.g., `WorldMapTracker`, `HudManager`, `CameraManager`) to their default states. This is often called during player reinitialization.
- **Parameters:**
    - `holder`: The player's `Holder` in the `EntityStore`.

### `public boolean isOverrideBlockPlacementRestrictions()`
Checks if the player currently has block placement restrictions overridden.
- **Returns:** `true` if restrictions are overridden, `false` otherwise.

### `public void setOverrideBlockPlacementRestrictions(@Nonnull Ref<EntityStore> ref, boolean overrideBlockPlacementRestrictions, @Nonnull ComponentAccessor<EntityStore> componentAccessor)`
Sets whether the player's block placement restrictions are overridden. This can be used to grant creative-like building capabilities.
- **Parameters:**
    - `ref`: A `Ref` to the player's entity.
    - `overrideBlockPlacementRestrictions`: `true` to override, `false` to enforce restrictions.
    - `componentAccessor`: An accessor for entity components.

### `public boolean hasPermission(@Nonnull String id)`
Checks if the player has a specific permission identified by `id`.
- **Parameters:**
    - `id`: The permission string (e.g., "myplugin.command.use").
- **Returns:** `true` if the player has the permission, `false` otherwise.

### `public boolean hasPermission(@Nonnull String id, boolean def)`
Checks if the player has a specific permission, providing a default value if the permission is not explicitly set.
- **Parameters:**
    - `id`: The permission string.
    - `def`: The default boolean value to return if the permission is not found.
- **Returns:** `true` if the player has the permission or the default is `true`, `false` otherwise.

### `public boolean hasSpawnProtection()`
Checks if the player currently has spawn protection (invulnerability after spawning/teleporting).
- **Returns:** `true` if the player is spawn protected, `false` otherwise.

### `public boolean isWaitingForClientReady()`
Checks if the server is currently waiting for the client to signal that it's ready after a spawn or teleport.
- **Returns:** `true` if waiting for client readiness, `false` otherwise.

### `public void setClientViewRadius(int clientViewRadius)`
Sets the client's requested view radius in chunks. The actual view radius might be capped by server settings.
- **Parameters:**
    - `clientViewRadius`: The desired view radius in chunks.

### `public int getClientViewRadius()`
Retrieves the client's requested view radius in chunks.
- **Returns:** The client's requested view radius.

### `public int getViewRadius()`
Retrieves the effective view radius for the player, which is the minimum of the client's requested radius and the server's maximum allowed view radius.
- **Returns:** The effective view radius in chunks.

### `public boolean canDecreaseItemStackDurability(@Nonnull Ref<EntityStore> ref, @Nonnull ComponentAccessor<EntityStore> componentAccessor)`
Determines if an item stack's durability can decrease for this player, typically based on their game mode (e.g., durability does not decrease in Creative mode).
- **Parameters:**
    - `ref`: A `Ref` to the player's entity.
    - `componentAccessor`: An accessor for entity components.
- **Returns:** `true` if durability can decrease, `false` otherwise.

### `public boolean canApplyItemStackPenalties(@Nonnull Ref<EntityStore> ref, @Nonnull ComponentAccessor<EntityStore> componentAccessor)`
Determines if item stack penalties (e.g., breaking an item) can be applied to this player.
- **Parameters:**
    - `ref`: A `Ref` to the player's entity.
    - `componentAccessor`: An accessor for entity components.
- **Returns:** `true` if penalties can apply, `false` otherwise.

### `public ItemStackSlotTransaction updateItemStackDurability(@Nonnull Ref<EntityStore> ref, @Nonnull ItemStack itemStack, ItemContainer container, int slotId, double durabilityChange, @Nonnull ComponentAccessor<EntityStore> componentAccessor)`
Updates the durability of an `ItemStack` in the player's inventory, applying necessary logic for item breakage and sending notifications.
- **Parameters:**
    - `ref`: A `Ref` to the player's entity.
    - `itemStack`: The `ItemStack` whose durability is being updated.
    - `container`: The `ItemContainer` holding the item.
    - `slotId`: The slot ID of the item within the container.
    - `durabilityChange`: The amount of durability change (positive for increase, negative for decrease).
    - `componentAccessor`: An accessor for entity components.
- **Returns:** An `ItemStackSlotTransaction` if the durability was updated, `null` otherwise.

### `public GameMode getGameMode()`
Retrieves the player's current `GameMode`.
- **Returns:** The player's `GameMode`.

### `public String getDisplayName()`
Returns the player's display name, which is typically their username.
- **Returns:** The player's display name as a `String`.

## Deprecated Methods

### `public PacketHandler getPlayerConnection()`
*Deprecated.* Use `PlayerRef.getPacketHandler()` instead to get the player's packet handler.

### `public PlayerRef getPlayerRef()`
*Deprecated.* Retrieves the associated `PlayerRef` instance. This method is deprecated as the `Player` component usually works with its own `Ref<EntityStore>` and `ComponentAccessor`.
