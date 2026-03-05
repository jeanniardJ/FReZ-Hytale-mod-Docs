# 🌍 L'API du Monde (World & Universe)

Ce guide explique comment interagir avec le monde de Hytale au niveau du serveur. Il couvre les deux classes centrales : `Universe`, qui gère l'ensemble des mondes, et `World`, qui représente un monde de jeu spécifique.

---

## 1. Comprendre `Universe` et `World`

Il faut voir ces deux classes comme des poupées russes :

*   **`Universe`**: C'est le conteneur le plus externe. Il n'y en a qu'un seul par serveur. L'univers contient et gère tous les mondes, tous les joueurs connectés, et la configuration globale. C'est le point d'entrée pour accéder à presque tout.

*   **`World`**: C'est un monde de jeu individuel (une dimension, une carte...). Un serveur peut avoir plusieurs mondes (un monde principal, un monde minage, un donjon instancié...). C'est sur l'objet `World` que vous effectuerez la plupart des opérations concrètes : modifier des blocs, faire apparaître des entités, etc.

---

## 2. Accéder à l'Univers et aux Mondes

### a. Obtenir l'instance de l'Univers

`Universe` est un singleton. Vous pouvez y accéder de manière statique de n'importe où dans votre code.

```java
import com.hypixel.hytale.server.core.universe.Universe;

// Obtenir l'instance unique de l'Univers
Universe universe = Universe.get();
```

### b. Obtenir un Monde Spécifique

Une fois que vous avez l'Univers, vous pouvez obtenir un objet `World`.

```java
import com.hypixel.hytale.server.core.universe.world.World;

// Obtenir le monde par défaut du serveur
World defaultWorld = universe.getDefaultWorld();

// Obtenir un monde par son nom (insensible à la casse)
World miningWorld = universe.getWorld("monde_minage");

if (miningWorld != null) {
    // Le monde a été trouvé, on peut travailler avec.
}

// Obtenir une liste de tous les mondes chargés
Map<String, World> allWorlds = universe.getWorlds();
```

---

## 3. Utiliser l'objet `World`

> **Note :** Les exemples de code ci-dessous sont basés sur les classes utilitaires trouvées dans l'API et les conventions habituelles. Les noms exacts des méthodes de la classe `World` peuvent légèrement varier.

L'objet `World` est votre porte d'entrée pour manipuler l'environnement de jeu.

### a. Manipuler les Blocs

Pour lire ou modifier des blocs, vous utiliserez probablement des méthodes directement sur l'objet `World`.

```java
import com.hypixel.hytale.math.vector.Vector3i;

Vector3i blockPosition = new Vector3i(100, 50, -200);

// Lire le type d'un bloc (le nom exact de la méthode peut varier)
// BlockType blockType = world.getBlockTypeAt(blockPosition);

// Modifier un bloc (le nom exact de la méthode peut varier)
// world.setBlock("hytale:stone", blockPosition); 
```

### b. Gérer les Entités

Vous pouvez faire apparaître de nouvelles entités ou obtenir la liste de celles déjà présentes. La classe `SpawnUtil` est probablement utilisée pour cela.

```java
import com.hypixel.hytale.server.core.universe.world.SpawnUtil;
import com.hypixel.hytale.server.core.entity.Entity; // Hypothetical
import com.hypixel.hytale.math.vector.Vector3d;

// Faire apparaître une entité (le nom exact de la méthode peut varier)
// Le premier argument est souvent un "asset" qui définit le type d'entité.
// SpawnUtil.spawnEntity("hytale:creeper", world, new Vector3d(105.5, 51, -198.5));

// Obtenir la liste des joueurs dans un monde
// Note : world.getPlayers() renvoie probablement des PlayerRef (dépréciés).
// Il est préférable de passer par l'Univers et de filtrer par monde.
for (PlayerRef playerRef : Universe.get().getPlayers()) {
    if (playerRef.getWorldUuid().equals(world.getUuid())) {
        // Ce joueur est dans ce monde.
    }
}
```

### c. Jouer des Sons

La classe `SoundUtil` suggère qu'il est facile de jouer des sons pour les joueurs dans un monde.

```java
import com.hypixel.hytale.server.core.universe.world.SoundUtil;
import com.hypixel.hytale.math.vector.Vector3d;

// Jouer un son à une position 3D pour tous les joueurs à proximité
SoundUtil.playSoundEvent3d(
    world, 
    "hytale:sfx.magic.cast", // Le nom de l'effet sonore
    new Vector3d(100, 51, -200)
);
```

### d. Obtenir la configuration du monde

Chaque monde a sa propre configuration, que vous pouvez lire.

```java
import com.hypixel.hytale.server.core.universe.world.WorldConfig;

WorldConfig config = world.getWorldConfig();

// Exemple : Obtenir le GameMode par défaut du monde
GameMode defaultGameMode = config.getGameMode();
```

---
## 4. L'Univers en tant que Gestionnaire de Joueurs

L'Univers gère également la liste globale des joueurs.

> ⚠️ **Attention :** Les méthodes comme `Universe.get().getPlayer(uuid)` ou `getPlayers()` renvoient des objets `PlayerRef`, qui sont **dépréciés**. Pour interagir avec un joueur, vous devez toujours essayer d'obtenir le **composant `Player`** via le système d'événements, comme expliqué dans [la documentation de l'API Joueur](entity/player_api.md).

```java
// Approche dépréciée, à éviter dans du nouveau code !
PlayerRef playerRef = Universe.get().getPlayer(someUuid);
if (playerRef != null) {
    playerRef.sendMessage(Message.raw("Ceci est une méthode dépréciée !"));
}
```
