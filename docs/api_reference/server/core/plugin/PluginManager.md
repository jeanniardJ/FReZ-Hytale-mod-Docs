# PluginManager

The `PluginManager` class is responsible for managing the lifecycle of plugins on the Hytale server. It handles loading, starting, stopping, and reloading plugins, as well as managing their dependencies and classloaders. Mod developers will primarily interact with this manager to query information about other plugins or to register their own plugins.

## Static Methods

### `public static PluginManager get()`
Retrieves the singleton instance of the `PluginManager`. This is the primary way to access plugin management functionality.
- **Returns:** The singleton `PluginManager` instance.

## Methods

### `public void registerCorePlugin(@Nonnull PluginManifest builder)`
Registers a core plugin with the server. Core plugins are typically internal server components packaged as plugins.
- **Parameters:**
    - `builder`: The `PluginManifest` of the core plugin to register.

### `public void setup()`
Initializes the plugin manager and prepares it for loading plugins. This method is called internally during server boot.

### `public void start()`
Starts all loaded plugins. This method is called internally after the server setup is complete.

### `public void shutdown()`
Initiates the shutdown process for all active plugins, allowing them to perform cleanup operations. This method is called internally during server shutdown.

### `public PluginState getState()`
Retrieves the current lifecycle state of the plugin manager.
- **Returns:** A `PluginState` enum indicating the current state (e.g., NONE, SETUP, START, SHUTDOWN).

### `public PluginBridgeClassLoader getBridgeClassLoader()`
Retrieves the `PluginBridgeClassLoader` used for inter-plugin class loading. This is an advanced feature primarily for managing class visibility and isolation between plugins.
- **Returns:** The `PluginBridgeClassLoader` instance.

### `public List<PluginBase> getPlugins()`
Returns an immutable list of all currently loaded `PluginBase` instances.
- **Returns:** A `List` of `PluginBase` objects.

### `public PluginBase getPlugin(PluginIdentifier identifier)`
Retrieves a specific plugin by its `PluginIdentifier`.
- **Parameters:**
    - `identifier`: The unique `PluginIdentifier` of the plugin to retrieve.
- **Returns:** The `PluginBase` instance if found, otherwise `null`.

### `public boolean hasPlugin(PluginIdentifier identifier, @Nonnull SemverRange range)`
Checks if a plugin with the given `PluginIdentifier` is loaded and its version satisfies the specified `SemverRange`.
- **Parameters:**
    - `identifier`: The unique `PluginIdentifier` of the plugin.
    - `range`: The `SemverRange` to check against the plugin's version.
- **Returns:** `true` if the plugin is loaded and its version matches the range, `false` otherwise.

### `public boolean reload(@Nonnull PluginIdentifier identifier)`
Attempts to unload and then reload a plugin identified by its `PluginIdentifier`.
- **Parameters:**
    - `identifier`: The unique `PluginIdentifier` of the plugin to reload.
- **Returns:** `true` if the plugin was successfully reloaded, `false` otherwise.

### `public boolean unload(@Nonnull PluginIdentifier identifier)`
Attempts to unload a plugin identified by its `PluginIdentifier`. This will stop and disable the plugin.
- **Parameters:**
    - `identifier`: The unique `PluginIdentifier` of the plugin to unload.
- **Returns:** `true` if the plugin was successfully unloaded, `false` otherwise.

### `public boolean load(@Nonnull PluginIdentifier identifier)`
Attempts to load and enable a plugin identified by its `PluginIdentifier`.
- **Parameters:**
    - `identifier`: The unique `PluginIdentifier` of the plugin to load.
- **Returns:** `true` if the plugin was successfully loaded, `false` otherwise.

### `public Map<PluginIdentifier, PluginManifest> getAvailablePlugins()`
Returns a map of all plugins that are available to be loaded, including both loaded and unloaded plugins.
- **Returns:** A `Map` where keys are `PluginIdentifier`s and values are `PluginManifest`s.

### `public ComponentType<EntityStore, PluginListPageManager.SessionSettings> getSessionSettingsComponentType()`
Retrieves the `ComponentType` for session-specific settings managed by the `PluginListPageManager`. This is likely an internal component for managing UI-related settings for plugin listings.
- **Returns:** The `ComponentType` instance.
