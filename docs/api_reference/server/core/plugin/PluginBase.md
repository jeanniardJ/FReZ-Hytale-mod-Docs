# PluginBase

The `PluginBase` abstract class serves as the foundation for all plugins within the Hytale server environment. It provides core functionalities, lifecycle management, and access to various server registries that mod developers will use to extend and customize the game. Any plugin, including `JavaPlugin`, extends `PluginBase`.

## Constructor

### `public PluginBase(@Nonnull PluginInit init)`
Initializes a new `PluginBase` instance. This constructor is typically called by the plugin loading system with initialization parameters.
- **Parameters:**
    - `init`: An initialization object containing essential plugin information like its manifest, data directory, and whether it's part of the server classpath.

## Methods

### `public String getName()`
Retrieves the human-readable name of the plugin as defined in its manifest.
- **Returns:** The plugin's name as a `String`.

### `public HytaleLogger getLogger()`
Provides a dedicated `HytaleLogger` instance for this plugin, allowing for logging messages that are prefixed with the plugin's name.
- **Returns:** The plugin's `HytaleLogger` instance.

### `public PluginIdentifier getIdentifier()`
Retrieves the unique identifier of the plugin. This is typically a combination of the plugin's group and name.
- **Returns:** The `PluginIdentifier` for this plugin.

### `public PluginManifest getManifest()`
Returns the `PluginManifest` object for this plugin, which contains metadata such as the plugin's version, dependencies, and author information.
- **Returns:** The `PluginManifest` of the plugin.

### `public Path getDataDirectory()`
Retrieves the file system path to the plugin's persistent data directory. Plugins can use this directory to store configuration files, saved data, and other resources.
- **Returns:** A `Path` object representing the plugin's data directory.

### `public PluginState getState()`
Returns the current lifecycle state of the plugin (e.g., `NONE`, `SETUP`, `START`, `ENABLED`, `DISABLED`, `SHUTDOWN`).
- **Returns:** A `PluginState` enum.

### `public ClientFeatureRegistry getClientFeatureRegistry()`
Provides access to the `ClientFeatureRegistry`, allowing plugins to register custom client-side features, such as UI elements or rendering logic.
- **Returns:** The `ClientFeatureRegistry` instance.

### `public CommandRegistry getCommandRegistry()`
Provides access to the `CommandRegistry`, enabling plugins to register new server commands that can be executed by players or the console.
- **Returns:** The `CommandRegistry` instance.

### `public EventRegistry getEventRegistry()`
Provides access to the `EventRegistry`, which allows plugins to register listeners for various server events.
- **Returns:** The `EventRegistry` instance.

### `public BlockStateRegistry getBlockStateRegistry()`
Provides access to the `BlockStateRegistry`, allowing plugins to register custom block states.
- **Returns:** The `BlockStateRegistry` instance.

### `public EntityRegistry getEntityRegistry()`
Provides access to the `EntityRegistry`, allowing plugins to register custom entity types.
- **Returns:** The `EntityRegistry` instance.

### `public TaskRegistry getTaskRegistry()`
Provides access to the `TaskRegistry`, enabling plugins to schedule custom tasks to run at specific intervals or delays.
- **Returns:** The `TaskRegistry` instance.

### `public ComponentRegistryProxy<EntityStore> getEntityStoreRegistry()`
Provides a proxy to the `EntityStore`'s component registry, allowing plugins to register custom components that can be attached to entities and persisted.
- **Returns:** A `ComponentRegistryProxy` for `EntityStore`.

### `public ComponentRegistryProxy<ChunkStore> getChunkStoreRegistry()`
Provides a proxy to the `ChunkStore`'s component registry, allowing plugins to register custom components that can be attached to chunks and persisted.
- **Returns:** A `ComponentRegistryProxy` for `ChunkStore`.

### `public AssetRegistry getAssetRegistry()`
Provides access to the `AssetRegistry`, which allows plugins to interact with and manage game assets.
- **Returns:** The `AssetRegistry` instance.

### `public <T, C extends Codec<? extends T>> CodecMapRegistry<T, C> getCodecRegistry(@Nonnull StringCodecMapCodec<T, C> mapCodec)`
Retrieves a `CodecMapRegistry` for a given `StringCodecMapCodec`. This is a generic method for interacting with various codec-based registries.
- **Parameters:**
    - `mapCodec`: The `StringCodecMapCodec` to use for retrieving the registry.
- **Returns:** A `CodecMapRegistry` instance.

### `public <K, T extends com.hypixel.hytale.assetstore.JsonAsset<K>> CodecMapRegistry.Assets<T, ?> getCodecRegistry(@Nonnull AssetCodecMapCodec<K, T> mapCodec)`
Retrieves an asset-specific `CodecMapRegistry` for a given `AssetCodecMapCodec`. This is used for managing registries of JSON-based assets.
- **Parameters:**
    - `mapCodec`: The `AssetCodecMapCodec` to use for retrieving the asset registry.
- **Returns:** A `CodecMapRegistry.Assets` instance.

### `public <V> MapKeyMapRegistry<V> getCodecRegistry(@Nonnull MapKeyMapCodec<V> mapCodec)`
Retrieves a `MapKeyMapRegistry` for a given `MapKeyMapCodec`. This is used for managing registries where elements are identified by map keys.
- **Parameters:**
    - `mapCodec`: The `MapKeyMapCodec` to use for retrieving the registry.
- **Returns:** A `MapKeyMapRegistry` instance.

### `public final String getBasePermission()`
Returns the base permission string associated with this plugin, typically derived from its group and name. This string can be used as a prefix for defining specific permissions within the plugin.
- **Returns:** The base permission `String`.

### `public boolean isDisabled()`
Checks if the plugin is currently in a disabled state (`NONE`, `DISABLED`, or `SHUTDOWN`).
- **Returns:** `true` if the plugin is disabled, `false` otherwise.

### `public boolean isEnabled()`
Checks if the plugin is currently in an enabled state.
- **Returns:** `true` if the plugin is enabled, `false` otherwise.

### `public abstract PluginType getType()`
An abstract method that must be implemented by subclasses to return the specific type of the plugin (e.g., `PluginType.PLUGIN`, `PluginType.ASSET_PACK`).
- **Returns:** The `PluginType` of the plugin.

## Protected Methods (Lifecycle Hooks)

These methods are intended to be overridden by subclasses (`JavaPlugin` or other plugin implementations) to inject custom logic at different stages of the plugin's lifecycle.

### `protected final <T> Config<T> withConfig(@Nonnull BuilderCodec<T> configCodec)`
Registers a configuration file for the plugin using a default filename. This method must be called before the plugin's setup phase.
- **Parameters:**
    - `configCodec`: A `BuilderCodec` that defines how the configuration object is encoded and decoded.
- **Returns:** The `Config` instance managing the configuration file.

### `protected final <T> Config<T> withConfig(@Nonnull String name, @Nonnull BuilderCodec<T> configCodec)`
Registers a configuration file for the plugin with a custom filename. This method must be called before the plugin's setup phase.
- **Parameters:**
    - `name`: The filename for the configuration file (without extension).
    - `configCodec`: A `BuilderCodec` that defines how the configuration object is encoded and decoded.
- **Returns:** The `Config` instance managing the configuration file.

### `protected void setup()`
This method is called during the plugin's setup phase, after the plugin has been loaded but before it has started. It's a good place for initializations that don't depend on other plugins being fully enabled.

### `protected void start()`
This method is called when the plugin is starting, after all dependencies have been set up. This is typically where plugins register their event listeners, commands, and other functionalities that need to be active for the plugin to function.

### `protected void shutdown()`
This method is called when the plugin is being shut down. Plugins should use this hook to perform cleanup operations, save data, unregister listeners, and release resources.
