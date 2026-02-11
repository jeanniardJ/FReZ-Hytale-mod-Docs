# JavaPlugin

La classe `JavaPlugin` est une classe abstraite de base que les plugins Hytale concrets (vos mods) écrits en Java devraient étendre. Elle fournit des fonctionnalités fondamentales et s'intègre au système de gestion des plugins du serveur. Les développeurs créant de nouveaux mods Hytale construiront généralement une classe qui étend `JavaPlugin` pour définir le comportement de leur mod.

## Constructeur

### `public JavaPlugin(@Nonnull JavaPluginInit init)`
Initialise une nouvelle instance d'un `JavaPlugin`. Ce constructeur est généralement appelé en interne par le système de chargement des plugins et n'est pas censé être appelé directement par votre code de mod.
- **Paramètres :**
    - `init` : Un objet d'initialisation (`JavaPluginInit`) contenant des détails sur le plugin, tels que son chemin de fichier (`Path`) et son chargeur de classes (`PluginClassLoader`).

## Méthodes

### `public Path getFile()`
Récupère le chemin du système de fichiers vers le fichier JAR (`.jar`) à partir duquel ce plugin a été chargé.
- **Retourne :** Un objet `Path` représentant le fichier JAR du plugin.

### `public PluginClassLoader getClassLoader()`
Récupère l'instance du `PluginClassLoader` spécifiquement associée à ce plugin. Ce chargeur de classes est responsable du chargement des classes et des ressources pour ce plugin particulier, en gérant son propre environnement d'exécution.
- **Retourne :** Le `PluginClassLoader` pour ce plugin.

### `public final PluginType getType()`
Retourne le type de ce plugin. Pour les instances de `JavaPlugin`, cela retournera toujours `PluginType.PLUGIN`, indiquant qu'il s'agit d'un plugin classique (par opposition à un pack de ressources, par exemple).
- **Retourne :** Le `PluginType` du plugin.
