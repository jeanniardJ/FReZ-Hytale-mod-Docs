# 🤖 Le Système de Commandes

Ce guide explique comment créer vos propres commandes de chat pour votre serveur Hytale, de la création de la classe à son enregistrement.

---

## 1. Les Concepts Clés

Pour créer une commande, vous devez connaître trois éléments principaux :

*   **`AbstractCommand`**: C'est la classe "modèle" que votre propre commande devra étendre. C'est ici que vous définirez son nom, ses arguments et sa logique.
*   **`CommandSender`**: C'est une interface qui représente **qui** a exécuté la commande. Ce peut être un joueur (`Player`) ou la console du serveur. Elle vous permet de renvoyer des messages et de vérifier des permissions.
*   **`CommandRegistry`**: C'est le "registre" du serveur. Une fois votre commande créée, vous devez l'y "enregistrer" pour que le serveur la reconnaisse.

---

## 2. Créer une Commande : Exemple Complet `/heal`

Nous allons créer une commande `/heal [quantité]` qui restaure la vie du joueur qui l'exécute.

### Étape 1 : Créer la classe de la commande
Créez une nouvelle classe qui hérite de `AbstractCommand`.

```java
// Fichier : com/monplugin/commands/HealCommand.java
package com.monplugin.commands;

import com.hypixel.hytale.server.core.command.system.AbstractCommand;
import com.hypixel.hytale.server.core.command.system.CommandContext;
import com.hypixel.hytale.server.core.command.system.CommandSender;
import com.hypixel.hytale.server.core.command.system.arguments.types.IntegerArgumentType;
import com.hypixel.hytale.server.core.entity.LivingEntity;
import com.hypixel.hytale.server.core.entity.entities.Player;
import com.hypixel.hytale.server.core.Message;

import java.util.concurrent.CompletableFuture;
import javax.annotation.Nonnull;

public class HealCommand extends AbstractCommand {

    // ÉTAPE 1 : Le constructeur, où l'on définit la commande
    public HealCommand() {
        // Nom de la commande, et sa description (affichée dans /help)
        super("heal", "Restaure la vie du joueur.");

        // Définir la permission nécessaire pour utiliser cette commande
        requirePermission("monplugin.commands.heal");

        // Ajouter un argument optionnel pour la quantité de vie à restaurer
        withOptionalArg(
            "quantité", // nom de l'argument
            "La quantité de points de vie à restaurer. Par défaut, toute la vie.", // description
            new IntegerArgumentType() // le type de l'argument
        );
    }

    // ÉTAPE 2 : La logique de la commande
    @Override
    protected CompletableFuture<Void> execute(@Nonnull CommandContext context) {
        // On récupère qui a exécuté la commande
        CommandSender sender = context.sender();

        // On vérifie si c'est bien un joueur (et pas la console)
        if (!(sender instanceof Player)) {
            sender.sendMessage(Message.raw("Cette commande ne peut être utilisée que par un joueur."));
            return CompletableFuture.completedFuture(null);
        }

        Player player = (Player) sender;

        // On récupère la valeur de l'argument optionnel "quantité"
        // Si l'argument n'est pas fourni, 'getArg' renverra null.
        Integer amountToHeal = context.getArg("quantité");

        if (amountToHeal != null) {
            // Si une quantité est spécifiée
            float currentHealth = player.getHealth(); // Supposons que getHealth() existe sur LivingEntity
            player.setHealth(currentHealth + amountToHeal);
            sender.sendMessage(Message.raw("Vous avez été soigné de " + amountToHeal + " points de vie !"));
        } else {
            // Si aucune quantité n'est spécifiée, on restaure toute la vie
            player.setHealth(player.getMaxHealth()); // Supposons que getMaxHealth() existe
            sender.sendMessage(Message.raw("Vous avez été entièrement soigné !"));
        }

        // On signale que la commande est terminée
        return CompletableFuture.completedFuture(null);
    }
}
```

### Étape 2 : Enregistrer la commande
Maintenant que la classe est prête, il faut l'enregistrer auprès du serveur. Cela se fait dans la méthode `onEnable()` de votre plugin principal.

```java
// Fichier : com/monplugin/MonPlugin.java
package com.monplugin;

import com.hypixel.hytale.server.api.Plugin;
import com.hypixel.hytale.server.api.HytaleServer;
import com.hypixel.hytale.server.core.command.system.CommandRegistry;
import com.monplugin.commands.HealCommand;

public class MonPlugin implements Plugin {

    @Override
    public void onEnable() {
        // 1. Obtenir le registre des commandes du serveur
        CommandRegistry commandRegistry = HytaleServer.getInstance().getCommandRegistry();

        // 2. Enregistrer une nouvelle instance de notre commande
        commandRegistry.registerCommand(new HealCommand());
        
        HytaleServer.getLogger().info("La commande /heal a été enregistrée.");
    }
    
    // ... onDisable ...
}
```

Votre commande `/heal` est maintenant fonctionnelle en jeu !

---

## 3. Types d'Arguments

Vous pouvez définir plusieurs types d'arguments pour vos commandes :

*   **`withRequiredArg("nom", "desc", new StringArgumentType())`**: Argument obligatoire. La commande échouera s'il n'est pas fourni.
*   **`withOptionalArg("nom", "desc", new IntegerArgumentType())`**: Argument optionnel. Si non fourni, `context.getArg("nom")` renverra `null`.
*   **`withFlagArg("nom", "desc")`**: Un argument "drapeau" (flag), qui n'a pas de valeur. Sa simple présence est sa valeur. `context.hasFlag("nom")` renverra `true` ou `false`. Exemple : `/command --force`.

Hytale fournit de nombreux `ArgumentType` prêts à l'emploi :
*   `StringArgumentType`
*   `IntegerArgumentType`
*   `FloatArgumentType`
*   `PlayerArgumentType` (pour cibler d'autres joueurs)
*   ...et bien d'autres dans le package `com.hypixel.hytale.server.core.command.system.arguments.types`.
