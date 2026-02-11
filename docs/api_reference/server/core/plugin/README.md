# Server Core Plugin API Reference

This section provides detailed API documentation for the core plugin management components of the Hytale server. These classes are fundamental for developing and managing plugins (mods) that extend the server's functionality.

## Classes

-   [`PluginManager`](./PluginManager.md): Manages the lifecycle of all plugins on the server.
-   [`PluginBase`](./PluginBase.md): The abstract base class for all Hytale plugins, providing common functionalities and access to various server registries.
-   [`JavaPlugin`](./JavaPlugin.md): A concrete abstract class extending `PluginBase`, specifically designed for plugins written in Java.
