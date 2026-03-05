# ⚡ Le Système d'Événements (Event System)

Le système d'événements est l'un des concepts les plus importants dans le développement de mods pour Hytale. Il vous permet de "réagir" à des actions qui se produisent dans le jeu, comme la connexion d'un joueur, la destruction d'un bloc ou l'attaque d'une entité.

Ce système favorise un code propre et découplé : au lieu de modifier directement le code du jeu (ce qui est impossible et serait une mauvaise pratique), vous "écoutez" les événements qui vous intéressent et y ajoutez votre propre logique.

---

## 1. Les Concepts Clés

### a. L'Événement (`IEvent`)
Un **événement** est un simple objet Java qui représente une action qui vient de se produire (ou est sur le point de se produire). Chaque type d'action a sa propre classe d'événement.

*   `PlayerJoinEvent` : Se déclenche quand un joueur rejoint le serveur.
*   `BlockBreakEvent` : Se déclenche quand un joueur casse un bloc.
*   `EntityDeathEvent` : Se déclenche quand une entité meurt.

Tous les événements implémentent une interface de base, `IEvent`.

### b. L'EventBus
L'**EventBus** est le cœur du système. C'est un canal central où tous les événements du jeu sont publiés. Son rôle est simple : recevoir un événement et le distribuer à tous ceux qui se sont "abonnés" pour l'écouter.

*[Schéma du flux d'événements à insérer ici]*

### c. Le Listener (ou "Consumer")
Un **listener** (écouteur) est une fonction ou une classe que vous écrivez pour traiter un événement spécifique. C'est là que se trouve votre logique. Par exemple, pour un `PlayerJoinEvent`, votre listener pourrait envoyer un message de bienvenue au joueur.

---

## 2. Comment Écouter un Événement : Exemple Complet

Voici comment écouter le `PlayerJoinEvent` pour envoyer un message de bienvenue.

### Étape 1 : Créer une classe pour vos Listeners
Il est recommandé de regrouper vos listeners dans une ou plusieurs classes dédiées.

```java
// Fichier : com/monplugin/listeners/PlayerConnectionListener.java

package com.monplugin.listeners;

import com.hypixel.hytale.server.api.HytaleServer;
import com.hypixel.hytale.server.api.event.player.PlayerJoinEvent;
import com.hypixel.hytale.server.api.player.HytalePlayer;
import com.hypixel.hytale.text.HytaleText;
import org.apache.logging.log4j.Logger;

public class PlayerConnectionListener {

    private static final Logger LOGGER = HytaleServer.getLogger();

    /**
     * Cette méthode sera appelée à chaque fois qu'un PlayerJoinEvent est déclenché.
     * @param event L'objet événement, contenant les informations sur l'action.
     */
    public void onPlayerJoin(PlayerJoinEvent event) {
        // 1. Récupérer le joueur depuis l'événement
        HytalePlayer player = event.getPlayer();

        // 2. Envoyer un message de bienvenue au joueur
        player.sendMessage(HytaleText.builder().append("Bienvenue sur notre serveur, " + player.getName() + " !").build());

        // 3. Logger l'information dans la console du serveur (bonne pratique)
        LOGGER.info("Le joueur {} a rejoint le serveur.", player.getName());
    }
}
```

### Étape 2 : Enregistrer le Listener auprès de l'EventBus
Maintenant que votre méthode `onPlayerJoin` est prête, il faut dire à l'EventBus de l'appeler quand un `PlayerJoinEvent` se produit. Cela se fait généralement dans la classe principale de votre plugin.

```java
// Fichier : com/monplugin/MonPlugin.java

package com.monplugin;

import com.hypixel.hytale.server.api.HytaleServer;
import com.hypixel.hytale.server.api.Plugin;
import com.hypixel.hytale.event.EventBus;
import com.hypixel.hytale.server.api.event.player.PlayerJoinEvent;

import com.monplugin.listeners.PlayerConnectionListener;

public class MonPlugin implements Plugin {

    private PlayerConnectionListener connectionListener;

    @Override
    public void onEnable() {
        // 1. Créer une instance de notre classe de listeners
        this.connectionListener = new PlayerConnectionListener();

        // 2. Obtenir l'instance de l'EventBus du serveur
        EventBus eventBus = HytaleServer.getInstance().getEventBus();

        // 3. Enregistrer notre méthode comme listener pour PlayerJoinEvent
        //    - Le premier argument est la classe de l'événement à écouter.
        //    - Le second est une référence à la méthode qui doit être appelée.
        eventBus.register(PlayerJoinEvent.class, this.connectionListener::onPlayerJoin);
        
        HytaleServer.getLogger().info("MonPlugin a été activé et écoute les connexions !");
    }

    @Override
    public void onDisable() {
        // Idéalement, il faudrait ici annuler l'enregistrement,
        // surtout si votre listener est complexe.
        HytaleServer.getLogger().info("MonPlugin a été désactivé.");
    }
}
```

Et c'est tout ! Chaque fois qu'un joueur rejoindra le serveur, votre message de bienvenue sera envoyé.

---

## 3. Priorité d'Exécution (`EventPriority`)

Que se passe-t-il si plusieurs plugins écoutent le même événement ? C'est là que la **priorité** intervient. Vous pouvez spécifier une priorité pour que votre listener s'exécute avant ou après les autres.

Les priorités disponibles sont (de la première à la dernière exécutée) :
*   `HIGHEST`
*   `HIGH`
*   `NORMAL` (par défaut)
*   `LOW`
*   `LOWEST`

**Cas d'usage :** Un plugin de protection de zone (comme un LandClaim) doit absolument vérifier si un joueur a le droit de casser un bloc **avant** qu'un autre plugin ne donne une récompense pour l'avoir cassé. Le plugin de protection utilisera donc une priorité `HIGH` ou `HIGHEST`.

```java
// Enregistrement avec une priorité HAUTE
eventBus.register(
    EventPriority.HIGH, 
    BlockBreakEvent.class, 
    this.myListener::onBlockBreak
);
```

---

## 4. Annuler un Événement (`ICancellable`)

Certains événements peuvent être "annulés". Si un événement implémente l'interface `ICancellable`, cela signifie que vous pouvez empêcher l'action associée de se produire.

**Cas d'usage :** Empêcher un joueur de casser un bloc de bedrock.

```java
// Dans votre classe de listener...

public void onBlockBreak(BlockBreakEvent event) {
    // Vérifier si le bloc est de la bedrock
    if (event.getBlock().getType().equals("hytale:bedrock")) {
        // 1. Annuler l'événement. Le bloc ne se cassera pas.
        event.setCancelled(true);

        // 2. (Optionnel) Informer le joueur.
        event.getPlayer().sendMessage(HytaleText.builder().color(HytaleText.Color.RED).append("Vous ne pouvez pas casser ce bloc !").build());
    }
}
```

**Bonne pratique :** Si votre listener a une priorité basse (`LOW` ou `LOWEST`), vérifiez toujours si l'événement n'a pas déjà été annulé par un autre plugin avant d'exécuter votre logique.

```java
public void onBlockBreak(BlockBreakEvent event) {
    // Si un plugin avec une priorité plus élevée a déjà annulé l'événement, on ne fait rien.
    if (event.isCancelled()) {
        return;
    }

    // ... votre logique ici ...
}
```
