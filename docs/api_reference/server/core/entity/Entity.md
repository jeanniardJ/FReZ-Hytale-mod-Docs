# 🐾 L'API des Entités (Entity)

Ce guide explique comment fonctionnent les entités dans Hytale et comment interagir avec elles en respectant l'architecture du jeu.

## 1. Qu'est-ce qu'une Entité ?

Dans Hytale, une **entité** représente tout "objet" qui existe dans le monde et qui n'est pas un bloc statique. Cela inclut :
*   Les joueurs
*   Les PNJ (Personnages Non Joueurs) et les monstres
*   Les objets lâchés au sol
*   Les projectiles (flèches, boules de feu)

La classe de base pour toutes ces entités est `Entity`.

---

## 2. Le Principe Fondamental : l'Architecture ECS

Le point le plus important à comprendre est que Hytale utilise une architecture **ECS (Entité-Composant-Système)**.

*   **Entité (`Entity`)**: Ce n'est qu'un **identifiant**. Une coquille vide.
*   **Composant (`Component`)**: C'est un **bloc de données**. La position, la vie, l'inventaire... tout est un composant attaché à une entité.
*   **Système (`System`)**: C'est la **logique**. Un système prend toutes les entités qui ont un certain type de composant et effectue des opérations (ex: le système de physique fait bouger les entités ayant un composant `Velocity`).

C'est pour cela que de nombreuses méthodes sur la classe `Entity` sont marquées comme **`@Deprecated`** (dépréciées). Il ne faut pas les utiliser ! L'approche moderne est de ne jamais modifier l'entité directement, mais de **lire et modifier ses composants**.

---

## 3. Hiérarchie : `Entity` vs `LivingEntity`

Il existe une hiérarchie simple à connaître :

*   **`Entity`**: La classe de base pour absolument tout.
*   **`LivingEntity`**: Hérite de `Entity`. Elle représente les entités "vivantes" et leur ajoute des fonctionnalités communes :
    *   Un inventaire (`Inventory`)
    *   Des statistiques (`StatModifiersManager` pour la vie, la vitesse, etc.)
    *   La capacité de respirer, de subir des dégâts de chute, etc.

Un `Player` ou un `Creeper` sont des `LivingEntity`, mais un objet au sol est une simple `Entity`.

---

## 4. Comment Interagir avec une Entité

La bonne méthode consiste toujours à :
1.  Obtenir une référence à l'entité (souvent via un événement).
2.  Demander à voir l'un de ses composants.
3.  Lire ou modifier les données de ce composant.

### Exemple : Déplacer une entité

Voici comment déplacer une entité à une nouvelle position en modifiant son `TransformComponent`.

```java
import com.hypixel.hytale.server.api.event.entity.EntityDeathEvent;
import com.hypixel.hytale.component.Holder;
import com.hypixel.hytale.server.core.universe.world.storage.EntityStore;
import com.hypixel.hytale.server.core.modules.entity.component.TransformComponent;
import com.hypixel.hytale.math.vector.Vector3d;

// Contexte : dans un listener qui écoute la mort d'une entité.
public void onEntityDeath(EntityDeathEvent event) {
    // 1. Obtenir le "Holder" de l'entité depuis l'événement.
    Holder<EntityStore> entityHolder = event.getHolder();

    // 2. Récupérer le composant qui gère la position et la rotation.
    TransformComponent transform = entityHolder.getComponent(TransformComponent.getComponentType());

    if (transform != null) {
        // 3. Lire la position actuelle
        Vector3d currentPosition = transform.getPosition();
        LOGGER.info("L'entité est morte à la position : {}", currentPosition);

        // 4. Modifier la position (par exemple, pour la faire réapparaître plus haut)
        // Note : dans la pratique, on ne téléporte pas une entité morte,
        // mais c'est pour l'exemple de modification.
        transform.getPosition().set(currentPosition.getX(), currentPosition.getY() + 10, currentPosition.getZ());
        LOGGER.info("L'entité a été déplacée à {}", transform.getPosition());
    }
}
```

### Exemple : Accéder à l'inventaire d'une `LivingEntity`

```java
import com.hypixel.hytale.server.core.entity.LivingEntity;
import com.hypixel.hytale.server.core.inventory.Inventory;

// Supposons que 'entityHolder' contienne une LivingEntity (ex: un PNJ).

// On récupère d'abord le composant principal (ex: 'PnjComponent')
PnjComponent pnj = entityHolder.getComponent(PnjComponent.getComponentType());

// La classe PnjComponent hérite probablement de LivingEntity.
if (pnj instanceof LivingEntity) {
    LivingEntity livingEntity = (LivingEntity) pnj;

    // On peut maintenant accéder à son inventaire.
    Inventory inventory = livingEntity.getInventory();
    
    // ... et le manipuler.
}
```

---

## 5. Méthodes Utiles (Approche Moderne)

Au lieu d'utiliser les méthodes dépréciées, voici les méthodes importantes :

### `remove()`
Marque l'entité pour qu'elle soit supprimée du monde au prochain "tick" du serveur. Déclenche un `EntityRemoveEvent`.

### `getWorld()`
Retourne le `World` dans lequel se trouve l'entité. Utile pour effectuer des actions sur le monde environnant.

### `getReference()`
Retourne la `Ref<EntityStore>`, une référence technique à l'entité dans la base de données ECS. Utile pour des opérations avancées ou pour passer en paramètre à d'autres API.

### `clone()` et `toHolder()`
Permettent de créer des copies ou des représentations de l'entité, utiles pour la sérialisation ou la création de "snapshots".

---
*Le reste de ce document est conservé pour référence, mais rappelez-vous de toujours privilégier l'interaction avec les **composants** plutôt qu'avec l'objet `Entity` lui-même.*
---
## Méthodes Dépréciées (À NE PAS UTILISER)

Les méthodes suivantes sont dépréciées et ne devraient pas être utilisées. Elles sont listées ici pour information.

*   `getUuid()`: Utilisez le `UUIDComponent`.
*   `getTransformComponent()`: Utilisez `holder.getComponent(TransformComponent.getComponentType())`.
*   `moveTo(...)`: Modifiez directement la position dans le `TransformComponent`.
*   ...et bien d'autres. Si une méthode est marquée `@Deprecated`, cherchez une alternative basée sur les composants.
