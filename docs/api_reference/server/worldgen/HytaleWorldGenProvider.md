# HytaleWorldGenProvider

The `HytaleWorldGenProvider` class serves as the default implementation for `IWorldGenProvider`, offering a standardized way to configure and retrieve Hytale's world generation logic. Mod developers interested in customizing or understanding how world generation is initiated will find this class relevant.

## Constants

### `public static final String ID = "Hytale"`
The unique identifier for this specific world generation provider.

### `public static final BuilderCodec<HytaleWorldGenProvider> CODEC`
A `BuilderCodec` instance responsible for serializing and deserializing `HytaleWorldGenProvider` configurations. This allows the provider's settings (like generator name and path) to be loaded from or saved to a persistent format.

## Methods

### `public IWorldGen getGenerator() throws WorldGenLoadException`
This is the core method of the provider. It constructs and returns an instance of `IWorldGen`, which is the actual world generation engine configured by this provider. The generator's behavior is determined by the `name` and `path` properties set on this `HytaleWorldGenProvider` instance.
- **Returns:** An `IWorldGen` instance ready to generate world chunks.
- **Throws:** `WorldGenLoadException` if there is an issue loading the world generation configuration or the generator itself.

### `public String toString()`
Returns a string representation of this `HytaleWorldGenProvider` instance, including its configured name and path.
- **Returns:** A `String` detailing the provider's current state.
