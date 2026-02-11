# Univers (Universe)

La classe `Universe` est un composant central et "singleton" (unique) au sein du serveur Hytale. Elle agit comme le gestionnaire principal de tous les mondes, des joueurs et de l'état global du jeu. Elle étend `JavaPlugin`, ce qui indique son rôle de plugin de base fournissant des fonctionnalités serveur fondamentales. Les développeurs de mods interagiront fréquemment avec l'`Universe` pour accéder aux instances de monde, aux données des joueurs et aux événements globaux.

## Constantes

### `public static final PluginManifest MANIFEST`
Le `PluginManifest` pour le plugin "core" `Universe`, contenant ses métadonnées et ses dépendances.

### `public static final MetricsRegistry<Universe> METRICS_REGISTRY`
Un `MetricsRegistry` spécifiquement pour la classe `Universe`, utilisé pour suivre et rapporter diverses métriques à l'échelle du serveur.

### `public static Map<Integer, String> LEGACY_BLOCK_ID_MAP`
*Déprécié.* Une carte (`Map`) fournissant une correspondance entre les anciens ID de bloc ("legacy block IDs") et leurs représentations sous forme de chaînes de caractères. Ceci est probablement destiné à la compatibilité ascendante avec les anciens formats de monde.

## Méthodes Statiques

### `public static Universe get()`
Récupère l'instance "singleton" de l'`Universe`. C'est le point d'entrée principal pour accéder à toutes les fonctionnalités au niveau de l'univers.
- **Retourne :** L'instance unique de l'`Universe`.

### `public static Path getWorldGenPath()`
Retourne le `Path` (chemin de fichier) vers le répertoire où sont stockées les configurations et les ressources de génération de monde.
- **Retourne :** Un objet `Path` pointant vers le répertoire de génération de monde.

## Méthodes

### `public CompletableFuture<Void> runBackup()`
Lance une opération de sauvegarde asynchrone (en arrière-plan) pour l'ensemble de l'univers. Cela inclut la sauvegarde de tous les mondes actuellement chargés.
- **Retourne :** Un `CompletableFuture<Void>` qui se termine lorsque le processus de sauvegarde est terminé.

### `public void disconnectAllPLayers()`
Force tous les joueurs actuellement connectés à se déconnecter du serveur. Ceci est généralement utilisé lors de l'arrêt du serveur ou de la maintenance.

### `public void shutdownAllWorlds()`
Lance le processus d'arrêt pour tous les mondes actuellement chargés, garantissant que leurs données sont sauvegardées et que les ressources sont libérées. Ceci est également généralement utilisé lors de l'arrêt du serveur.

### `public CompletableFuture<Void> getUniverseReady()`
Retourne un `CompletableFuture` qui se termine une fois que l'`Universe` a entièrement chargé tous ses composants, ses mondes, et est prêt à accepter les connexions des joueurs. Les développeurs de mods peuvent l'utiliser pour s'assurer que le serveur est entièrement opérationnel avant d'exécuter une certaine logique.
- **Retourne :** Un `CompletableFuture<Void>`.

### `public boolean isWorldLoadable(@Nonnull String name)`
Vérifie si un monde portant le `name` donné existe sur le disque dans la structure de répertoires de l'univers et est dans un état où il peut être chargé.
- **Paramètres :**
    - `name` : Le nom du monde à vérifier.
- **Retourne :** `true` si le monde est chargeable, `false` sinon.

### `public CompletableFuture<World> addWorld(@Nonnull String name)`
Ajoute de manière asynchrone un nouveau monde à l'univers avec le `name` spécifié, en utilisant les configurations par défaut de génération de monde et de stockage.
- **Paramètres :**
    - `name` : Le nom du nouveau monde.
- **Retourne :** Un `CompletableFuture<World>` qui se termine avec l'instance `World` nouvellement créée.
- **Lance :** `IllegalArgumentException` si un monde portant le nom donné existe déjà ou est chargeable depuis le disque.

### `public CompletableFuture<World> addWorld(@Nonnull String name, @Nullable String generatorType, @Nullable String chunkStorageType)`
*Déprécié.* Ajoute de manière asynchrone un nouveau monde à l'univers avec le `name` spécifié, permettant des `generatorType` (type de générateur) et `chunkStorageType` (type de stockage de "chunks") personnalisés. Cette méthode est dépréciée, ce qui suggère que la création de monde avec des fournisseurs personnalisés pourrait être gérée différemment dans les nouvelles versions.
- **Paramètres :**
    - `name` : Le nom du nouveau monde.
    - `generatorType` : Une chaîne de caractères optionnelle spécifiant le type de générateur de monde à utiliser (par exemple, "Hytale", "Flat").
    - `chunkStorageType` : Une chaîne de caractères optionnelle spécifiant le type de fournisseur de stockage de "chunks" à utiliser.
- **Retourne :** Un `CompletableFuture<World>`.

### `public CompletableFuture<World> makeWorld(@Nonnull String name, @Nonnull Path savePath, @Nonnull WorldConfig worldConfig)`
Crée de manière asynchrone une instance de `World` avec un `name`, un `savePath` (chemin de sauvegarde) et une `WorldConfig` (configuration du monde) donnés. C'est un moyen plus direct d'instancier un monde.
- **Paramètres :**
    - `name` : Le nom du monde.
    - `savePath` : Le `Path` où les données du monde seront sauvegardées.
    - `worldConfig` : La `WorldConfig` définissant les propriétés du monde.
- **Retourne :** Un `CompletableFuture<World>`.

### `public CompletableFuture<World> loadWorld(@Nonnull String name)`
Charge de manière asynchrone un monde existant depuis le disque avec le `name` spécifié.
- **Paramètres :**
    - `name` : Le nom du monde à charger.
- **Retourne :** Un `CompletableFuture<World>` qui se termine avec l'instance `World` chargée.
- **Lance :** `IllegalArgumentException` si le monde est déjà chargé ou n'existe pas sur le disque.

### `public World getWorld(@Nullable String worldName)`
Récupère une instance de `World` chargée par son nom. Le nom est insensible à la casse.
- **Paramètres :**
    - `worldName` : Le nom du monde à récupérer.
- **Retourne :** L'instance de `World` si trouvée, `null` sinon.

### `public World getWorld(@Nonnull UUID uuid)`
Récupère une instance de `World` chargée par son `UUID` unique.
- **Paramètres :**
    - `uuid` : L'`UUID` du monde à récupérer.
- **Retourne :** L'instance de `World` si trouvée, `null` sinon.

### `public World getDefaultWorld()`
Récupère l'instance de `World` configurée comme monde par défaut pour le serveur.
- **Retourne :** L'instance du monde par défaut, ou `null` si aucun monde par défaut n'est configuré ou chargé.

### `public boolean removeWorld(@Nonnull String name)`
Supprime un monde portant le `name` spécifié de l'univers. Cela arrêtera le monde et sauvegardera ses données, si nécessaire, puis le déchargera.
- **Paramètres :**
    - `name` : Le nom du monde à supprimer.
- **Retourne :** `true` si le monde a été supprimé avec succès, `false` si la suppression a été annulée par un écouteur d'événements.
- **Lance :** `NullPointerException` si le monde n'existe pas.

### `public Path getPath()`
Récupère le `Path` racine du répertoire de l'univers où toutes les données du monde et d'autres fichiers au niveau de l'univers sont stockées.
- **Retourne :** Un objet `Path`.

### `public Map<String, World> getWorlds()`
Retourne une `Map` (carte) non modifiable de toutes les instances de `World` actuellement chargées dans l'univers, mappées par leurs noms (insensible à la casse).
- **Retourne :** Une `Map<String, World>` non modifiable.

### `public List<PlayerRef> getPlayers()`
Retourne une liste de toutes les instances de `PlayerRef` représentant les joueurs actuellement connectés.
- **Retourne :** Une `List` d'objets `PlayerRef`.

### `public PlayerRef getPlayer(@Nonnull UUID uuid)`
Récupère une `PlayerRef` pour un joueur connecté en utilisant son `UUID`.
- **Paramètres :**
    - `uuid` : L'`UUID` du joueur à récupérer.
- **Retourne :** L'instance de `PlayerRef` si le joueur est connecté, `null` sinon.

### `public PlayerRef getPlayer(@Nonnull String value, @Nonnull NameMatching matching)`
Récupère une `PlayerRef` pour un joueur connecté en faisant correspondre son nom d'utilisateur à la `value` fournie en utilisant une stratégie `NameMatching` spécifiée.
- **Paramètres :**
    - `value` : La chaîne à faire correspondre aux noms d'utilisateur des joueurs.
    - `matching` : La stratégie `NameMatching` à utiliser (par exemple, `EXACT`, `STARTS_WITH`, `CONTAINS`).
- **Retourne :** L'instance de `PlayerRef` si une correspondance est trouvée, `null` sinon.

### `public int getPlayerCount()`
Retourne le nombre total de joueurs actuellement connectés au serveur.
- **Retourne :** Un `int` représentant le nombre de joueurs.

### `public void removePlayer(@Nonnull PlayerRef playerRef)`
Supprime un joueur connecté de l'univers. Cela gère sa déconnexion et son nettoyage.
- **Paramètres :**
    - `playerRef` : Le `PlayerRef` du joueur à supprimer.

### `public void sendMessage(@Nonnull Message message)`
Diffuse un `Message` à tous les joueurs actuellement connectés dans l'univers.
- **Paramètres :**
    - `message` : L'objet `Message` à envoyer.

### `public void broadcastPacket(@Nonnull Packet packet)`
Diffuse un seul `Packet` à tous les joueurs actuellement connectés.
- **Paramètres :**
    - `packet` : Le `Packet` à envoyer.

### `public void broadcastPacket(@Nonnull Packet... packets)`
Diffuse plusieurs `Packet`s à tous les joueurs actuellement connectés.
- **Paramètres :**
    - `packets` : Un tableau d'objets `Packet` à envoyer.

### `public PlayerStorage getPlayerStorage()`
Récupère l'instance de `PlayerStorage` responsable du chargement et de la sauvegarde des données des joueurs.
- **Retourne :** L'instance de `PlayerStorage`.

### `public WorldConfigProvider getWorldConfigProvider()`
Récupère l'instance de `WorldConfigProvider` utilisée pour charger et gérer les configurations de monde.
- **Retourne :** L'instance de `WorldConfigProvider`.
