# PluginBase

La classe abstraite `PluginBase` sert de fondation à tous les plugins au sein de l'environnement serveur Hytale. Elle fournit des fonctionnalités de base, gère le cycle de vie des plugins et offre un accès à divers registres du serveur que les développeurs de mods utiliseront pour étendre et personnaliser le jeu. Tout plugin, y compris le `JavaPlugin`, étend `PluginBase`.

## Constructeur

### `public PluginBase(@Nonnull PluginInit init)`
Initialise une nouvelle instance de `PluginBase`. Ce constructeur est généralement appelé par le système de chargement des plugins avec des paramètres d'initialisation. Vous n'aurez pas à l'appeler directement.
- **Paramètres :**
    - `init` : Un objet d'initialisation (`PluginInit`) contenant des informations essentielles sur le plugin comme son manifeste, son répertoire de données et si il fait partie du "classpath" du serveur (la liste des chemins où Java cherche les classes).

## Méthodes

### `public String getName()`
Récupère le nom lisible par l'homme du plugin, tel que défini dans son manifeste (le fichier de configuration du plugin).
- **Retourne :** Le nom du plugin sous forme de `String`.

### `public HytaleLogger getLogger()`
Fournit une instance dédiée de `HytaleLogger` pour ce plugin. Cela vous permet d'enregistrer des messages (logs) qui seront préfixés par le nom de votre plugin, facilitant le débogage.
- **Retourne :** L'instance du `HytaleLogger` de ce plugin.

### `public PluginIdentifier getIdentifier()`
Récupère l'identifiant unique du plugin. Il s'agit généralement d'une combinaison du groupe et du nom du plugin (par exemple, "com.monentreprise.monplugin").
- **Retourne :** Le `PluginIdentifier` de ce plugin.

### `public PluginManifest getManifest()`
Retourne l'objet `PluginManifest` pour ce plugin. Il contient des métadonnées importantes telles que la version du plugin, ses dépendances et les informations sur l'auteur.
- **Retourne :** Le `PluginManifest` du plugin.

### `public Path getDataDirectory()`
Récupère le chemin du système de fichiers vers le répertoire de données persistant du plugin. Les plugins peuvent utiliser ce répertoire pour stocker des fichiers de configuration, des données sauvegardées et d'autres ressources qui doivent survivre aux redémarrages du serveur.
- **Retourne :** Un objet `Path` représentant le répertoire de données du plugin.

### `public PluginState getState()`
Retourne l'état actuel du cycle de vie du plugin (par exemple, `NONE` pour non chargé, `SETUP` pour configuré, `START` pour en démarrage, `ENABLED` pour activé, `DISABLED` pour désactivé, ou `SHUTDOWN` pour en arrêt).
- **Retourne :** Une énumération `PluginState`.

### `public ClientFeatureRegistry getClientFeatureRegistry()`
Fournit un accès au `ClientFeatureRegistry`. Cela permet aux plugins d'enregistrer des fonctionnalités personnalisées côté client, comme des éléments d'interface utilisateur (UI) ou une logique de rendu spécifique.
- **Retourne :** L'instance du `ClientFeatureRegistry`.

### `public CommandRegistry getCommandRegistry()`
Fournit un accès au `CommandRegistry`. Cela permet aux plugins d'enregistrer de nouvelles commandes serveur qui peuvent être exécutées par les joueurs ou la console.
- **Retourne :** L'instance du `CommandRegistry`.

### `public EventRegistry getEventRegistry()`
Fournit un accès à l'`EventRegistry`. Cela permet aux plugins d'enregistrer des écouteurs (listeners) pour divers événements du serveur, afin de réagir aux actions des joueurs, aux changements du monde, etc.
- **Retourne :** L'instance de l'`EventRegistry`.

### `public BlockStateRegistry getBlockStateRegistry()`
Fournit un accès au `BlockStateRegistry`, permettant aux plugins d'enregistrer des états de blocs personnalisés.
- **Retourne :** L'instance du `BlockStateRegistry`.

### `public EntityRegistry getEntityRegistry()`
Fournit un accès à l'`EntityRegistry`, permettant aux plugins d'enregistrer des types d'entités personnalisés (créatures, objets spéciaux, etc.).
- **Retourne :** L'instance de l'`EntityRegistry`.

### `public TaskRegistry getTaskRegistry()`
Fournit un accès au `TaskRegistry`, permettant aux plugins de planifier l'exécution de tâches personnalisées à des intervalles spécifiques ou après un certain délai (par exemple, un événement toutes les 5 minutes).
- **Retourne :** L'instance du `TaskRegistry`.

### `public ComponentRegistryProxy<EntityStore> getEntityStoreRegistry()`
Fournit un "proxy" (un intermédiaire) au registre des composants de l'`EntityStore`. Cela permet aux plugins d'enregistrer des composants personnalisés qui peuvent être attachés aux entités et sauvegardés.
- **Retourne :** Un `ComponentRegistryProxy` pour `EntityStore`.

### `public ComponentRegistryProxy<ChunkStore> getChunkStoreRegistry()`
Fournit un "proxy" au registre des composants du `ChunkStore`. Cela permet aux plugins d'enregistrer des composants personnalisés qui peuvent être attachés aux "chunks" (morceaux de monde) et sauvegardés.
- **Retourne :** Un `ComponentRegistryProxy` pour `ChunkStore`.

### `public AssetRegistry getAssetRegistry()`
Fournit un accès à l'`AssetRegistry`, qui permet aux plugins d'interagir avec les ressources du jeu (modèles, sons, textures, etc.) et de les gérer.
- **Retourne :** L'instance de l'`AssetRegistry`.

### `public <T, C extends Codec<? extends T>> CodecMapRegistry<T, C> getCodecRegistry(@Nonnull StringCodecMapCodec<T, C> mapCodec)`
Récupère un `CodecMapRegistry` pour un `StringCodecMapCodec` donné. C'est une méthode générique pour interagir avec différents registres basés sur des "codecs" (des systèmes pour convertir des données vers et depuis un format spécifique, comme JSON).
- **Paramètres :**
    - `mapCodec` : Le `StringCodecMapCodec` à utiliser pour récupérer le registre.
- **Retourne :** Une instance de `CodecMapRegistry`.

### `public <K, T extends com.hypixel.hytale.assetstore.JsonAsset<K>> CodecMapRegistry.Assets<T, ?> getCodecRegistry(@Nonnull AssetCodecMapCodec<K, T> mapCodec)`
Récupère un `CodecMapRegistry` spécifique aux ressources (`Assets`) pour un `AssetCodecMapCodec` donné. Ceci est utilisé pour gérer les registres de ressources basées sur JSON.
- **Paramètres :**
    - `mapCodec` : L'`AssetCodecMapCodec` à utiliser pour récupérer le registre des ressources.
- **Retourne :** Une instance de `CodecMapRegistry.Assets`.

### `public <V> MapKeyMapRegistry<V> getCodecRegistry(@Nonnull MapKeyMapCodec<V> mapCodec)`
Récupère un `MapKeyMapRegistry` pour un `MapKeyMapCodec` donné. Ceci est utilisé pour gérer les registres où les éléments sont identifiés par des clés de carte.
- **Paramètres :**
    - `mapCodec` : Le `MapKeyMapCodec` à utiliser pour récupérer le registre.
- **Retourne :** Une instance de `MapKeyMapRegistry`.

### `public final String getBasePermission()`
Retourne la chaîne de permission de base associée à ce plugin. Elle est généralement dérivée de son groupe et de son nom. Cette chaîne peut être utilisée comme préfixe pour définir des permissions spécifiques au sein de votre plugin (par exemple, "monplugin.admin.commande").
- **Retourne :** La chaîne de permission de base (`String`).

### `public boolean isDisabled()`
Vérifie si le plugin est actuellement dans un état désactivé (`NONE`, `DISABLED` ou `SHUTDOWN`).
- **Retourne :** `true` si le plugin est désactivé, `false` sinon.

### `public boolean isEnabled()`
Vérifie si le plugin est actuellement dans un état activé.
- **Retourne :** `true` si le plugin est activé, `false` sinon.

### `public abstract PluginType getType()`
Une méthode abstraite qui doit être implémentée par les sous-classes (comme `JavaPlugin`) pour retourner le type spécifique du plugin (par exemple, `PluginType.PLUGIN` pour un plugin classique, `PluginType.ASSET_PACK` pour un pack de ressources).
- **Retourne :** Le `PluginType` du plugin.

## Méthodes Protégées (Hooks du Cycle de Vie)

Ces méthodes sont destinées à être surchargées par les sous-classes (`JavaPlugin` ou d'autres implémentations de plugins) pour injecter une logique personnalisée à différentes étapes du cycle de vie du plugin. C'est là que vous écrirez votre code pour initialiser, démarrer et arrêter votre mod.

### `protected final <T> Config<T> withConfig(@Nonnull BuilderCodec<T> configCodec)`
Enregistre un fichier de configuration pour le plugin en utilisant un nom de fichier par défaut. Cette méthode *doit* être appelée avant la phase de configuration (`setup()`) du plugin.
- **Paramètres :**
    - `configCodec` : Un `BuilderCodec` qui définit comment l'objet de configuration est encodé (écrit) et décodé (lu).
- **Retourne :** L'instance de `Config` gérant le fichier de configuration.

### `protected final <T> Config<T> withConfig(@Nonnull String name, @Nonnull BuilderCodec<T> configCodec)`
Enregistre un fichier de configuration pour le plugin avec un nom de fichier personnalisé. Cette méthode *doit* être appelée avant la phase de configuration (`setup()`) du plugin.
- **Paramètres :**
    - `name` : Le nom de fichier pour le fichier de configuration (sans extension, par exemple "mes-options").
    - `configCodec` : Un `BuilderCodec` qui définit comment l'objet de configuration est encodé et décodé.
- **Retourne :** L'instance de `Config` gérant le fichier de configuration.

### `protected void setup()`
Cette méthode est appelée pendant la phase de configuration (`setup`) du plugin, après que le plugin a été chargé mais avant qu'il n'ait démarré. C'est un bon endroit pour les initialisations qui ne dépendent pas du fait que d'autres plugins soient entièrement activés. Par exemple, enregistrer des gestionnaires de commandes.

### `protected void start()`
Cette méthode est appelée lorsque le plugin démarre, après que toutes ses dépendances ont été configurées. C'est généralement ici que les plugins enregistrent leurs écouteurs d'événements, leurs commandes et d'autres fonctionnalités qui doivent être actives pour que le plugin fonctionne. C'est le cœur de votre logique de démarrage.

### `protected void shutdown()`
Cette méthode est appelée lorsque le plugin est en cours d'arrêt. Les plugins doivent utiliser ce "hook" pour effectuer des opérations de nettoyage, sauvegarder des données, désenregistrer des écouteurs et libérer des ressources afin d'éviter les fuites de mémoire.
