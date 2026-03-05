# 👤 L'API du Joueur (Player API)

Ce document est le guide de référence pour interagir avec un joueur (`Player`) côté serveur. Il est destiné à un développeur junior et se concentre sur l'approche moderne de l'API Hytale.

---

## 1. `Player` vs `PlayerRef` : Lequel utiliser ?

En explorant le code, vous trouverez deux classes qui semblent représenter un joueur : `Player` et `PlayerRef`.

*   **`Player`**: C'est un **Composant** de l'architecture ECS. C'est la classe **moderne et recommandée** pour contenir les données et la logique métier liées à un joueur (GameMode, permissions, inventaire...).
*   **`PlayerRef`**: C'est une ancienne référence à une entité joueur. Elle est maintenant **dépréciée (`@Deprecated`)**. Vous ne devriez **plus l'utiliser** dans du nouveau code. Elle existe encore pour des raisons de compatibilité interne.

**Règle d'or : Toujours utiliser le composant `Player` pour interagir avec un joueur.**

---

## 2. Comment obtenir le composant `Player` ?

Le composant `Player` est attaché à l'entité du joueur. La façon la plus courante de l'obtenir est via un événement qui concerne ce joueur.

### Exemple : Depuis un `PlayerJoinEvent`

```java
import com.hypixel.hytale.server.api.event.player.PlayerJoinEvent;
import com.hypixel.hytale.server.core.entity.entities.Player;
import com.hypixel.hytale.component.Holder;

public void onPlayerJoin(PlayerJoinEvent event) {
    // 1. L'événement vous donne un "Holder", qui est un conteneur pour une entité et ses composants.
    Holder<EntityStore> playerHolder = event.getPlayerHolder();

    // 2. Utilisez getComponent() avec le "ComponentType" de la classe Player pour récupérer le composant.
    Player playerComponent = playerHolder.getComponent(Player.getComponentType());

    // 3. Vérifiez toujours si le composant n'est pas null avant de l'utiliser.
    if (playerComponent != null) {
        // Vous pouvez maintenant interagir avec le joueur !
        playerComponent.sendMessage(Message.raw("Bienvenue ! Le composant Player a été trouvé."));
    }
}
```

---

## 3. Actions courantes sur un joueur

Voici les opérations les plus fréquentes que vous effectuerez sur un `Player`.

### a. Envoyer un message

La méthode `sendMessage` permet d'envoyer un message texte au joueur.
> Pour construire des messages complexes avec des couleurs et des traductions, consultez la documentation sur les [Messages](../ui/message.md).

```java
import com.hypixel.hytale.server.core.Message;

// Envoi d'un message simple
playerComponent.sendMessage(Message.raw("Ceci est un message simple."));

// Envoi d'un message avec une couleur
playerComponent.sendMessage(
    Message.builder()
        .append("Attention ! Message important.")
        .color("#FF5555") // Code couleur hexadécimal
        .build()
);
```

### b. Gérer le GameMode

Vous pouvez facilement lire ou modifier le mode de jeu d'un joueur.

```java
import com.hypixel.hytale.protocol.GameMode;
import com.hypixel.hytale.component.Ref; // Nécessaire pour setGameMode

// Lire le GameMode actuel
GameMode currentMode = playerComponent.getGameMode(); // Renvoie GameMode.Creative, GameMode.Survival, etc.

// Changer le GameMode
// Note : Le changement de GameMode est une action plus complexe qui nécessite
// une "Ref" à l'entité et un "ComponentAccessor" (généralement disponible dans un système ECS).
Ref<EntityStore> playerRef = playerHolder.getRef();
ComponentAccessor<EntityStore> accessor = playerRef.getStore();

Player.setGameMode(playerRef, GameMode.Creative, accessor);
LOGGER.info("Le GameMode de {} est maintenant Creative.", playerComponent.getDisplayName());
```

### c. Vérifier les permissions

La classe `Player` implémente `PermissionHolder`, ce qui vous permet de vérifier les permissions directement.

```java
if (playerComponent.hasPermission("frez.commands.admin")) {
    playerComponent.sendMessage(Message.raw("Vous avez les permissions d'admin."));
} else {
    playerComponent.sendMessage(Message.raw("Action refusée. Permissions insuffisantes."));
}
```

### d. Interagir avec l'inventaire

Vous pouvez accéder à l'inventaire du joueur pour y lire, ajouter ou retirer des objets.

```java
import com.hypixel.hytale.server.core.inventory.Inventory;
import com.hypixel.hytale.server.core.inventory.ItemStack;

// Obtenir l'inventaire
Inventory inventory = playerComponent.getInventory();

// Ajouter un objet (exemple simple)
// La création d'ItemStack est plus complexe et dépend des assets du jeu.
ItemStack myItem = new ItemStack(...);
inventory.addItem(myItem);

// Envoyer les mises à jour de l'inventaire au client
playerComponent.sendInventory();
```

---

## 4. Les "Managers" du Joueur

Le composant `Player` délègue certaines de ses responsabilités à des classes "Manager" spécialisées. Vous pouvez y accéder directement depuis une instance de `Player`.

*   **`getWindowManager()`**: Gère les fenêtres d'UI ouvertes par le joueur.
*   **`getPageManager()`**: Gère les pages d'UI comme le livre de quêtes.
*   **`getHudManager()`**: Contrôle les éléments affichés sur le HUD (barre de vie, etc.).
*   **`getHotbarManager()`**: Gère la barre d'accès rapide du joueur.

```java
// Exemple : Accéder au WindowManager
playerComponent.getWindowManager().closeAll(); // Ferme toutes les fenêtres ouvertes
```

---
## 5. Bonnes Pratiques

*   **Préférez `Player` à `PlayerRef`** dans tout nouveau code.
*   **Vérifiez les nulls :** Lorsque vous récupérez un composant, vérifiez toujours s'il n'est pas `null`.
*   **Utilisez les événements :** Ne modifiez pas l'état d'un joueur directement sans raison. Passez par des événements pour garder un code propre et prévisible.
