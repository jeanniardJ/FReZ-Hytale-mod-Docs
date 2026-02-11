# EventBus

L'`EventBus` est un composant central du système d'événements du serveur Hytale. Il facilite la communication entre les différentes parties du serveur et ses plugins. Il permet aux plugins d'enregistrer des "écouteurs" (`listeners`) pour des événements spécifiques et d'envoyer (`dispatch`) des événements, ce qui rend l'architecture du moddage très découplée et extensible.

Les développeurs de mods interagiront généralement avec l'`EventBus` pour :
- **Enregistrer des Écouteurs (`Listeners`)** : S'abonner aux événements qui les intéressent.
- **Envoyer des Événements (`Dispatch Events`)** : Déclencher leurs propres événements personnalisés pour notifier d'autres parties du serveur ou d'autres plugins.

## Constructeur

### `public EventBus(boolean timeEvents)`
Initialise une nouvelle instance de l'`EventBus`. Bien que les développeurs de mods obtiennent généralement l'instance de l'`EventBus` via `HytaleServer.get().getEventBus()`, comprendre ce constructeur clarifie sa configuration de base.
- **Paramètres :**
    - `timeEvents` : Un booléen indiquant si la mesure du temps d'exécution des événements doit être activée pour la surveillance des performances.

## Méthodes

### `public void shutdown()`
Arrête l'`EventBus` et désenregistre tous ses écouteurs et registres. Cette méthode est appelée en interne pendant l'arrêt du serveur.

### `public Set<Class<? extends IBaseEvent<?>>> getRegisteredEventClasses()`
Récupère un ensemble de toutes les classes d'événements pour lesquelles au moins un écouteur ou un "dispatcher" a été enregistré.
- **Retourne :** Un `Set` d'objets `Class` représentant les types d'événements enregistrés.

### `public Set<String> getRegisteredEventClassNames()`
Récupère un ensemble de noms de classes simples (par exemple, "PlayerJoinEvent" au lieu de "com.hypixel.hytale.event.player.PlayerJoinEvent") pour toutes les classes d'événements qui ont enregistré des écouteurs.
- **Retourne :** Un `Set` d'objets `String` représentant les noms simples des types d'événements enregistrés.

### `public <KeyType, EventType extends IBaseEvent<KeyType>> EventBusRegistry<KeyType, EventType, ?> getRegistry(@Nonnull Class<? super EventType> eventClass)`
La méthode principale pour obtenir un `EventBusRegistry` pour un type d'événement spécifique. Ce registre permet ensuite un contrôle précis sur l'enregistrement des écouteurs d'événements et leur envoi.
- **Paramètres :**
    - `eventClass` : L'objet `Class` du type d'événement pour lequel obtenir le registre.
- **Retourne :** Une instance d'`EventBusRegistry` spécifique à la `eventClass` fournie.

### `public <EventType extends IBaseEvent<Void>> EventRegistration<Void, EventType> register(@Nonnull Class<? super EventType> eventClass, @Nonnull Consumer<EventType> consumer)`
Enregistre un écouteur d'événements synchrone qui sera appelé chaque fois qu'un événement du type `eventClass` spécifié est envoyé. Cette surcharge est pour les événements qui n'utilisent pas de `KeyType` (c'est-à-dire `Void`).
- **Paramètres :**
    - `eventClass` : La `Class` de l'événement à écouter.
    - `consumer` : Une interface fonctionnelle `Consumer` qui sera exécutée lorsque l'événement est envoyé.
- **Retourne :** Un objet `EventRegistration`, qui peut être utilisé pour désenregistrer l'écouteur.

### `public <KeyType, EventType extends IBaseEvent<KeyType>> EventRegistration<KeyType, EventType> register(@Nonnull EventPriority priority, @Nonnull Class<? super EventType> eventClass, @Nullable KeyType key, @Nonnull Consumer<EventType> consumer)`
Enregistre un écouteur d'événements synchrone avec une priorité spécifiée et une clé optionnelle. La clé permet de filtrer les événements, garantissant que l'écouteur ne reçoit que les événements envoyés avec une clé correspondante.
- **Paramètres :**
    - `priority` : L'`EventPriority` (par exemple, `HIGHEST`, `NORMAL`, `LOWEST`) déterminant l'ordre d'invocation des écouteurs.
    - `eventClass` : La `Class` de l'événement à écouter.
    - `key` : Un objet `KeyType` optionnel pour filtrer les événements. Si `null`, l'écouteur recevra tous les événements de la `eventClass` donnée.
    - `consumer` : Une interface fonctionnelle `Consumer` qui sera exécutée lorsque l'événement est envoyé.
- **Retourne :** Un objet `EventRegistration`.

### `public <KeyType, EventType extends IAsyncEvent<KeyType>> EventRegistration<KeyType, EventType> registerAsync(@Nonnull EventPriority priority, @Nonnull Class<? super EventType> eventClass, @Nullable KeyType key, @Nonnull Function<CompletableFuture<EventType>, CompletableFuture<EventType>> function)`
Enregistre un écouteur d'événements asynchrone avec une priorité spécifiée et une clé optionnelle. Les écouteurs asynchrones reçoivent un `CompletableFuture` de l'événement et doivent retourner un `CompletableFuture` modifié. Cela permet de traiter les événements sans bloquer le thread principal.
- **Paramètres :**
    - `priority` : L'`EventPriority` déterminant l'ordre d'invocation des écouteurs.
    - `eventClass` : La `Class` de l'événement asynchrone à écouter.
    - `key` : Un objet `KeyType` optionnel pour filtrer les événements.
    - `function` : Une fonction (`Function`) qui prend un `CompletableFuture<EventType>` et retourne un `CompletableFuture<EventType>`, permettant un traitement et une modification asynchrones de l'événement.
- **Retourne :** Un objet `EventRegistration`.

### `public <KeyType, EventType extends IBaseEvent<KeyType>> EventRegistration<KeyType, EventType> registerGlobal(@Nonnull EventPriority priority, @Nonnull Class<? super EventType> eventClass, @Nonnull Consumer<EventType> consumer)`
Enregistre un écouteur d'événements synchrone global. Cet écouteur sera appelé pour tous les événements du type `eventClass` spécifié, indépendamment de toute clé spécifique qu'ils pourraient avoir.
- **Paramètres :**
    - `priority` : L'`EventPriority` pour cet écouteur.
    - `eventClass` : La `Class` de l'événement à écouter.
    - `consumer` : Une interface fonctionnelle `Consumer` pour gérer l'événement.
- **Retourne :** Un objet `EventRegistration`.

### `public <KeyType, EventType extends IAsyncEvent<KeyType>> EventRegistration<KeyType, EventType> registerAsyncGlobal(@Nonnull EventPriority priority, @Nonnull Class<? super EventType> eventClass, @Nonnull Function<CompletableFuture<EventType>, CompletableFuture<EventType>> function)`
Enregistre un écouteur d'événements asynchrone global.
- **Paramètres :**
    - `priority` : L'`EventPriority` pour cet écouteur.
    - `eventClass` : La `Class` de l'événement asynchrone à écouter globalement.
    - `function` : La `Function` pour gérer l'événement asynchrone.
- **Retourne :** Un objet `EventRegistration`.

### `public <KeyType, EventType extends IEvent<KeyType>> IEventDispatcher<EventType, EventType> dispatchFor(@Nonnull Class<? super EventType> eventClass, KeyType key)`
Récupère un `IEventDispatcher` pour envoyer des événements synchrones d'une `eventClass` et d'une `key` spécifiques. Le "dispatcher" permet de déclencher des événements qui seront ensuite traités par les écouteurs enregistrés.
- **Paramètres :**
    - `eventClass` : La `Class` de l'événement à envoyer.
    - `key` : La `KeyType` associée à l'événement.
- **Retourne :** Une instance de `IEventDispatcher`.

### `public <KeyType, EventType extends IAsyncEvent<KeyType>> IEventDispatcher<EventType, CompletableFuture<EventType>> dispatchForAsync(@Nonnull Class<? super EventType> eventClass, KeyType key)`
Récupère un `IEventDispatcher` pour envoyer des événements asynchrones d'une `eventClass` et d'une `key` spécifiques.
- **Paramètres :**
    - `eventClass` : La `Class` de l'événement asynchrone à envoyer.
    - `key` : La `KeyType` associée à l'événement.
- **Retourne :** Une instance de `IEventDispatcher` pour les événements asynchrones.
