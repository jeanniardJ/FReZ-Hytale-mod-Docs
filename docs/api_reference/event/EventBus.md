# EventBus

The `EventBus` is a central component of the Hytale server's event system, facilitating communication between different parts of the server and its plugins. It allows plugins to register listeners for specific events and to dispatch events, enabling a highly decoupled and extensible architecture.

Mod developers will typically interact with the `EventBus` to:
-   **Register Listeners:** Subscribe to events they are interested in.
-   **Dispatch Events:** Fire their own custom events to notify other parts of the server or other plugins.

## Constructor

### `public EventBus(boolean timeEvents)`
Initializes a new `EventBus` instance. While mod developers usually obtain the `EventBus` instance from `HytaleServer.get().getEventBus()`, understanding this constructor clarifies its basic setup.
- **Parameters:**
    - `timeEvents`: A boolean indicating whether event timing should be enabled for performance monitoring.

## Methods

### `public void shutdown()`
Shuts down the `EventBus` and unregisters all its listeners and registries. This method is called internally during server shutdown.

### `public Set<Class<? extends IBaseEvent<?>>> getRegisteredEventClasses()`
Retrieves a set of all event classes for which at least one listener or dispatcher has been registered.
- **Returns:** A `Set` of `Class` objects representing the registered event types.

### `public Set<String> getRegisteredEventClassNames()`
Retrieves a set of simple class names (e.g., "PlayerJoinEvent" instead of "com.hypixel.hytale.event.player.PlayerJoinEvent") for all event classes that have registered listeners.
- **Returns:** A `Set` of `String` objects representing the simple names of registered event types.

### `public <KeyType, EventType extends IBaseEvent<KeyType>> EventBusRegistry<KeyType, EventType, ?> getRegistry(@Nonnull Class<? super EventType> eventClass)`
The primary method for obtaining an `EventBusRegistry` for a specific event type. This registry then allows for fine-grained control over event listener registration and dispatching.
- **Parameters:**
    - `eventClass`: The `Class` object of the event type for which to get the registry.
- **Returns:** An `EventBusRegistry` instance specific to the provided `eventClass`.

### `public <EventType extends IBaseEvent<Void>> EventRegistration<Void, EventType> register(@Nonnull Class<? super EventType> eventClass, @Nonnull Consumer<EventType> consumer)`
Registers a synchronous event listener that will be invoked whenever an event of the specified `eventClass` is dispatched. This overload is for events that do not use a `KeyType` (i.e., `Void`).
- **Parameters:**
    - `eventClass`: The `Class` of the event to listen for.
    - `consumer`: A `Consumer` functional interface that will be executed when the event is dispatched.
- **Returns:** An `EventRegistration` object, which can be used to unregister the listener.

### `public <KeyType, EventType extends IBaseEvent<KeyType>> EventRegistration<KeyType, EventType> register(@Nonnull EventPriority priority, @Nonnull Class<? super EventType> eventClass, @Nullable KeyType key, @Nonnull Consumer<EventType> consumer)`
Registers a synchronous event listener with a specified priority and an optional key. The key allows for filtering events, ensuring the listener only receives events dispatched with a matching key.
- **Parameters:**
    - `priority`: The `EventPriority` (e.g., `HIGHEST`, `NORMAL`, `LOWEST`) determining the order of listener invocation.
    - `eventClass`: The `Class` of the event to listen for.
    - `key`: An optional `KeyType` object to filter events. If `null`, the listener will receive all events of the given `eventClass`.
    - `consumer`: A `Consumer` functional interface that will be executed when the event is dispatched.
- **Returns:** An `EventRegistration` object.

### `public <KeyType, EventType extends IAsyncEvent<KeyType>> EventRegistration<KeyType, EventType> registerAsync(@Nonnull EventPriority priority, @Nonnull Class<? super EventType> eventClass, @Nullable KeyType key, @Nonnull Function<CompletableFuture<EventType>, CompletableFuture<EventType>> function)`
Registers an asynchronous event listener with a specified priority and an optional key. Asynchronous listeners receive a `CompletableFuture` of the event and should return a modified `CompletableFuture`.
- **Parameters:**
    - `priority`: The `EventPriority` determining the order of listener invocation.
    - `eventClass`: The `Class` of the asynchronous event to listen for.
    - `key`: An optional `KeyType` object to filter events.
    - `function`: A `Function` that takes a `CompletableFuture<EventType>` and returns a `CompletableFuture<EventType>`, allowing for asynchronous event processing and modification.
- **Returns:** An `EventRegistration` object.

### `public <KeyType, EventType extends IBaseEvent<KeyType>> EventRegistration<KeyType, EventType> registerGlobal(@Nonnull EventPriority priority, @Nonnull Class<? super EventType> eventClass, @Nonnull Consumer<EventType> consumer)`
Registers a global synchronous event listener. This listener will be invoked for all events of the specified `eventClass`, regardless of any specific keys they might have.
- **Parameters:**
    - `priority`: The `EventPriority` for this listener.
    - `eventClass`: The `Class` of the event to listen for.
    - `consumer`: A `Consumer` functional interface to handle the event.
- **Returns:** An `EventRegistration` object.

### `public <KeyType, EventType extends IAsyncEvent<KeyType>> EventRegistration<KeyType, EventType> registerAsyncGlobal(@Nonnull EventPriority priority, @Nonnull Class<? super EventType> eventClass, @Nonnull Function<CompletableFuture<EventType>, CompletableFuture<EventType>> function)`
Registers a global asynchronous event listener.
- **Parameters:**
    - `priority`: The `EventPriority` for this listener.
    - `eventClass`: The `Class` of the asynchronous event to listen for globally.
    - `function`: The `Function` to handle the asynchronous event.
- **Returns:** An `EventRegistration` object.

### `public <KeyType, EventType extends IEvent<KeyType>> IEventDispatcher<EventType, EventType> dispatchFor(@Nonnull Class<? super EventType> eventClass, KeyType key)`
Retrieves an `IEventDispatcher` for dispatching synchronous events of a specific `eventClass` and `key`. The dispatcher allows for firing events that will then be processed by registered listeners.
- **Parameters:**
    - `eventClass`: The `Class` of the event to dispatch.
    - `key`: The `KeyType` associated with the event.
- **Returns:** An `IEventDispatcher` instance.

### `public <KeyType, EventType extends IAsyncEvent<KeyType>> IEventDispatcher<EventType, CompletableFuture<EventType>> dispatchForAsync(@Nonnull Class<? super EventType> eventClass, KeyType key)`
Retrieves an `IEventDispatcher` for dispatching asynchronous events of a specific `eventClass` and `key`.
- **Parameters:**
    - `eventClass`: The `Class` of the asynchronous event to dispatch.
    - `key`: The `KeyType` associated with the event.
- **Returns:** An `IEventDispatcher` instance for asynchronous events.
