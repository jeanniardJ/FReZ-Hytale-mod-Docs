# HytaleServer

La classe `HytaleServer` est le point d'entrée principal et l'orchestrateur central de l'application serveur Hytale. Elle gère le cycle de vie du serveur, incluant le démarrage (boot), la gestion des plugins, la gestion des commandes et l'arrêt (shutdown).

## Constructeur

### `public HytaleServer() throws IOException`
Initialise une nouvelle instance du serveur Hytale. Ce constructeur configure les composants essentiels du serveur, charge la configuration, initialise l'authentification et enregistre les "hooks" d'arrêt (des actions à exécuter avant l'arrêt).

## Méthodes

### `public EventBus getEventBus()`
Récupère l'instance de l'`EventBus` du serveur. L'EventBus est utilisé pour envoyer et s'abonner aux événements du serveur. Pensez-y comme à un système de notification : quand quelque chose se passe (un événement), l'EventBus le diffuse, et d'autres parties du code (vos plugins, par exemple) peuvent "écouter" ces notifications.
- **Retourne :** L'instance de l'`EventBus`.

### `public PluginManager getPluginManager()`
Récupère l'instance du `PluginManager` du serveur. C'est le responsable du chargement, de la gestion et de l'interaction avec tous les plugins (vos mods).
- **Retourne :** L'instance du `PluginManager`.

### `public CommandManager getCommandManager()`
Récupère l'instance du `CommandManager` du serveur. Il est utilisé pour enregistrer et gérer les commandes du serveur (comme `/teleport` ou vos propres commandes personnalisées).
- **Retourne :** L'instance du `CommandManager`.

### `public HytaleServerConfig getConfig()`
Récupère la configuration actuelle du serveur. C'est ici que vous trouverez des paramètres importants comme le nombre maximal de joueurs, etc.
- **Retourne :** L'instance de `HytaleServerConfig`.

### `public void shutdownServer()`
Lance un arrêt ordonné du serveur Hytale avec une raison d'arrêt par défaut. Un arrêt ordonné signifie que le serveur essaie de sauvegarder les données et de fermer les connexions proprement.

### `public void shutdownServer(@Nonnull ShutdownReason reason)`
Lance un arrêt ordonné du serveur Hytale avec une raison spécifique.
- **Paramètres :**
    - `reason` : L'énumération `ShutdownReason` qui indique la raison de l'arrêt du serveur (par exemple, arrêt normal, erreur critique).

### `public void doneSetup(PluginBase plugin)`
Appelée par le `PluginManager` lorsqu'un plugin a terminé sa phase de configuration (setup). C'est une méthode interne.
- **Paramètres :**
    - `plugin` : L'instance de `PluginBase` qui a terminé sa configuration.

### `public void doneStart(PluginBase plugin)`
Appelée par le `PluginManager` lorsqu'un plugin a terminé sa phase de démarrage (start). C'est une méthode interne.
- **Paramètres :**
    - `plugin` : L'instance de `PluginBase` qui a terminé son démarrage.

### `public void doneStop(PluginBase plugin)`
Appelée par le `PluginManager` lorsqu'un plugin a terminé sa phase d'arrêt (stop). C'est une méthode interne.
- **Paramètres :**
    - `plugin` : L'instance de `PluginBase` qui a terminé son arrêt.

### `public void sendSingleplayerProgress()`
Envoie des mises à jour de progression liées au chargement ou à l'arrêt du serveur en mode solo (singleplayer).

### `public String getServerName()`
Récupère le nom configuré du serveur.
- **Retourne :** Le nom du serveur sous forme de `String`.

### `public boolean isBooting()`
Vérifie si le serveur est actuellement en cours de démarrage.
- **Retourne :** `true` si le serveur est en phase de démarrage, `false` sinon.

### `public boolean isBooted()`
Vérifie si le serveur a terminé son processus de démarrage avec succès.
- **Retourne :** `true` si le serveur a démarré, `false` sinon.

### `public boolean isShuttingDown()`
Vérifie si le serveur est actuellement en cours d'arrêt.
- **Retourne :** `true` si le serveur s'arrête, `false` sinon.

### `public Instant getBoot()`
Récupère l'`Instant` (un point dans le temps) où le processus de démarrage du serveur a commencé.
- **Retourne :** Un `Instant` représentant l'heure de démarrage.

### `public long getBootStart()`
Récupère le temps en nanosecondes du système lorsque le processus de démarrage du serveur a commencé.
- **Retourne :** Un `long` représentant le temps de démarrage en nanosecondes.

### `public ShutdownReason getShutdownReason()`
Récupère la raison de l'arrêt du serveur, si le serveur est en cours d'arrêt.
- **Retourne :** L'énumération `ShutdownReason`, ou `null` si le serveur ne s'arrête pas.

### `public void reportSingleplayerStatus(String message)`
Rapporte un message de statut spécifiquement pour le mode solo.
- **Paramètres :**
    - `message` : Le message de statut à rapporter.

### `public void reportSaveProgress(@Nonnull World world, int saved, int total)`
Rapporte la progression des opérations de sauvegarde du monde, généralement pendant l'arrêt.
- **Paramètres :`
    - `world` : L'instance du `World` en cours de sauvegarde.
    - `saved` : Le nombre de "chunks" (morceaux de monde) sauvegardés jusqu'à présent.
    - `total` : Le nombre total de "chunks" à sauvegarder.

### `public static HytaleServer get()`
Récupère l'instance "singleton" du `HytaleServer`. Un "singleton" est une classe dont il n'existe qu'une seule instance dans toute l'application. C'est la manière principale d'accéder aux fonctionnalités du serveur Hytale.
- **Retourne :** L'instance unique du `HytaleServer`.
