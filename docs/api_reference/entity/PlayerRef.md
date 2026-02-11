# PlayerRef

The `PlayerRef` class represents a lightweight, persistent reference to a connected player within the Hytale `Universe`. It acts as a bridge between the player's network connection and their in-game entity data, which is stored in an `EntityStore` as components. Mod developers will use `PlayerRef` to interact with players, send them messages, manage their connection, and access their associated entity components.

## Static Methods

### `public static ComponentType<EntityStore, PlayerRef> getComponentType()`
Retrieves the `ComponentType` specifically associated with `PlayerRef` components. This is essential for accessing or manipulating `PlayerRef` instances within an `EntityStore`.
- **Returns:** The `ComponentType` for `PlayerRef`.

## Constructor

### `public PlayerRef(@Nonnull Holder<EntityStore> holder, @Nonnull UUID uuid, @Nonnull String username, @Nonnull String language, @Nonnull PacketHandler packetHandler, @Nonnull ChunkTracker chunkTracker)`
Initializes a new `PlayerRef` instance. This constructor is primarily used internally by the server during player connection and loading.
- **Parameters:**
    - `holder`: The `Holder` containing the player's initial entity components.
    - `uuid`: The unique identifier (`UUID`) of the player.
    - `username`: The player's username.
    - `language`: The player's preferred language.
    - `packetHandler`: The `PacketHandler` responsible for network communication with this player.
    - `chunkTracker`: The `ChunkTracker` instance managing the chunks the player is viewing.

## Methods

### `public boolean isValid()`
Checks if this `PlayerRef` is currently valid, meaning it is associated with an active player entity either as a `Holder` or a `Ref<EntityStore>`.
- **Returns:** `true` if valid, `false` otherwise.

### `public Ref<EntityStore> getReference()`
Retrieves the `Ref<EntityStore>` that points to the player's actual entity data within the `EntityStore` once the player entity is loaded into a world.
- **Returns:** The `Ref<EntityStore>` for this player, or `null` if the player entity is not yet fully in a world.

### `public Holder<EntityStore> getHolder()`
Retrieves the `Holder<EntityStore>` that temporarily holds the player's entity components before they are fully added to an `EntityStore`. This is typically used during the player loading process.
- **Returns:** The `Holder<EntityStore>` for this player, or `null` if the player's entity is already in an `EntityStore`.

### `public UUID getUuid()`
Returns the unique identifier (`UUID`) of the player. This `UUID` is persistent across sessions.
- **Returns:** The player's `UUID`.

### `public String getUsername()`
Returns the current username of the player.
- **Returns:** The player's username as a `String`.

### `public PacketHandler getPacketHandler()`
Retrieves the `PacketHandler` associated with this player's network connection. Mod developers can use this to send custom packets directly to the player's client.
- **Returns:** The `PacketHandler` instance for this player.

### `public ChunkTracker getChunkTracker()`
Retrieves the `ChunkTracker` instance for this player. The `ChunkTracker` manages which world chunks the player is currently viewing or has loaded.
- **Returns:** The `ChunkTracker` for this player.

### `public HiddenPlayersManager getHiddenPlayersManager()`
Retrieves the `HiddenPlayersManager` for this player. This manager controls which other players are hidden from this specific player's view.
- **Returns:** The `HiddenPlayersManager` instance.

### `public String getLanguage()`
Returns the player's currently set language preference (e.g., "en_US", "fr_FR").
- **Returns:** The language code as a `String`.

### `public void setLanguage(@Nonnull String language)`
Sets the player's language preference.
- **Parameters:**
    - `language`: The new language code (e.g., "en_US").

### `public Transform getTransform()`
Returns a `Transform` object representing the player's current position, rotation, and scale in the game world. This object is mutable.
- **Returns:** The player's `Transform`.

### `public UUID getWorldUuid()`
Retrieves the `UUID` of the `World` the player is currently located in.
- **Returns:** The `UUID` of the player's world, or `null` if the player is not yet fully in a world.

### `public Vector3f getHeadRotation()`
Returns a `Vector3f` representing the player's head rotation (pitch, yaw, roll).
- **Returns:** The player's `Vector3f` head rotation.

### `public void updatePosition(@Nonnull World world, @Nonnull Transform transform, @Nonnull Vector3f headRotation)`
Updates the player's current `World` association, position, and head rotation. This method is typically called by the server's movement system.
- **Parameters:**
    - `world`: The `World` the player is now in.
    - `transform`: The new `Transform` (position and rotation).
    - `headRotation`: The new `Vector3f` head rotation.

### `public void referToServer(@Nonnull String host, int port)`
Instructs the player's client to connect to another server at the specified host and port.
- **Parameters:**
    - `host`: The hostname or IP address of the target server.
    - `port`: The port of the target server.

### `public void referToServer(@Nonnull String host, int port, @Nullable byte[] data)`
Instructs the player's client to connect to another server, optionally sending additional byte data. This can be used for passing session tokens or other relevant information to the new server.
- **Parameters:**
    - `host`: The hostname or IP address of the target server.
    - `port`: The port of the target server.
    - `data`: An optional byte array containing additional referral data (maximum 4096 bytes).

### `public void sendMessage(@Nonnull Message message)`
Sends a formatted `Message` object to this specific player. The message will be displayed in the player's chat.
- **Parameters:**
    - `message`: The `Message` object to send.

## Deprecated Methods

### `public <T extends Component<EntityStore>> T getComponent(@Nonnull ComponentType<EntityStore, T> componentType)`
*Deprecated.* Retrieves a component from the player's entity holder or store. It's generally recommended to access components directly via the player's `Entity` instance (if available) or by using the `EntityStore` API.

### `public void replaceHolder(@Nonnull Holder<EntityStore> holder)`
*Deprecated.* Replaces the internal `Holder` for this `PlayerRef`. This is an internal method likely used during complex player state transitions.

### `public Component<EntityStore> clone()`
*Note: This method returns `this`.* `PlayerRef` instances are essentially references and are not designed to be cloned in the traditional sense. This implementation suggests that a `PlayerRef` is unique per player and its cloning operation simply returns the same instance.
