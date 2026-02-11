# PluginManager

La classe `PluginManager` est responsable de la gestion du cycle de vie des plugins sur le serveur Hytale. Elle gère le chargement, le démarrage, l'arrêt et le rechargement des plugins, ainsi que leurs dépendances et leurs chargeurs de classes (`classloaders`). Les développeurs de mods interagiront principalement avec ce gestionnaire pour obtenir des informations sur d'autres plugins ou pour enregistrer leurs propres plugins.

## Méthodes Statiques

### `public static PluginManager get()`
Récupère l'instance "singleton" du `PluginManager`. Un "singleton" est une classe dont il n'existe qu'une seule instance dans toute l'application. C'est le moyen principal d'accéder aux fonctionnalités de gestion des plugins.
- **Retourne :** L'instance unique du `PluginManager`.

## Méthodes

### `public void registerCorePlugin(@Nonnull PluginManifest builder)`
Enregistre un plugin "core" (faisant partie du cœur du serveur) auprès du serveur. Les plugins "core" sont généralement des composants internes du serveur empaquetés comme des plugins.
- **Paramètres :**
    - `builder` : Le `PluginManifest` du plugin "core" à enregistrer.

### `public void setup()`
Initialise le gestionnaire de plugins et le prépare au chargement des plugins. Cette méthode est appelée en interne pendant le démarrage du serveur. En tant que développeur de mods, vous n'aurez pas besoin de l'appeler directement.

### `public void start()`
Démarre tous les plugins chargés. Cette méthode est appelée en interne une fois que la configuration du serveur est terminée.

### `public void shutdown()`
Lance le processus d'arrêt pour tous les plugins actifs, leur permettant d'effectuer des opérations de nettoyage. Cette méthode est également appelée en interne pendant l'arrêt du serveur.

### `public PluginState getState()`
Récupère l'état actuel du cycle de vie du gestionnaire de plugins. Cela peut être utile pour savoir si le gestionnaire est en train de se configurer, de démarrer, etc.
- **Retourne :** Une énumération `PluginState` indiquant l'état actuel (par exemple, `NONE`, `SETUP`, `START`, `SHUTDOWN`).

### `public PluginBridgeClassLoader getBridgeClassLoader()`
Récupère le `PluginBridgeClassLoader` utilisé pour le chargement de classes entre plugins. Il s'agit d'une fonctionnalité avancée principalement destinée à gérer la visibilité et l'isolation des classes entre différents plugins.
- **Retourne :** L'instance du `PluginBridgeClassLoader`.

### `public List<PluginBase> getPlugins()`
Retourne une liste immuable (qui ne peut pas être modifiée après sa création) de toutes les instances de `PluginBase` actuellement chargées. Cela vous permet de voir quels plugins sont actifs.
- **Retourne :** Une `List` d'objets `PluginBase`.

### `public PluginBase getPlugin(PluginIdentifier identifier)`
Récupère un plugin spécifique en utilisant son `PluginIdentifier` (son identifiant unique).
- **Paramètres :**
    - `identifier` : L'`PluginIdentifier` unique du plugin à récupérer.
- **Retourne :** L'instance de `PluginBase` si le plugin est trouvé, sinon `null`.

### `public boolean hasPlugin(PluginIdentifier identifier, @Nonnull SemverRange range)`
Vérifie si un plugin avec l'`PluginIdentifier` donné est chargé et si sa version satisfait la `SemverRange` spécifiée. C'est utile pour vérifier les dépendances d'un plugin.
- **Paramètres :**
    - `identifier` : L'`PluginIdentifier` unique du plugin.
    - `range` : La `SemverRange` (plage de versions, par exemple ">=1.0.0 <2.0.0") à vérifier par rapport à la version du plugin.
- **Retourne :** `true` si le plugin est chargé et sa version correspond à la plage, `false` sinon.

### `public boolean reload(@Nonnull PluginIdentifier identifier)`
Tente de décharger puis de recharger un plugin identifié par son `PluginIdentifier`. Cela peut être utile pendant le développement pour appliquer des changements sans redémarrer tout le serveur.
- **Paramètres :**
    - `identifier` : L'`PluginIdentifier` unique du plugin à recharger.
- **Retourne :** `true` si le plugin a été rechargé avec succès, `false` sinon.

### `public boolean unload(@Nonnull PluginIdentifier identifier)`
Tente de décharger un plugin identifié par son `PluginIdentifier`. Cela arrêtera et désactivera le plugin.
- **Paramètres :**
    - `identifier` : L'`PluginIdentifier` unique du plugin à décharger.
- **Retourne :** `true` si le plugin a été déchargé avec succès, `false` sinon.

### `public boolean load(@Nonnull PluginIdentifier identifier)`
Tente de charger et d'activer un plugin identifié par son `PluginIdentifier`.
- **Paramètres :**
    - `identifier` : L'`PluginIdentifier` unique du plugin à charger.
- **Retourne :** `true` si le plugin a été chargé avec succès, `false` sinon.

### `public Map<PluginIdentifier, PluginManifest> getAvailablePlugins()`
Retourne une carte (`Map`) de tous les plugins disponibles qui peuvent être chargés, incluant à la fois les plugins déjà chargés et ceux qui ne le sont pas encore.
- **Retourne :** Une `Map` où les clés sont des `PluginIdentifier` et les valeurs sont des `PluginManifest`s.

### `public ComponentType<EntityStore, PluginListPageManager.SessionSettings> getSessionSettingsComponentType()`
Récupère le `ComponentType` pour les paramètres spécifiques à la session gérés par le `PluginListPageManager`. Il s'agit probablement d'un composant interne pour gérer les paramètres liés à l'interface utilisateur pour les listes de plugins. En tant que développeur de mods, vous n'aurez probablement pas besoin d'interagir directement avec cela.
- **Retourne :** L'instance de `ComponentType`.
