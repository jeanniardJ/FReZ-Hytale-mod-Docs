# ArgTypes

`com.hypixel.hytale.server.core.command.system.arguments.types.ArgTypes`

A utility class containing standard argument types for command parsing.

## Standard Types

| Field | Type | Description | Examples |
|-------|------|-------------|----------|
| `BOOLEAN` | `Boolean` | Boolean values | `true`, `false` |
| `INTEGER` | `Integer` | Integer numbers | `-1`, `0`, `1`, `56346` |
| `STRING` | `String` | String text | `"Hello"`, `"Text"` |
| `FLOAT` | `Float` | Floating point numbers | `3.14`, `-2.5` |
| `DOUBLE` | `Double` | Double precision numbers | `-3.14`, `0.0` |
| `UUID` | `UUID` | UUID strings | `550e8400-e29b-41d4-a716-446655440000` |

## Player & Entity Types

| Field | Type | Description |
|-------|------|-------------|
| `PLAYER_UUID` | `UUID` | Resolves a player name or UUID string to a UUID. Checks online players first. |
| `GAME_PROFILE_LOOKUP` | `PublicGameProfile` | Synchronous lookup of a game profile by name or UUID. |
| `GAME_PROFILE_LOOKUP_ASYNC` | `CompletableFuture<PublicGameProfile>` | Asynchronous lookup of a game profile. |
| `PLAYER_REF` | `PlayerRef` | Resolves a player username to a `PlayerRef`. |
| `ENTITY_ID` | `UUID` | Wrapped argument for Entity ID. |

## Coordinate & Position Types

| Field | Type | Description | Examples |
|-------|------|-------------|----------|
| `RELATIVE_DOUBLE_COORD` | `Coord` | Coordinate that can be relative (`~`). | `5.0`, `~-2.3` |
| `RELATIVE_INT_COORD` | `IntCoord` | Integer coordinate that can be relative (`~`). | `5`, `~-2` |
| `RELATIVE_INTEGER` | `RelativeInteger` | Integer that can be relative. | `5`, `~-2` |
| `RELATIVE_FLOAT` | `RelativeFloat` | Float that can be relative. | `90.0`, `~` |
| `VECTOR2I` | `Vector2i` | 2D Integer Vector (x, z). | `124 232` |
| `VECTOR3I` | `Vector3i` | 3D Integer Vector (x, y, z). | `124 232 234` |
| `RELATIVE_VECTOR3I` | `RelativeVector3i` | 3D Relative Integer Vector. | `~5 0 ~-3` |
| `RELATIVE_BLOCK_POSITION` | `RelativeIntPosition` | Relative block position (x, y, z). | `~5 ~ ~-3` |
| `RELATIVE_POSITION` | `RelativeDoublePosition` | Relative precise position (x, y, z). | `~5.5 ~ ~` |
| `RELATIVE_CHUNK_POSITION` | `RelativeChunkPosition` | Relative chunk coordinates (x, z). | `5 10`, `~ ~` |
| `ROTATION` | `Vector3f` | Rotation vector (pitch, yaw, roll). | `0 90 0` |

## Asset Types

These types resolve to specific game assets.

* `MODEL_ASSET`: `ModelAsset`
* `WEATHER_ASSET`: `Weather`
* `INTERACTION_ASSET`: `Interaction`
* `ROOT_INTERACTION_ASSET`: `RootInteraction`
* `EFFECT_ASSET`: `EntityEffect`
* `ENVIRONMENT_ASSET`: `Environment`
* `ITEM_ASSET`: `Item`
* `BLOCK_TYPE_ASSET`: `BlockType`
* `PARTICLE_SYSTEM`: `ParticleSystem`
* `HITBOX_COLLISION_CONFIG`: `HitboxCollisionConfig`
* `REPULSION_CONFIG`: `RepulsionConfig`
* `SOUND_EVENT_ASSET`: `SoundEvent`
* `AMBIENCE_FX_ASSET`: `AmbienceFX`

## Block & World Types

| Field | Type | Description |
|-------|------|-------------|
| `WORLD` | `World` | Resolves a world name to a `World` object. |
| `BLOCK_TYPE_KEY` | `String` | A block type identifier string. |
| `BLOCK_ID` | `Integer` | Resolves a block type key to its integer ID. |
| `WEIGHTED_BLOCK_TYPE` | `Pair<Integer, String>` | A block type with an associated weight. |
| `BLOCK_PATTERN` | `BlockPattern` | A pattern of blocks with weights. |
| `BLOCK_MASK` | `BlockMask` | A mask for filtering blocks (e.g. `>Grass_Full`, `!Fluid_Water`). |

## Miscellaneous Types

| Field | Type | Description |
|-------|------|-------------|
| `COLOR` | `Integer` | Parses hex colors (`#RRGGBB`, `0x...`) or integers. |
| `TICK_RATE` | `Integer` | Parses tick rates or durations (`30tps`, `50ms`). |
| `GAME_MODE` | `GameMode` | Game mode selection. |
| `SOUND_CATEGORY` | `SoundCategory` | Sound category enum. |
| `INT_RANGE` | `Pair<Integer, Integer>` | Range of two integers (min max). |
| `RELATIVE_INT_RANGE` | `RelativeIntegerRange` | Range of two relative integers. |
| `INTEGER_COMPARISON_OPERATOR` | `IntegerComparisonOperator` | Operators: `>`, `<`, `>=`, `<=`, `%`, `=`, `!=` |
| `INTEGER_OPERATION` | `IntegerOperation` | Operators: `+`, `-`, `*`, `/`, `%`, `=` |

## Helper Methods

### forEnum

```java
public static <E extends Enum<E>> SingleArgumentType<E> forEnum(String name, @Nonnull Class<E> enumType)
```
Creates an argument type for a specific Enum class.
