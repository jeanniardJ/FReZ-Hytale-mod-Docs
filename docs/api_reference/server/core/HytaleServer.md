# HytaleServer

The `HytaleServer` class is the main entry point and core orchestrator for the Hytale server application. It manages the server's lifecycle, including booting, plugin management, command handling, and shutdown.

## Constructor

### `public HytaleServer() throws IOException`
Initializes a new instance of the Hytale server. This constructor sets up the server's core components, loads configuration, initializes authentication, and registers shutdown hooks.

## Methods

### `public EventBus getEventBus()`
Retrieves the server's `EventBus` instance, which is used for dispatching and subscribing to server events.
- **Returns:** The `EventBus` instance.

### `public PluginManager getPluginManager()`
Retrieves the server's `PluginManager` instance, responsible for loading, managing, and interacting with plugins.
- **Returns:** The `PluginManager` instance.

### `public CommandManager getCommandManager()`
Retrieves the server's `CommandManager` instance, used for registering and handling server commands.
- **Returns:** The `CommandManager` instance.

### `public HytaleServerConfig getConfig()`
Retrieves the current server configuration.
- **Returns:** The `HytaleServerConfig` instance.

### `public void shutdownServer()`
Initiates a graceful shutdown of the Hytale server with a default shutdown reason.

### `public void shutdownServer(@Nonnull ShutdownReason reason)`
Initiates a graceful shutdown of the Hytale server with a specified reason.
- **Parameters:**
    - `reason`: The `ShutdownReason` enum indicating why the server is shutting down.

### `public void doneSetup(PluginBase plugin)`
Called by the `PluginManager` when a plugin has completed its setup phase.
- **Parameters:**
    - `plugin`: The `PluginBase` instance that completed setup.

### `public void doneStart(PluginBase plugin)`
Called by the `PluginManager` when a plugin has completed its start phase.
- **Parameters:**
    - `plugin`: The `PluginBase` instance that completed startup.

### `public void doneStop(PluginBase plugin)`
Called by the `PluginManager` when a plugin has completed its stop phase.
- **Parameters:**
    - `plugin`: The `PluginBase` instance that completed stopping.

### `public void sendSingleplayerProgress()`
Sends progress updates related to single-player server loading/shutdown.

### `public String getServerName()`
Retrieves the configured name of the server.
- **Returns:** The server's name as a `String`.

### `public boolean isBooting()`
Checks if the server is currently in the booting process.
- **Returns:** `true` if the server is booting, `false` otherwise.

### `public boolean isBooted()`
Checks if the server has successfully completed its booting process.
- **Returns:** `true` if the server has booted, `false` otherwise.

### `public boolean isShuttingDown()`
Checks if the server is currently in the process of shutting down.
- **Returns:** `true` if the server is shutting down, `false` otherwise.

### `public Instant getBoot()`
Retrieves the `Instant` when the server's boot process began.
- **Returns:** An `Instant` representing the boot time.

### `public long getBootStart()`
Retrieves the system's nano time when the server's boot process began.
- **Returns:** A `long` representing the boot start time in nanoseconds.

### `public ShutdownReason getShutdownReason()`
Retrieves the reason for the server's shutdown, if any.
- **Returns:** The `ShutdownReason` enum, or `null` if the server is not shutting down.

### `public void reportSingleplayerStatus(String message)`
Reports a status message specifically for single-player mode.
- **Parameters:**
    - `message`: The status message to report.

### `public void reportSaveProgress(@Nonnull World world, int saved, int total)`
Reports the progress of world saving operations, typically during shutdown.
- **Parameters:**
    - `world`: The `World` instance being saved.
    - `saved`: The number of chunks saved so far.
    - `total`: The total number of chunks to save.

### `public static HytaleServer get()`
Retrieves the singleton instance of the `HytaleServer`.
- **Returns:** The `HytaleServer` instance.