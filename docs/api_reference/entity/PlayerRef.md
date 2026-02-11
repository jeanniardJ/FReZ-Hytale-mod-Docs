# PlayerRef (Référence de Joueur)

La classe `PlayerRef` représente une référence légère et persistante à un joueur connecté au sein de l'`Universe` (l'univers) de Hytale. Elle agit comme un pont entre la connexion réseau du joueur et ses données d'entité en jeu, qui sont stockées dans un `EntityStore` sous forme de "composants" (`Component`). Les développeurs de mods utiliseront `PlayerRef` pour interagir avec les joueurs, leur envoyer des messages, gérer leur connexion et accéder à leurs composants d'entité associés.

## Méthodes Statiques

### `public static ComponentType<EntityStore, PlayerRef> getComponentType()`
Récupère le `ComponentType` spécifiquement associé aux composants `PlayerRef`. C'est essentiel pour accéder ou manipuler les instances de `PlayerRef` au sein d'un `EntityStore`.
- **Retourne :** Le `ComponentType` pour `PlayerRef`.

## Constructeur

### `public PlayerRef(@Nonnull Holder<EntityStore> holder, @Nonnull UUID uuid, @Nonnull String username, @Nonnull String language, @Nonnull PacketHandler packetHandler, @Nonnull ChunkTracker chunkTracker)`
Initialise une nouvelle instance de `PlayerRef`. Ce constructeur est principalement utilisé en interne par le serveur lors de la connexion et du chargement du joueur. Vous n'aurez pas à l'appeler directement.
- **Paramètres :**
    - `holder` : Le `Holder` (un conteneur) contenant les composants d'entité initiaux du joueur.
    - `uuid` : L'identifiant unique (`UUID`) du joueur.
    - `username` : Le nom d'utilisateur du joueur.
    - `language` : La langue préférée du joueur.
    - `packetHandler` : Le `PacketHandler` (gestionnaire de paquets) responsable de la communication réseau avec ce joueur.
    - `chunkTracker` : L'instance de `ChunkTracker` qui gère les "chunks" (morceaux de monde) que le joueur est en train de visualiser.

## Méthodes

### `public boolean isValid()`
Vérifie si cette `PlayerRef` est actuellement valide, c'est-à-dire si elle est associée à une entité joueur active, soit en tant que `Holder`, soit en tant que `Ref<EntityStore>`.
- **Retourne :** `true` si valide, `false` sinon.

### `public Ref<EntityStore> getReference()`
Récupère la `Ref<EntityStore>` qui pointe vers les données d'entité réelles du joueur dans l'`EntityStore` une fois que l'entité joueur est chargée dans un monde.
- **Retourne :** La `Ref<EntityStore>` pour ce joueur, ou `null` si l'entité joueur n'est pas encore entièrement dans un monde.

### `public Holder<EntityStore> getHolder()`
Récupère le `Holder<EntityStore>` qui détient temporairement les composants d'entité du joueur avant qu'ils ne soient entièrement ajoutés à un `EntityStore`. Ceci est généralement utilisé pendant le processus de chargement du joueur.
- **Retourne :** Le `Holder<EntityStore>` pour ce joueur, ou `null` si l'entité du joueur est déjà dans un `EntityStore`.

### `public UUID getUuid()`
Retourne l'identifiant unique (`UUID`) du joueur. Cet `UUID` est persistant d'une session à l'autre et identifie le joueur de manière fiable.
- **Retourne :** L'`UUID` du joueur.

### `public String getUsername()`
Retourne le nom d'utilisateur actuel du joueur.
- **Retourne :** Le nom d'utilisateur du joueur sous forme de `String`.

### `public PacketHandler getPacketHandler()`
Récupère le `PacketHandler` associé à la connexion réseau de ce joueur. Les développeurs de mods peuvent l'utiliser pour envoyer des paquets personnalisés directement au client du joueur. C'est le moyen de communiquer directement avec le jeu du joueur.
- **Retourne :** L'instance de `PacketHandler` pour ce joueur.

### `public ChunkTracker getChunkTracker()`
Récupère l'instance de `ChunkTracker` pour ce joueur. Le `ChunkTracker` gère les "chunks" (morceaux de monde) que le joueur est en train de visualiser ou qu'il a chargés, ce qui est crucial pour le rendu et la logique du jeu autour du joueur.
- **Retourne :** Le `ChunkTracker` pour ce joueur.

### `public HiddenPlayersManager getHiddenPlayersManager()`
Récupère le `HiddenPlayersManager` pour ce joueur. Ce gestionnaire contrôle quels autres joueurs sont cachés de la vue de ce joueur spécifique (par exemple, pour la furtivité ou des événements spéciaux).
- **Retourne :** L'instance de `HiddenPlayersManager`.

### `public String getLanguage()`
Retourne la préférence linguistique actuellement définie du joueur (par exemple, "en_US", "fr_FR").
- **Retourne :** Le code de langue sous forme de `String`.

### `public void setLanguage(@Nonnull String language)`
Définit la préférence linguistique du joueur.
- **Paramètres :**
    - `language` : Le nouveau code de langue (par exemple, "en_US").

### `public Transform getTransform()`
Retourne un objet `Transform` représentant la position, la rotation et l'échelle actuelles du joueur dans le monde du jeu. Cet objet est mutable, ce qui signifie que vous pouvez modifier ses valeurs.
- **Retourne :** Le `Transform` du joueur.

### `public UUID getWorldUuid()`
Récupère l'`UUID` du `World` (monde) dans lequel le joueur est actuellement situé.
- **Retourne :** L'`UUID` du monde du joueur, ou `null` si le joueur n'est pas encore entièrement dans un monde.

### `public Vector3f getHeadRotation()`
Retourne un `Vector3f` représentant la rotation de la tête du joueur (tangage, lacet, roulis).
- **Retourne :** La rotation de la tête du joueur (`Vector3f`).

### `public void updatePosition(@Nonnull World world, @Nonnull Transform transform, @Nonnull Vector3f headRotation)`
Met à jour l'association du `World` (monde), la position et la rotation de la tête du joueur. Cette méthode est généralement appelée par le système de mouvement du serveur.
- **Paramètres :**
    - `world` : Le `World` dans lequel le joueur se trouve maintenant.
    - `transform` : La nouvelle `Transform` (position et rotation).
    - `headRotation` : La nouvelle rotation de la tête (`Vector3f`).

### `public void referToServer(@Nonnull String host, int port)`
Demande au client du joueur de se connecter à un autre serveur à l'hôte et au port spécifiés. Cela permet de transférer les joueurs entre différents serveurs ou instances du jeu.
- **Paramètres :**
    - `host` : Le nom d'hôte ou l'adresse IP du serveur cible.
    - `port` : Le port du serveur cible.

### `public void referToServer(@Nonnull String host, int port, @Nullable byte[] data)`
Demande au client du joueur de se connecter à un autre serveur, en envoyant éventuellement des données supplémentaires sous forme de tableau d'octets. Cela peut être utilisé pour transmettre des jetons de session ou d'autres informations pertinentes au nouveau serveur.
- **Paramètres :**
    - `host` : Le nom d'hôte ou l'adresse IP du serveur cible.
    - `port` : Le port du serveur cible.
    - `data` : Un tableau d'octets optionnel contenant des données de référence supplémentaires (taille maximale de 4096 octets).

### `public void sendMessage(@Nonnull Message message)`
Envoie un objet `Message` formaté à ce joueur spécifique. Le message sera affiché dans le chat du joueur.
- **Paramètres :**
    - `message` : L'objet `Message` à envoyer.

## Méthodes Dépréciées

### `public <T extends Component<EntityStore>> T getComponent(@Nonnull ComponentType<EntityStore, T> componentType)`
*Déprécié.* Récupère un composant du "holder" d'entité ou du "store" du joueur. Il est généralement recommandé d'accéder aux composants directement via l'instance d'`Entity` du joueur (si disponible) ou en utilisant l'API `EntityStore` pour une approche plus moderne et basée sur les composants.

### `public void replaceHolder(@Nonnull Holder<EntityStore> holder)`
*Déprécié.* Remplace le `Holder` interne pour cette `PlayerRef`. Il s'agit d'une méthode interne probablement utilisée lors de transitions complexes de l'état du joueur.

### `public Component<EntityStore> clone()`
*Note : Cette méthode retourne `this`.* Les instances de `PlayerRef` sont essentiellement des références et ne sont pas conçues pour être clonées au sens traditionnel. Cette implémentation suggère qu'une `PlayerRef` est unique par joueur et que son opération de clonage renvoie simplement la même instance.
