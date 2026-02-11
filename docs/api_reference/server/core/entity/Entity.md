# Entity

The `Entity` class represents any interactive object or being within the Hytale world. This can include players, NPCs, creatures, items, and more. Entities are built upon a component-based architecture, meaning their behaviors and properties are defined by various `Component`s attached to them and stored within an `EntityStore`.

Many methods related to direct manipulation of entity properties might be marked `@Deprecated` or `forRemoval = true`, indicating a shift towards a more component-driven approach for modifying entity behavior and data. Mod developers should favor interacting with entities through their attached `Component`s and the `EntityStore` where possible.

## Constants

### `public static final int VERSION = 5`
The current version of the entity serialization format.

### `public static final KeyedCodec<Model.ModelReference> MODEL`
A `KeyedCodec` used for encoding and decoding the `Model.ModelReference` component of an entity, which defines its visual model.

### `public static final KeyedCodec<String> DISPLAY_NAME`
A `KeyedCodec` used for encoding and decoding the display name of an entity.

### `public static final KeyedCodec<UUID> UUID`
A `KeyedCodec` used for encoding and decoding the unique `UUID` of an entity.

### `public static final BuilderCodec<Entity> CODEC`
A `BuilderCodec` used for the serialization and deserialization of `Entity` objects.

### `public static final int UNASSIGNED_ID = -1`
A constant representing an unassigned network ID for an entity.

## Constructors

### `public Entity()`
The default constructor for creating an `Entity` instance.

### `public Entity(@Nullable World world)`
*Deprecated.* A constructor that initializes an `Entity` and associates it with a specific `World`. This method is deprecated, suggesting that entities should be created and managed through more explicit component-based systems or dedicated entity creation methods.

## Methods

### `public boolean remove()`
Attempts to remove the entity from its current world. This will trigger an `EntityRemoveEvent` and clear the entity's reference in the `EntityStore`.
- **Returns:** `true` if the entity was successfully marked for removal, `false` otherwise (e.g., if it was already removed).

### `public void loadIntoWorld(@Nonnull World world)`
Loads this entity into the specified `World`. This method sets the entity's associated `World` and assigns a network ID if one hasn't been assigned yet.
- **Parameters:**
    - `world`: The `World` instance into which the entity should be loaded.
- **Throws:** `IllegalArgumentException` if the entity is already in a world.

### `public void unloadFromWorld()`
Unloads the entity from its current `World`. This clears the entity's `World` association and resets its network ID.
- **Throws:** `IllegalArgumentException` if the entity is not currently in a world.

### `public World getWorld()`
Retrieves the `World` instance that this entity is currently loaded into.
- **Returns:** The `World` instance, or `null` if the entity is not currently in a world.

### `public boolean wasRemoved()`
Checks if this entity has been marked for removal.
- **Returns:** `true` if the entity was removed, `false` otherwise.

### `public boolean isCollidable()`
Checks if this entity is considered collidable.
- **Returns:** `true` if the entity can collide with other objects, `false` otherwise.

### `public boolean isHiddenFromLivingEntity(@Nonnull Ref<EntityStore> ref, @Nonnull Ref<EntityStore> targetRef, @Nonnull ComponentAccessor<EntityStore> componentAccessor)`
Determines if this entity should be hidden from a specified living entity.
- **Parameters:**
    - `ref`: A `Ref` to this entity in the `EntityStore`.
    - `targetRef`: A `Ref` to the target living entity.
    - `componentAccessor`: An accessor for entity components.
- **Returns:** `true` if this entity should be hidden from the target entity, `false` otherwise.

### `public void setReference(@Nonnull Ref<EntityStore> reference)`
Sets the internal `Ref` that points to this entity's data within the `EntityStore`. This method should typically be managed by the entity system.
- **Parameters:**
    - `reference`: The `Ref` to associate with this entity.
- **Throws:** `IllegalArgumentException` if the entity already has a valid reference.

### `public Ref<EntityStore> getReference()`
Retrieves the `Ref` that points to this entity's data within the `EntityStore`.
- **Returns:** The `Ref<EntityStore>` for this entity, or `null` if not set.

### `public Component<EntityStore> clone()`
Creates a deep clone of this `Entity` instance, including all its components.
- **Returns:** A new `Entity` instance that is a copy of the current one.

### `public Holder<EntityStore> toHolder()`
Converts this `Entity` instance and its associated components into an `Holder<EntityStore>`, which is a container for entity data used by the component system.
- **Returns:** An `Holder<EntityStore>` containing the entity's data.

## Deprecated Methods

The following methods are deprecated and should be avoided in new code. They are likely retained for backward compatibility or are being phased out in favor of component-based interactions.

### `public void setLegacyUUID(@Nullable UUID uuid)`
*Deprecated.* Sets a legacy `UUID` for the entity.

### `public int getNetworkId()`
*Deprecated.* Retrieves the network ID assigned to this entity.

### `public String getLegacyDisplayName()`
*Deprecated.* Retrieves the legacy display name of the entity.

### `public UUID getUuid()`
*Deprecated.* Retrieves the `UUID` of the entity.

### `public void setTransformComponent(TransformComponent transform)`
*Deprecated.* Sets the `TransformComponent` for the entity.

### `public TransformComponent getTransformComponent()`
*Deprecated.* Retrieves the `TransformComponent` of the entity.

### `public void moveTo(@Nonnull Ref<EntityStore> ref, double locX, double locY, double locZ, @Nonnull ComponentAccessor<EntityStore> componentAccessor)`
*Deprecated.* Moves the entity to the specified coordinates.

### `public void clearReference()`
*Deprecated.* Clears the entity's internal reference.

## Nested Class: `DefaultAnimations`

The `DefaultAnimations` nested static class provides utility methods and constants for common animation IDs.

### Constants

-   `public static final String DEATH = "Death"`
-   `public static final String HURT = "Hurt"`
-   `public static final String DESPAWN = "Despawn"`
-   `public static final String SWIM_SUFFIX = "Swim"`
-   `public static final String FLY_SUFFIX = "Fly"`

### Methods

### `public static String[] getHurtAnimationIds(@Nonnull MovementStates movementStates, @Nonnull DamageCause damageCause)`
Generates an array of animation IDs relevant to being hurt, considering the entity's `MovementStates` (e.g., swimming, flying) and the `DamageCause`.
- **Parameters:**
    - `movementStates`: The current `MovementStates` of the entity.
    - `damageCause`: The `DamageCause` that inflicted the damage.
- **Returns:** An array of `String` animation IDs.

### `public static String[] getDeathAnimationIds(@Nonnull MovementStates movementStates, @Nonnull DamageCause damageCause)`
Generates an array of animation IDs relevant to the entity's death, considering its `MovementStates` and the `DamageCause`.
- **Parameters:**
    - `movementStates`: The current `MovementStates` of the entity.
    - `damageCause`: The `DamageCause` that led to the death.
- **Returns:** An array of `String` animation IDs.
