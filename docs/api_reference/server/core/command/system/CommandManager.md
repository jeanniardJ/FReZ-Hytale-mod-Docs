# CommandManager

The `CommandManager` class is the central authority for managing and dispatching commands within the Hytale server. It allows for the registration of new commands, handling command execution from various sources (like players or the console), and managing command-related permissions. Mod developers will interact with this manager to add custom commands to their server.

## Static Methods

### `public static CommandManager get()`
Retrieves the singleton instance of the `CommandManager`. This is the primary way to access command management functionality.
- **Returns:** The singleton `CommandManager` instance.

## Methods

### `public Map<String, AbstractCommand> getCommandRegistration()`
Returns an immutable map of all currently registered commands. The keys are the command names (case-insensitive), and the values are their corresponding `AbstractCommand` instances.
- **Returns:** A `Map` of command names to `AbstractCommand` objects.

### `public Map<String, Set<String>> createVirtualPermissionGroups()`
Generates a map of virtual permission groups based on the permissions defined by registered commands. This is primarily used for internal permission management and introspection.
- **Returns:** A `Map` where keys are permission group names and values are sets of associated permissions.

### `public void registerSystemCommand(@Nonnull AbstractCommand command)`
Registers a command as a system-level command. These commands are typically part of the core server functionality but can also be extended by mods.
- **Parameters:**
    - `command`: The `AbstractCommand` instance to register.

### `public CommandRegistration register(@Nonnull AbstractCommand command)`
Registers a custom command with the server. This is the main method for mod developers to add their own commands. Upon successful registration, a `CommandRegistration` object is returned, which can be used to manage the command's lifecycle (e.g., unregistration).
- **Parameters:**
    - `command`: The `AbstractCommand` instance representing the command to register.
- **Returns:** A `CommandRegistration` object if the command was successfully registered, or `null` if registration failed (e.g., due to an invalid name or conflict).

### `public CompletableFuture<Void> handleCommand(@Nonnull PlayerRef playerRef, @Nonnull String command)`
Dispatches a command string for execution, typically originating from a player. The command is processed asynchronously.
- **Parameters:**
    - `playerRef`: A `PlayerRef` object representing the player who issued the command.
    - `command`: The full command string, including the command name and arguments.
- **Returns:** A `CompletableFuture<Void>` that completes when the command has finished execution or an error occurred.

### `public CompletableFuture<Void> handleCommand(@Nonnull CommandSender commandSender, @Nonnull String commandString)`
Dispatches a command string for execution, originating from any `CommandSender` (e.g., a player, the console, or another plugin). The command is processed asynchronously.
- **Parameters:**
    - `commandSender`: The `CommandSender` who issued the command.
    - `commandString`: The full command string, including the command name and arguments.
- **Returns:** A `CompletableFuture<Void>` that completes when the command has finished execution or an error occurred.

### `public CompletableFuture<Void> handleCommands(@Nonnull CommandSender sender, @Nonnull Deque<String> commands)`
Dispatches a sequence of command strings for execution by a `CommandSender`. Each command in the deque is processed sequentially.
- **Parameters:**
    - `sender`: The `CommandSender` who issued the commands.
    - `commands`: A `Deque` (double-ended queue) of command strings to be executed.
- **Returns:** A `CompletableFuture<Void>` that completes when all commands in the sequence have finished execution.

### `public String getName()`
Returns the name of the owner of these commands. In the context of the core server, this typically returns "HytaleServer".
- **Returns:** The owner's name as a `String`.
