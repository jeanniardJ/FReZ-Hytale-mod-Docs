# Documentation de l'API de FReZ-Hytale-mod-CoreRolePlay

Ce document fournit une vue d'ensemble détaillée de l'API publique (CoreRolePlayAPI) du plugin FReZ-Hytale-mod-CoreRolePlay, expliquant son objectif, ses composants clés et des exemples d'utilisation.

---

## 1. Vue d'ensemble du Plugin CoreRolePlay

Le plugin `FReZ-Hytale-mod-CoreRolePlay` est un module fondamental de la suite FReZ pour Hytale. Son rôle principal est de gérer l'identité de base des joueurs (UUID, nom) et d'orchestrer les événements essentiels de leur cycle de vie (connexion, déconnexion). Il fournit une fondation stable et réutilisable pour tous les autres plugins de gameplay qui nécessitent des informations essentielles sur les joueurs, tout en respectant l'architecture ECS.

L'objectif de cette API est de centraliser l'accès aux données joueur et de servir de façade pour faciliter l'intégration avec d'autres modules de la suite FReZ.

## 2. Accéder à l'API Publique (`CoreRolePlayAPI`)

L'API de CoreRolePlay est exposée via une interface singleton statique, permettant aux autres plugins d'y accéder facilement et de manière cohérente.

```java
import com.jjeanniard.CoreRolePlay;
import com.jjeanniard.CoreRolePlayAPI;
import com.jjeanniard.HytalePlayer;

// Obtenez l'instance de l'API
CoreRolePlayAPI api = CoreRolePlay.getAPI();

if (api == null) {
    // L'API n'est pas encore initialisée ou le plugin n'est pas activé.
    // Cela ne devrait pas arriver si les dépendances de plugins sont correctement configurées.
    return;
}
```

## 3. L'Interface `HytalePlayer`

L'interface `HytalePlayer` représente une abstraction d'un joueur Hytale dans le contexte de l'API CoreRolePlay. Elle fournit l'accès à ses identifiants de base et à son état d'activité.

### 3.1. Accéder à un `HytalePlayer`

Pour récupérer un objet `HytalePlayer` pour un joueur spécifique (généralement actif), vous utilisez la méthode `getPlayer()` de l'API :

```java
import java.util.Optional;
import java.util.UUID;

// Supposons que vous avez l'UUID d'un joueur, par exemple depuis un événement.
UUID playerUUID = /* ... */;

Optional<HytalePlayer> hytalePlayerOptional = api.getPlayer(playerUUID);

hytalePlayerOptional.ifPresent(hytalePlayer -> {
    // Le joueur existe et est actuellement actif (en ligne et géré par le plugin).
    String playerName = hytalePlayer.getName();
    UUID uuid = hytalePlayer.getUUID();
    boolean isOnline = hytalePlayer.isOnline(); // Vérifie si le joueur est actif selon l'API

    Log.info(String.format("Informations du joueur: %s (%s), En ligne: %b", playerName, uuid, isOnline));

    // Des exemples d'accès futurs aux APIs d'autres plugins via hytalePlayer :
    // try {
    //     EconomyAPI economyAPI = hytalePlayer.getEconomy();
    //     double balance = economyAPI.getBalance();
    //     Log.info(String.format("Solde de %s: %f", playerName, balance));
    // } catch (UnsupportedOperationException e) {
    //     Log.warn("Le plugin Economy n'est pas intégré ou activé pour ce joueur.");
    // }
});
```

### 3.2. Méthodes de `HytalePlayer`

*   `UUID getUUID()`: Retourne l'identifiant unique universel du joueur.
*   `String getName()`: Retourne le nom d'utilisateur actuel du joueur.
*   `boolean isOnline()`: Retourne `true` si le joueur est actuellement actif (en ligne et géré par le plugin CoreRolePlay), `false` sinon.

## 4. Gestion du Cycle de Vie des Joueurs (`PlayerLifecycleListener`)

Le `PlayerLifecycleListener` est responsable d'intégrer l'API CoreRolePlay avec les événements de connexion et de déconnexion du serveur Hytale.

*   Lors de la connexion d'un joueur (`PlayerConnectEvent`), le listener s'assure qu'un `PlayerComponent` est attaché au joueur et enregistre ensuite le joueur comme "actif" dans l'API CoreRolePlay via `apiInstance.addActivePlayer()`.
*   Lors de la déconnexion d'un joueur (`PlayerDisconnectEvent`), le listener supprime le joueur de la liste des joueurs actifs de l'API via `apiInstance.removeActivePlayer()`.

Cette intégration garantit que l'API CoreRolePlay maintient un état précis des joueurs actuellement en ligne et gérés par le plugin.

## 5. Extensibilité Future

L'interface `HytalePlayer` est conçue pour être extensible. À l'avenir, des méthodes pourront y être ajoutées pour fournir un accès direct aux APIs d'autres plugins (par exemple, `getEconomy()`, `getPermissions()`, `getProfessions()`). Cette approche permettra de créer une "façade joueur" complète, où un développeur pourra interagir avec toutes les facettes d'un joueur via un seul objet `HytalePlayer`, simplifiant ainsi le développement de gameplay inter-modules. L'intégration de ces APIs nécessitera que les plugins respectifs s'enregistrent auprès de l'API CoreRolePlay.
