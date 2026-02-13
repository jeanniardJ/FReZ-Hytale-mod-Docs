# HytaleServer API

La classe `HytaleServer` est le point d'entrée principal pour accéder aux gestionnaires de bas niveau du serveur.

## Accès à l'instance

```java
import com.hypixel.hytale.server.core.HytaleServer;

HytaleServer server = HytaleServer.get();
```

## Gestionnaires Principaux

### 1. Configuration (`ServerConfig`)

Accès à la configuration globale du serveur (server.properties).

```java
import com.hypixel.hytale.server.core.config.ServerConfig;

ServerConfig config = server.getConfig();
int maxPlayers = config.getMaxPlayers();
int port = config.getPort();
```

### 2. Gestion des Plugins (`PluginManager`)

Permet d'interagir avec les autres plugins chargés.

```java
import com.hypixel.hytale.server.core.plugin.PluginManager;

PluginManager pluginManager = server.getPluginManager();
// pluginManager.getPlugin("NomDuPlugin");
```

### 3. Gestion des Commandes (`CommandManager`)

Enregistrement et gestion des commandes.
> Note : Pour enregistrer des commandes globales, préférez utiliser `HytaleServer.get().getCommandManager().register(...)`. La méthode `getCommandRegistry()` de votre plugin est pour un usage interne ou pour des commandes de plus bas niveau.

```java
import com.hypixel.hytale.server.core.command.CommandManager;

CommandManager commandManager = server.getCommandManager();
```

### 4. Gestion des Événements (`EventBus`)

Le bus d'événements central.
> Note : Préférez utiliser `getEventRegistry()` dans votre classe `JavaPlugin`.

```java
import com.hypixel.hytale.event.EventBus;

EventBus eventBus = server.getEventBus();
```

### 5. Planificateur de Tâches (`Scheduler`)

Pour exécuter des tâches différées ou répétitives.

```java
import java.util.concurrent.ScheduledExecutorService;

ScheduledExecutorService scheduler = HytaleServer.SCHEDULED_EXECUTOR;
```

## Arrêt du Serveur

```java
// Arrête proprement le serveur
server.shutdown();
```
