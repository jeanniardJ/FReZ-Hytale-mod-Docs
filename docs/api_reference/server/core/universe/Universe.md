# Universe

The `Universe` class is a central and singleton component within the Hytale server, acting as the primary manager for all worlds, players, and global game state. It extends `JavaPlugin`, indicating its role as a core plugin that provides fundamental server functionalities. Mod developers will frequently interact with the `Universe` to access world instances, player data, and global events.

## Constants

### `public static final PluginManifest MANIFEST`
The `PluginManifest` for the core Universe plugin, containing its metadata and dependencies.

### `public static final MetricsRegistry<Universe> METRICS_REGISTRY`
A `MetricsRegistry` specifically for the `Universe` class, used for tracking and reporting various server-wide metrics.

### `public static Map<Integer, String> LEGACY_BLOCK_ID_MAP`
*Deprecated.* A map providing a lookup for legacy block IDs to their string representations. This is likely for backward compatibility with older world formats.

## Static Methods

### `public static Universe get()`
Retrieves the singleton instance of the `Universe`. This is the primary entry point for accessing all universe-level functionalities.
- **Returns:** The singleton `Universe` instance.

### `public static Path getWorldGenPath()`
Returns the `Path` to the directory where world generation configurations and assets are stored.
- **Returns:** A `Path` object pointing to the world generation directory.

## Methods

### `public CompletableFuture<Void> runBackup()`
Initiates an asynchronous backup operation for the entire universe. This includes saving all currently loaded worlds.
- **Returns:** A `CompletableFuture<Void>` that completes when the backup process is finished.

### `public void disconnectAllPLayers()`
Forces all currently connected players to disconnect from the server. This is typically used during server shutdown or maintenance.

### `public void shutdownAllWorlds()`
Initiates the shutdown process for all currently loaded worlds, ensuring their data is saved and resources are released. This is also typically used during server shutdown.

### `public CompletableFuture<Void> getUniverseReady()`
Returns a `CompletableFuture` that completes once the `Universe` has fully loaded all its components, worlds, and is ready to accept player connections. Mod developers can use this to ensure the server is fully operational before executing certain logic.
- **Returns:** A `CompletableFuture<Void>`.

### `public boolean isWorldLoadable(@Nonnull String name)`
Checks if a world with the given `name` exists on disk within the universe's directory structure and is in a state where it can be loaded.
- **Parameters:**
    - `name`: The name of the world to check.
- **Returns:** `true` if the world is loadable, `false` otherwise.

### `public CompletableFuture<World> addWorld(@Nonnull String name)`
Asynchronously adds a new world to the universe with the specified `name`, using default world generation and storage configurations.
- **Parameters:**
    - `name`: The name of the new world.
- **Returns:** A `CompletableFuture<World>` that completes with the newly created `World` instance.
- **Throws:** `IllegalArgumentException` if a world with the given name already exists or is loadable from disk.

### `public CompletableFuture<World> addWorld(@Nonnull String name, @Nullable String generatorType, @Nullable String chunkStorageType)`
*Deprecated.* Asynchronously adds a new world to the universe with the specified `name`, allowing for custom `generatorType` and `chunkStorageType`. This method is deprecated, suggesting that world creation with custom providers might be handled differently in newer versions.
- **Parameters:**
    - `name`: The name of the new world.
    - `generatorType`: An optional string specifying the type of world generator to use (e.g., "Hytale", "Flat").
    - `chunkStorageType`: An optional string specifying the type of chunk storage provider to use.
- **Returns:** A `CompletableFuture<World>`.

### `public CompletableFuture<World> makeWorld(@Nonnull String name, @Nonnull Path savePath, @Nonnull WorldConfig worldConfig)`
Asynchronously creates a `World` instance with a given `name`, `savePath`, and `WorldConfig`. This is a more direct way to instantiate a world.
- **Parameters:**
    - `name`: The name of the world.
    - `savePath`: The `Path` where the world data will be saved.
    - `worldConfig`: The `WorldConfig` defining the world's properties.
- **Returns:** A `CompletableFuture<World>`.

### `public CompletableFuture<World> loadWorld(@Nonnull String name)`
Asynchronously loads an existing world from disk with the specified `name`.
- **Parameters:**
    - `name`: The name of the world to load.
- **Returns:** A `CompletableFuture<World>` that completes with the loaded `World` instance.
- **Throws:** `IllegalArgumentException` if the world is already loaded or does not exist on disk.

### `public World getWorld(@Nullable String worldName)`
Retrieves a loaded `World` instance by its name. The name is case-insensitive.
- **Parameters:**
    - `worldName`: The name of the world to retrieve.
- **Returns:** The `World` instance if found, `null` otherwise.

### `public World getWorld(@Nonnull UUID uuid)`
Retrieves a loaded `World` instance by its unique `UUID`.
- **Parameters:**
    - `uuid`: The `UUID` of the world to retrieve.
- **Returns:** The `World` instance if found, `null` otherwise.

### `public World getDefaultWorld()`
Retrieves the `World` instance configured as the default world for the server.
- **Returns:** The default `World` instance, or `null` if no default world is configured or loaded.

### `public boolean removeWorld(@Nonnull String name)`
Removes a world with the specified `name` from the universe. This will stop the world and save its data, if necessary, then unload it.
- **Parameters:**
    - `name`: The name of the world to remove.
- **Returns:** `true` if the world was successfully removed, `false` if the removal was cancelled by an event listener.
- **Throws:** `NullPointerException` if the world does not exist.

### `public Path getPath()`
Retrieves the root `Path` of the universe directory where all world data and other universe-level files are stored.
- **Returns:** A `Path` object.

### `public Map<String, World> getWorlds()`
Returns an unmodifiable `Map` of all currently loaded `World` instances in the universe, mapped by their names (case-insensitive).
- **Returns:** An unmodifiable `Map<String, World>`.

### `public List<PlayerRef> getPlayers()`
Returns a list of all `PlayerRef` instances representing currently connected players.
- **Returns:** A `List` of `PlayerRef` objects.

### `public PlayerRef getPlayer(@Nonnull UUID uuid)`
Retrieves a `PlayerRef` for a connected player using their `UUID`.
- **Parameters:**
    - `uuid`: The `UUID` of the player to retrieve.
- **Returns:** The `PlayerRef` instance if the player is connected, `null` otherwise.

### `public PlayerRef getPlayer(@Nonnull String value, @Nonnull NameMatching matching)`
Retrieves a `PlayerRef` for a connected player by matching their username against the provided `value` using a specified `NameMatching` strategy.
- **Parameters:**
    - `value`: The string to match against player usernames.
    - `matching`: The `NameMatching` strategy to use (e.g., `EXACT`, `STARTS_WITH`, `CONTAINS`).
- **Returns:** The `PlayerRef` instance if a match is found, `null` otherwise.

### `public int getPlayerCount()`
Returns the total number of players currently connected to the server.
- **Returns:** An `int` representing the player count.

### `public void removePlayer(@Nonnull PlayerRef playerRef)`
Removes a connected player from the universe. This handles their disconnection and cleanup.
- **Parameters:**
    - `playerRef`: The `PlayerRef` of the player to remove.

### `public void sendMessage(@Nonnull Message message)`
Broadcasts a `Message` to all currently connected players in the universe.
- **Parameters:**
    - `message`: The `Message` object to send.

### `public void broadcastPacket(@Nonnull Packet packet)`
Broadcasts a single `Packet` to all currently connected players.
- **Parameters:**
    - `packet`: The `Packet` to send.

### `public void broadcastPacket(@Nonnull Packet... packets)`
Broadcasts multiple `Packet`s to all currently connected players.
- **Parameters:**
    - `packets`: An array of `Packet` objects to send.

### `public PlayerStorage getPlayerStorage()`
Retrieves the `PlayerStorage` instance responsible for loading and saving player data.
- **Returns:** The `PlayerStorage` instance.

### `public WorldConfigProvider getWorldConfigProvider()`
Retrieves the `WorldConfigProvider` instance used for loading and managing world configurations.
- **Returns:** The `WorldConfigProvider` instance.
