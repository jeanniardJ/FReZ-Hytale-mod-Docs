# JavaPlugin

The `JavaPlugin` is an abstract base class that concrete Hytale plugins (mods) written in Java should extend. It provides fundamental functionalities and hooks into the server's plugin management system. Developers building new Hytale mods will typically create a class that extends `JavaPlugin` to define their mod's behavior.

## Constructor

### `public JavaPlugin(@Nonnull JavaPluginInit init)`
Initializes a new instance of a `JavaPlugin`. This constructor is typically called internally by the plugin loading system.
- **Parameters:**
    - `init`: An initialization object containing details about the plugin, such as its file path and class loader.

## Methods

### `public Path getFile()`
Retrieves the file system path to the JAR file from which this plugin was loaded.
- **Returns:** A `Path` object representing the plugin's JAR file.

### `public PluginClassLoader getClassLoader()`
Retrieves the `PluginClassLoader` instance specifically associated with this plugin. This class loader is responsible for loading classes and resources for this particular plugin.
- **Returns:** The `PluginClassLoader` for this plugin.

### `public final PluginType getType()`
Returns the type of this plugin. For `JavaPlugin` instances, this will always return `PluginType.PLUGIN`.
- **Returns:** The `PluginType` of the plugin.
